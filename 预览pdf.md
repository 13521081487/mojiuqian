测试使用的axios来请求，使用proxyTable来跨域

后端代码：

    @GetMapping(value = "/pdf2", produces = {MediaType.APPLICATION_PDF_VALUE})
    public byte[] getpdf2() throws IOException {
        byte[] bytes = FileUtil.readBytes("/Users/yangxiying/Downloads/test.pdf");
        return bytes;
    }
前端的 proxy：

    proxyTable: { 
        '/yxy': {
            target: 'http://localhost:10001/',
            changeOrigin: true,//如果需要跨域
            pathRewrite: {
            '^/yxy': '/'
            }
        },
    }
页面展示图片
后台返回的类型为 image/png 的，可以直接在前端 img中的src上写上后端的服务地址，就可以打开显示图片

// 可以使用img标签

<img src="/yxy/test/pic2">

//也可以使用embed标签

<embed src="/yxy/test/pic2" type="image/png" width="100%" height="100%">

页面上的pdf预览显示
后台返回类型为 application/pdf 时：
如果浏览器支持打开pdf，可以简单使用如下方式打开，用的是浏览器的默认控件打开

<embed src="/yxy/test/pdf" type="application/pdf" width="100%" height="800">

使用pdf插件 vue-pdf 打开预览

vue-pdf 插件 ：https://www.npmjs.com/package/vue-pdf

安装插件：npm i vue-pdf --save

在使用时遇到一个问题 This dependency was not found: * @babel/runtime/regenerator in ./node_modules/_pdfjs-dist@2.2.228
检查是否配置依赖了 「"@babel/runtime"」 ，如果没有，安装 npm i @babel/runtime --save

简单使用 如下：默认只展示pdf的第一页

<template>
  <pdf src="/yxy/test/pdf2"></pdf>
</template>
 
<script>
import pdf from 'vue-pdf'
 
export default {
  components: {
    pdf
  }
}
把所有pdf内容全展示
<template>
    <div>
        <pdf
            v-for="i in numPages"
            :key="i"
            :src="src"
            :page="i"
            style="display: inline-block; width: 80%"
        ></pdf>
    </div>
</template>
 
<script>
 
    import pdf from 'vue-pdf'
    
    var loadingTask = pdf.createLoadingTask('/yxy/test/pdf2');
    
    export default {
        components: {
            pdf
        },
        data() {
            return {
                src: loadingTask,
                numPages: undefined,
            }
        },
        mounted() {
            this.src.then(pdf => {
                this.numPages = pdf.numPages;
            });
        }
    }
 
</script>
