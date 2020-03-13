### 手摸手教你撸一个基于Element的地区联动
#### [预备知识](#a)
#### [代码实现](#b)
#### [效果展示](#c)
#### [额外补充](#d)



#### 预备知识<div id="a"></div>

使用《中华人民共和国行政区划代码》国家标准(GB/T2260). 这部分可分为三个层次,从左到右的含义分别是：
- 第一、二位表示省(自治区、直辖市、特别行政区)
- 第三、四位表示市(地区、自治州、盟及国家直辖市所属市辖区和县的汇总码)
- 第五、六位表示县(市辖区、县级市、旗).

比如`440305`表示广东省深圳市南山区，`44`代表广东省，`03`代表深圳市，`05`表示南山区  

本次讲解所用到的中华人民共和国行政区划代码是从[中华人民共和国民政部](http://www.mca.gov.cn/article/sj/xzqh/2019/)和[国家统计局](http://www.stats.gov.cn/tjsj/tjbz/tjyqhdmhcxhfdm/)获取的最新数据,
港澳台的数据仅供参考，可以查找相关官方网站，[澳门行政区规划](https://www.cpu.gov.mo/zh-hans/),[澳门地籍资讯网](https://cadastre.gis.gov.mo/MGSP_Cad/index.html),[台湾行政规划](http://www.xzqh.org/html/list/10034.html),[香港行政规划](https://baike.baidu.com/item/%E9%A6%99%E6%B8%AF%E5%8D%81%E5%85%AB%E5%8C%BA/7265721?fr=aladdin)  
 
另外参考项目地址：[https://github.com/mumuy/data_location](https://github.com/mumuy/widget)


#### 代码实现<div id="b"></div>
```javascript
/**
* 获取城市二级或者三级联动的数据，返回的数据格式是element级联选择器所需要的格式
* @param area 参考项目地址演示数据获取的最新JSON数据
* @param level 2表示二级联动，3表示三级联动
* @returns {string}
*/
const getRegion = (area , level=3)=> {
    let areas = [];
    let hasCity = false;
    let province_code = '';
    let city_code = '';
    for(let [code,name] of Object.entries(area)){
        if( ! (code % 1e4) ){ //省份或者直辖市 34个
            province_code = code; //临时存储省或直辖市的code
            areas[code] = {value:name,label:name};
            hasCity = false;
        }else{
            if( areas[province_code].children === undefined){
                areas[province_code].children = [];
            }
            let diff = code - province_code;
            if(diff > 0 && diff < 1e4){ //同一省的城市或地区
                if( ! (code % 100) ){ //省份的地区市
                    hasCity = true;
                    city_code = code;
                    areas[province_code].children[code] = {value:name,label:name};
                }else if(diff > 8000){ //省直辖县级行政单位
                    city_code = code;
                    areas[province_code].children[code] = {value:name,label:name};
                }else if(hasCity){  //非直辖市
                    if( level !== 3 ){
                        continue;
                    }
                    if( areas[province_code].children[city_code].children === undefined){
                        areas[province_code].children[city_code].children = [];
                    }
                    areas[province_code].children[city_code].children[code]={value:name,label:name};
                }else{ //直辖市
                    areas[province_code].children[code] = {value:name,label:name};
                }
            }
        }
    }
    let tmp  = Object.values(areas);
    tmp.forEach(element =>{
        element.children = Object.values(element.children);
        element.children.forEach(value => {
            if(value.children !== undefined){
                value.children = Object.values(value.children);
            }
        });
    })
    return JSON.stringify(tmp);
}
```
#### 效果展示<div id="c"></div>
[![eaMDSS.md.png](https://s2.ax1x.com/2019/08/01/eaMDSS.md.png)](https://imgchr.com/i/eaMDSS)

#### 额外补充<div id="d"></div>
>具体需要做成什么样子的级联选择器，看项目需求，万变不离其宗，搞清原理就好    
>如果觉得数据不准确的话，可以请求高德或者百度的API获取数据  
