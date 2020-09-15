
##  创建Vue项目简单介绍

> 1.选择Vue项目的保存路径
```txt
    1.比如说D盘下面的html文件夹下边:[D:/html]
    2.vue create my-project  按下回车键
```

> 2.执行完[vue create my-project]命令之后会出现以下提示:
```txt
Vue CLI v3.0.0
? Please pick a preset:
  vuecli3 (vue-router,vueX,stylus,babel,eslint,unit-mocha)
  default (babel,eslint)
  > Manually select features
```
> 2.1解释上面一坨代码是嘛意思:
```txt
vuecli3,default,Manually是三个选择项
vuecli3:记录自定义vue项目的配置
default:默认配置
Manualy:自定义配置

*如果是第一次执行,选择default和manually,default一路默认执行下去就可以了,manually需要自己配置.
```

>3选择Manually自定义之后又会弹出一坨坨代码:
```txt
    Vue CLI v3.0.0
    ? Please pick a preset:Manually select features
    ?Check the features needed for your project:
    (*) Babel
    ( ) TypeScript
    ( ) Progressive Web App (PWA) Support
    (*) Router
    (*) VueX
    (*) CSS Pre-processors
    (*) Linter / Formatter
    (*) Unit Testing
    ( ) E2E Testing
```

>3.1 别着急我们继续分析:
```txt
  
  * 空格键是选中与取消,A键是全选
  
  TypeScript    --支持使用TypeScript书写源码
  Progressive Web App (PWA) Support  --PWA支持
  Router   --支持vue-router
  VueX     --支持vueX
  Css Pre-processors --支持css预处理
  Linter/Formatter  --支持代码风格检查和格式化
  Unit Testing  --支持单元测试
  E2E Testing --支持E2E测试
```
>4.选好之后,回车继续,令人兴奋的是 [又一坨]:
```txt
    Vue CLI v3.0.0
    ?please pick a preset:Manually select features
    ?Check the features needed for your project:Babel,Router,Vuex,CSS Pre-processors,Linter,Unit Testing
    ?Use history mode for router?(Requires proper server setup for index fallback in production) Yes
    ?Pick a CSS pre-processor (PostCss,Autoprefixer and CSS Modules are supported by default):
    SCSS/SASS
    LESS
   >Stylus 
```
>4.1 来来来我们分析一下:
```txt
    从?pick a CSS pre-processor这开始:
    上边的是我们第4步之前选择的一些配置
    下边的是我们将要选择的配置
    我们一般选择Stylus,Css的预处理
```
>5 回车一下,我们继续,这一步我选择的是ESLint + Prettier
```txt
    Vue CLI v3.0.0
    ?please pick a preset:Manually select features
    ?Check the features needed for your project:Babel,Router,Vuex,CSS Pre-processors,Linter,Unit Testing
    ?Use history mode for router?(Requires proper server setup for index fallback in production) Yes
    ?Pick a CSS pre-processor (PostCss,Autoprefixer and CSS Modules are supported by default):stylus
    ?Pick a linter / formatter config:
     ESlint with error prevention only
     ESlint + Airbnb config
     ESlint + Standard config
     ESlint + Prettier
```
>6.选择语法检查方式,我自己选择保存检测,第一个是保存检测,第二个是fix[修改]和commit[提交]的时候检测
```txt
    Vue CLI v3.0.0
    ?please pick a preset:Manually select features
    ?Check the features needed for your project:Babel,Router,Vuex,CSS Pre-processors,Linter,Unit Testing
    ?Use history mode for router?(Requires proper server setup for index fallback in production) Yes
    ?Pick a CSS pre-processor (PostCss,Autoprefixer and CSS Modules are supported by default):stylus
    ?Pick a linter / formatter config:Prettier
    ?Pick additional lint features:(Press <space> to select,<a> to toggle all,<i>to invert selection)
   >(*) Lint on save
    ( ) Lint and fix on commit 
```
>7.单元测试,我选择了Mocha
```txt
    Vue CLI v3.0.0
    ?please pick a preset:Manually select features
    ?Check the features needed for your project:Babel,Router,Vuex,CSS Pre-processors,Linter,Unit Testing
    ?Use history mode for router?(Requires proper server setup for index fallback in production) Yes
    ?Pick a CSS pre-processor (PostCss,Autoprefixer and CSS Modules are supported by default):stylus
    ?Pick a linter / formatter config:Prettier
    ?Pick additional lint features:Lint on save
    ?Pick a unit testing solution:(Use arrow keys)
   >Mocha + Chai
    Jest 
```
>8.配置文件存放地方,第一个式独立文件夹位置,第二个是在package.json文件里
```txt
    Vue CLI v3.0.0
    ?please pick a preset:Manually select features
    ?Check the features needed for your project:Babel,Router,Vuex,CSS Pre-processors,Linter,Unit Testing
    ?Use history mode for router?(Requires proper server setup for index fallback in production) Yes
    ?Pick a CSS pre-processor (PostCss,Autoprefixer and CSS Modules are supported by default):stylus
    ?Pick a linter / formatter config:Prettier
    ?Pick additional lint features:Lint on save
    ?Pick a unit testing solution:Mocha
    ?Where do you prefer placing config for Babel,PostCss,ESLint,etc.?(Use arrow keys)
   >In dedicated config files
    In packages.json
```
>9.询问是否记录这一次的配置,以便下次使用,如一开始的时候会显示的vuecli3配置
```txt
    Vue CLI v3.0.0
    ?please pick a preset:Manually select features
    ?Check the features needed for your project:Babel,Router,Vuex,CSS Pre-processors,Linter,Unit Testing
    ?Use history mode for router?(Requires proper server setup for index fallback in production) Yes
    ?Pick a CSS pre-processor (PostCss,Autoprefixer and CSS Modules are supported by default):stylus
    ?Pick a linter / formatter config:Prettier
    ?Pick additional lint features:Lint on save
    ?Pick a unit testing solution:Mocha
    ?Where do you prefer placing config for Babel,PostCss,ESLint,etc.? In dedicated config files
    ?Save this as a preset for future project?(y/N) y
```
>10.回车确定等待下载

>11.装好后，启动
```txt
    cd my-project // 进入到项目根目录
    npm run serve // 启动项目
```