>背景介绍：在重构项目做到产品添加（项目有关的产品分为线上课程，线下课程，音频课程，视频课程等）的时候，把产品的属性分为SKU属性（类似电商），动态属性。其中动态属性包括普通场景，表单场景和复训场景等。在添加SKU属性的时候，由于项目比较紧急，而且手上还有别的项目需要维护，暂时只做了个二维的SKU组合，但后来需要三维甚至更多的SKU组合，又特地优化一下，故此记录一下。

### 环境介绍

vue + element 

### 前端代码（精简版）
```html
<template>
    <div>
        <div>SKU组合demo</div>
        <div v-for="(v, i) in list" :key="i" class="mt-20">
            <!--<b>{{ v.name }}：</b>-->
            <el-checkbox-group v-model="checkList[i].list">
                <el-checkbox v-for="(k, j) in v.list" :key="j" :label="k" />
            </el-checkbox-group>
        </div>

         <div class="mt-20">
           <el-button type="primary" @click="handleClick">确定</el-button>
         </div>

         <div class="mt-20">
           <el-tag v-for="(item, index) in skuList" :key="index" style="margin:10px 10px;">{{ item }}</el-tag>
         </div>
    </div>
</template>

<script>
    export default {
        name: "Sku",
        data(){
            return {
                list: [
                     { name: '尺码', list: ['S', 'M', 'L', 'XL', 'XXL'] },
                     { name: '颜色', list: ['红色', '黄色', '蓝色', '粉色', '紫色'] },
                     { name: '图案', list: ['猫咪', '人物', '飞机', '闪电', '字母'] }
                ],
                checkList: [
                    { name: '尺码', list: [] },
                    { name: '颜色', list: [] },
                    { name: '图案', list: [] }
                ],
                skuArray: [],
                skuList: [],
            }
        },
        methods: {
            handleClick() {
                // 先清空数据，保证连续点击按钮，数据不会重复
                this.skuArray = []
                this.skuList = []
                // 将选中的规格组合成一个大数组 [[1, 2], [a, b]...]
                this.checkList.forEach(element => {
                    element.list.length > 0 ? this.skuArray.push(element.list) : ''
                })
                // 勾选了规格，才调用方法
                if (this.skuArray.length > 0) {
                    this.getSkuData([], 0, this.skuArray)
                } else {
                    this.$message.error('请先勾选规格')
                }
            },
            // 递归获取每条SKU数据
            getSkuData(skuArr = [], i, list) {
                for (let j = 0; j < list[i].length; j++) {
                    if (i < list.length - 1) {
                        skuArr[i] = list[i][j]
                        this.getSkuData(skuArr, i + 1, list) // 递归循环
                    } else {
                        this.skuList.push([...skuArr, list[i][j]]) // 扩展运算符，连接两个数组
                    }
                }
            }
        }
    }
</script>

<style scoped>
    .mt-20 {
        margin-top: 20px;
    }
</style>
```

### 效果

![1CGC1s](https://cdn.learnku.com/uploads/images/202001/19/21543/FUkjXnxZMi.gif!large)

### 优化
由于是公司项目，在此就不贴出具体代码，大致讲一下优化思路好了。

![1CU79S](https://cdn.learnku.com/uploads/images/202001/19/21543/tFUCl0en8P.gif!large)

1、产品SKU属性下面增加表格
2、各个产品SKU的组合名，价格，描述，库存，图片等支持编辑和删除
3、SKU图片支持预览
4、新增SKU组合时，监控组合，避免重复编辑
5、取消所有SKU组合时，表格隐藏等
6、抽取共同部分做成组件更好

