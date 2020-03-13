## Tinymce的安装
>网上资料有很多安装教程，而且大都一样。记得当初看到一篇文章说是tinymce安装教程，收藏之后就再也没有亲手实践。临近过春节，都放假了，趁今天有点空闲时间，就研究了一下，还是发现有不少坑需要踩的，以防将来需要用的时候，戳手不及，就记录一下。

1、安装`tinymce`
```html
npm install tinymce -S
```

2、安装`tinymce-vue`,在vue日益强大的趋势下，官方推出了依赖包。
```html
npm install @tinymce/tinymce-vue -S
```

我其实很纳闷，这些安装依赖项别人怎么知道，原来这些都在官网上说明了。费话不多说，有图有真相。

对应步骤1,官方给出的[Get Tinymce by npm](https://www.tiny.cloud/get-tiny/package-managers/)

![1iarVA](https://s2.ax1x.com/2020/01/20/1iarVA.png)

对应步骤2，官方给出的`tinymce-vue`[整合包](https://www.tiny.cloud/docs/integrations/vue/#procedure)

![1iag8f](https://s2.ax1x.com/2020/01/20/1iag8f.png)



3、汉化，下载中[文语言包](https://www.tiny.cloud/get-tiny/language-packages/)

![1iaT5q](https://s2.ax1x.com/2020/01/20/1iaT5q.png)

- 在项目根目录的public目录下新建tinymce文件夹，将下载的中文语言包解压后放在该文件夹下
- 在依赖包node_modules里找到tinymce文件夹，将里面的skins文件夹复制到public/tinymce目录下

![1iaq2T](https://s2.ax1x.com/2020/01/20/1iaq2T.png)

4、简单DEMO
```html
<template>
    <div class="activeConfig">
        <div class="activeConfig-container">
            <Editor id="tinymce" v-model="tinymceHtml" :init="editorInit" />
        </div>
    </div>
</template>

<script>
    // 引入组件
    import tinymce from 'tinymce/tinymce'
    import Editor from '@tinymce/tinymce-vue'
    // 引入富文本编辑器主题的js和css
    import 'tinymce/themes/silver/theme.min.js'
    import 'tinymce/skins/ui/oxide/skin.min.css'
    // 扩展插件
    import 'tinymce/plugins/image'
    import 'tinymce/plugins/media'
    import 'tinymce/plugins/link'
    import 'tinymce/plugins/code'
    import 'tinymce/plugins/table'
    import 'tinymce/plugins/lists'
    import 'tinymce/plugins/paste'
    import 'tinymce/plugins/imagetools'
    import 'tinymce/plugins/print'
    import 'tinymce/plugins/preview'
    import 'tinymce/plugins/searchreplace'
    import 'tinymce/plugins/autolink'
    import 'tinymce/plugins/directionality'
    import 'tinymce/plugins/visualblocks'
    import 'tinymce/plugins/visualchars'
    import 'tinymce/plugins/fullscreen'
    import 'tinymce/plugins/template'
    import 'tinymce/plugins/codesample'
    import 'tinymce/plugins/charmap'
    import 'tinymce/plugins/hr'
    import 'tinymce/plugins/pagebreak'
    import 'tinymce/plugins/nonbreaking'
    import 'tinymce/plugins/anchor'
    import 'tinymce/plugins/insertdatetime'
    import 'tinymce/plugins/advlist'
    import 'tinymce/plugins/wordcount'
    import 'tinymce/plugins/textpattern'
    import 'tinymce/plugins/help'
    import 'tinymce/plugins/emoticons'
    import 'tinymce/plugins/autosave'

    import 'tinymce/plugins/autoresize'
    //import 'tinymce/plugins/formatpainter'
    
    //以下组件为自定义组件，并且多图片上传，和百度地图，表情包存在坑，这些坑后面再介绍
    import '@/plugins/axupimgs'
    import '@/plugins/bdmap'
    import '@/plugins/indent2em'
    import '@/plugins/lineheight'

    export default {
        name: 'ActiveConfig',
        components: { Editor },
        data() {
            return {
                // tinymce的绑定值
                tinymceHtml: '',
                // tinymce的初始化配置
                editorInit: {
                    //tinumce容器
                    selector: '#tinymce',
                    //配置语言
                    language_url: '/tinymce/langs/zh_CN.js',
                    language: 'zh_CN',
                    //配置皮肤
                    skin_url: '/tinymce/skins/ui/oxide',
                    //配置编辑器高度
                    height:500,
                    min_height: 300,
                    max_height:650,
                    width:1000,
                    min_width:900,
                    max_width:1200,
                    emoticons_database_url: '/emojis.js',
                    emoticons_append: {
                        "diy1": {
                            "keywords": ["diy1"],
                            "char": "\uD83E\uDD2F",
                            "category": "animals_and_nature"
                        }
                    },
                    //配置插件
                    plugins: 'print preview searchreplace autolink directionality visualblocks visualchars fullscreen image link' +
                        ' media template code codesample table charmap hr pagebreak nonbreaking anchor insertdatetime advlist ' +
                        'lists wordcount imagetools textpattern help autosave bdmap indent2em autoresize lineheight ' +
                        'axupimgs emoticons',
                    toolbar: 'code undo redo restoredraft | cut copy paste pastetext | \
                        forecolor backcolor bold italic underline strikethrough link anchor | \
                        alignleft aligncenter alignright alignjustify outdent indent | \
                        styleselect formatselect fontselect fontsizeselect | bullist numlist | \
                        blockquote subscript superscript removeformat | \
                        table image media charmap hr pagebreak insertdatetime print preview | fullscreen | \
                        bdmap indent2em lineheight axupimgs emoticons',
                    // 此处为图片上传处理函数
                    iimages_upload_base_path: '/demo',
                    images_upload_handler: function (blobInfo, succFun, failFun) {
                        var xhr, formData;
                        var file = blobInfo.blob();//转化为易于理解的file对象
                        xhr = new XMLHttpRequest();
                        xhr.withCredentials = false;
                        xhr.open('POST', '/demo/upimg2.php');
                        xhr.onload = function() {
                            var json;
                            if (xhr.status != 200) {
                                failFun('HTTP Error: ' + xhr.status);
                                return;
                            }
                            json = JSON.parse(xhr.responseText);
                            if (!json || typeof json.location != 'string') {
                                failFun('Invalid JSON: ' + xhr.responseText);
                                return;
                            }
                            succFun(json.location);
                        };
                        formData = new FormData();
                        formData.append('file', file, file.name );
                        xhr.send(formData);
                    },
                    fontsize_formats: '12px 14px 16px 18px 24px 36px 48px 56px 72px',
                    font_formats: '微软雅黑=Microsoft YaHei,Helvetica Neue,PingFang SC,sans-serif;苹果苹方=PingFang SC,Microsoft YaHei,sans-serif;宋体=simsun,serif;仿宋体=FangSong,serif;黑体=SimHei,sans-serif;Arial=arial,helvetica,sans-serif;Arial Black=arial black,avant garde;Book Antiqua=book antiqua,palatino;',
                    link_list: [
                        { title: '预置链接1', value: 'http://www.tinymce.com' },
                        { title: '预置链接2', value: 'http://tinymce.ax-z.cn' }
                    ],
                    image_list: [
                        { title: '预置图片1', value: 'https://www.tiny.cloud/images/glyph-tinymce@2x.png' },
                        { title: '预置图片2', value: 'https://www.baidu.com/img/bd_logo1.png' }
                    ],
                    image_class_list: [
                        { title: 'None', value: '' },
                        { title: 'Some class', value: 'class-name' }
                    ],
                    importcss_append: true,
                    //自定义文件选择器的回调内容
                    file_picker_callback: function (callback, value, meta) {
                        if (meta.filetype === 'file') {
                            callback('https://www.baidu.com/img/bd_logo1.png', { text: 'My text' });
                        }
                        if (meta.filetype === 'image') {
                            callback('https://www.baidu.com/img/bd_logo1.png', { alt: 'My alt text' });
                        }
                        if (meta.filetype === 'media') {
                            callback('movie.mp4', { source2: 'alt.ogg', poster: 'https://www.baidu.com/img/bd_logo1.png' });
                        }
                    },
                    autosave_ask_before_unload: false,
                    // 水印“Powered by TinyMCE”
                    branding: false,
                    statusbar: false,
                }
            }
        },
        mounted() {
            tinymce.init({})
        },
        methods: {
            // 图片上传
            handleImgUpload(blobInfo, success, failure) {

            }
        }
    }
</script>

<style scoped>
    .activeConfig-container {
            margin-top: 30px;
            margin-left:25%;
    }
</style>

```

### 效果展示

![1iaugU](https://s2.ax1x.com/2020/01/20/1iaugU.gif)

### Tinymce组件属性

|名称|类型|描述|
|---|---|---|
|:content|string|作为文本内容传入编辑器，可以使用v-model实现双向绑定|
|@change|function|编辑器中Input Change Undo Redo ExecCommand KeyUp NodeChange事件响应后触发的事件返回文本内容|
|:setting|object|编辑器的设置，`setup`不建议在这设置|
|:setup|function|常用与自定义编辑器处理，组建内部做了些处理，建议在这里添加自定义的代码|
|:init|object|tinymce编辑器的初始化配置|


### 注意事项
`tinymce-vue`版本为 `^3.0.1`,`tinymce`的版本为`^5.1.5`,多图片上传，和百度地图，表情包这些插件存在坑，这些坑后面再介绍。因为我要回老家过年了，啊哈哈。
