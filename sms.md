 #### 短信服务
采用静态工厂模式设计，虽然违背了开放封闭原则，但考虑到短信发送渠道较少的情况下，可以适当放宽原则，对比工厂模式，每次新增一个渠道都要写两个文件，一个工厂类，一个产品类。

 - 阿里云
>composer.json(已提交git)更新 `composer update alibabacloud/client`  

官方文档：[发送短信](https://help.aliyun.com/document_detail/101414.html?spm=a2c4g.11186623.6.616.321756e0Z0WXDo)

```php
        $client = SmsFactory::factory('Aliyun');
        //短信签名名称
        $signName      = '新联新达';
        //手机号码
        $phoneNumbers  = '15299137390';
        //短信模板ID
        $templateCode  = 'SMS_173946383';
        //短信模板变量对应的实际值，JSON格式
        $templateParam = json_encode(['code'=>'4567']);
        //短信默认塔台
        $regionId      = '';
        $params = [$signName,$phoneNumbers,$templateCode,$templateParam,$regionId];
        $res    = $client->send($params);
```  
    
 
 - 容联云
 官方文档：[短信业务接口](http://doc.yuntongxun.com/p/5a533de33b8496dd00dce07c)
 
 ```php
        $client = SmsFactory::factory('CcpRest');
        
        //手机号
        $phone  = '15299137390';
        //模板id
        $tempId = 172985;
        //模板参数 当模板没有参数的时候为空数组
        $datas  = ["1234","5"];
      
        $params = [$phone,$tempId,$datas];
        $result = $client->send($params);
```
