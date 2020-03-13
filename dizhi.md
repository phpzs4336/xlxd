>由于运营部需求，需要将大量的名人信息进行统计，除了统计姓名和出生年月外，还需要统计与生日日期（阳历）相对应的天干地支，生肖，以及对应的农历月份。刚开始50个，由于量不大，部门每人分配几个轻松搞定。十分钟过后，150个，二十分钟后350个。。。再平均分配每人去统计，此时本人是千万只***在心中奔腾而过，如果需要统计成千上万个名人的话， 每个人什么事不用干，专门坐在电脑前百度算了，无疑是浪费时间和精力，而且还耽误工作。不管是软件设计还是代码编写的时候，有一条规则DRY（Don't Repeat Yourself），能重复，反复的东西就有方法去代替。况且只要动动脑，方法总比困难多。 

### 抓取天干地支，生肖和农历月份输出CSV类（可扩展）

```php
class Wnl {

    public $dates = [];

    private $url = 'https://wannianrili.51240.com/';

    public function __construct($dates)
    {
        $this->dates = $dates;
    }

    /**
     * @param $date 1966-09-01
     * @return array
     */
    private function getBranch($date){
        $tmpArray     = explode('-',$date);
        [$year,$month,$day] = $tmpArray;
        $yearAndMonth = $year .'-'.$month;

        $url      = $this->url . "ajax/?q={$yearAndMonth}&v=19021216";
        $day      = $day - 1 ;
        $position = 'id="wnrl_k_you_id_'. $day .'"';
        $html     = file_get_contents($url);
        $start    = strpos($html,$position);
        $str      = substr($html,$start,600);

        //月份
        preg_match('/<div\sclass="wnrl_k_you_id_wnrl_nongli">.*?<\/div>/',$str,$month);
        $month = strip_tags($month[0]);
        $month = strstr($month,'月',true).'月';

        //生肖
        preg_match('/<div\sclass="wnrl_k_you_id_wnrl_nongli_ganzhi">.*?<\/div>/',$str,$match);
        $dry_branch = strip_tags($match[0]);
        $tmp = explode(' ',$dry_branch);
        $zodiac = $tmp[1];
        unset($tmp[1]);
        $zodiac = str_replace(['【','】'],'',$zodiac);
        $zodiac = strstr($zodiac,'年',true);

        //天干地支
        $branch = implode(' ',$tmp);

        return [
            'date'   => date('Y年m月d日',strtotime($date)),
            'zodiac' => $zodiac,
            'branch' => $branch,
            'month'  => $month,
        ];
    }

    /**
     * 输出CSV表格
     * @param $data
     */
    private function export_csv($data)
    {
        $string="";
        foreach ($data as $key => $value)
        {
            foreach ($value as $k => $val)
            {
                $value[$k]=iconv('utf-8','gb2312',$value[$k]);
            }

            $string .= implode(",",$value)."\n"; //用英文逗号分开
        }
        $filename = date('Ymd').'.csv'; //设置文件名
        header("Content-type:text/csv");
        header("Content-Disposition:attachment;filename=".$filename);
        header('Cache-Control:must-revalidate,post-check=0,pre-check=0');
        header('Expires:0');
        header('Pragma:public');
        echo $string;
    }

    /**
     * 抓取数据并导出CSV
     */
    public function getBranchCsv()
    {
        $data = [];
        $data[] = [
            'date'   => '生日',
            'zodiac' => '生肖',
            'branch' => '地支',
            'month'  => '月份',
        ];
        foreach($this->dates as $date){
            $res = $this->getBranch($date);
            $data[] = $res;
        }
        $this->export_csv($data);
    }
}
```

### 测试
```php
//测试
$dates = [
    '2008-08-08',
    '2019-09-27',
];
$builder = new Wnl($dates);
$builder->getBranchCsv();
```

### 结果
>嗯，基本满足需求，如需别的数据，可做相应的扩展

![dry_branch](https://s2.ax1x.com/2019/09/27/uK44lq.png)
