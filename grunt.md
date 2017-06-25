## grunt for Mac 使用

### 安装所需

> Grunt和 Grunt 插件是通过 [npm](https://npmjs.org/) 安装并管理的，npm是 [Node.js](http://nodejs.org/) 的包管理器。

### 安装文档

- [官网](https://gruntjs.com/)
- [中文文档](http://www.gruntjs.net/getting-started)

### 淘宝镜像

地址: [淘宝 NPM 镜像](https://npm.taobao.org/)

适合对象: 使用 npm 安装包时长时间无响应可用此镜像替代

### 开始使用

示例项目下载: [udacity-fend-website-optimization](https://github.com/udacity/cn-frontend-development-advanced/raw/master/Website%20Optimization_zh.zip)

**使用grunt所必须的两个文件**：

- `package.json`
- `Gruntfile.js`

新建 `package.json`:

```shell
npm init
```

安装 grunt:

###### 以下命令 `cnpm` 是由于我使用了 淘宝镜像 ，如果你不是，请在操作时将 `cnpm` 替换为 `npm`

```shell
cnpm install grunt --save-dev
```

其它必要插件:

```shell
cnpm install grunt-contrib-copy --save-dev
			 grunt-contrib-clean
			 grunt-contrib-concat
			 grunt-contrib-cssmin
			 grunt-contrib-htmlmin
			 grunt-contrib-imagemin
			 grunt-contrib-jshint
			 grunt-contrib-uglify
			 grunt-usemin
			 matchdep
```

对应插件作用:

```python
grunt => grunt
grunt-contrib-clean 			=> 清除文件
grunt-contrib-concat 			=> 合并文件
grunt-contrib-copy 				=> 拷贝文件
grunt-contrib-cssmin 			=> 压缩css
grunt-contrib-htmlmin 			=> 压缩html
grunt-contrib-imagemin 			=> 压缩图片
grunt-contrib-jshint 			=> js语法检测
grunt-contrib-uglify 			=> 压缩js
grunt-usemin 					=> 文件引用替换
matchdep 						=> 自动载入所有任务
```



安装完毕后在项目根目录下新建 `Gruntfile.js` 文件：

```javascript
// wrapper函数，包含了整个Grunt配置信息。
// module.exports = function(grunt) {}

module.exports = function (grunt) {

  // matchdep 免去重复 加载task
  require('matchdep').filterDev('grunt-*').forEach(grunt.loadNpmTasks);

  // 初始化 configuration 对象
  grunt.initConfig({
    // 从package.json 文件读入项目配置信息，并存入pkg 属性内。
    pkg: grunt.file.readJSON('package.json'),

    // 清除生产目录dist下所有文件夹及其代码
    clean: {
      all: ['dist/**', 'dist/*.*'],
      image: ['dist/img', 'dist/views/images'],
      css: ['dist/css', 'dist/views/css'],
      html: 'dist/**/*'
    },

    // 从源src目录下拷贝需要做自动化处理的文件
    copy: {
      src: {
        files: [
          {expand: true, cwd: 'src', src: ['*.html', 'views/*.html'], dest: 'dist'}
        ]
      },
      image: {
        files: [
          {expand: true, cwd: 'src', src: ['img/*.{png,jpg,jpeg,gif}','views/images/*.{png,jpg,jpeg,gif}'], dest: 'dist'}
        ]
      },
      css: {
        files: [
          {expand: true, cwd: 'src', src: ['css/*.css', 'views/css/*.css'], dest: 'dist'}
        ]
      },
      js: {
        files: [
          {expand: true, cwd: 'src', src: ['js/*.js', 'views/js/*.js'], dest: 'dist'}
        ]
      }
    },

    // usemin 准备工作
    useminPrepare: {
      html: ['index.html', 'views/pizza.html'],
      options: {
        dest: 'dist'
      }
    },

    // // 文件合并
    // concat: {
    //   options: {
    //     separator: ';',    //定义一个用于插入合并输出文件之间的字符
    //     stripBanners: true //合并时允许输出头部信息
    //   },
    //   js: {
    //     src: [
    //       "src/js/*.js"          // 将要被合并的文件
    //     ],
    //     dest: "dist/js/app.js"   // 合并后的JS文件的存放位置
    //   },
    //   css:{
    //     src: [
    //       "src/css/*.css"
    //     ],
    //     dest: "dist/css/main.css"
    //   }
    // },

    // 压缩JS
    uglify: {
      prod: {
        options: {
            mangle: false,
            banner: '/*! <%= pkg.name %> <%= grunt.template.today("dd-mm-yyyy") %> */\n'
        },
        files: [{
            expand: true,
            cwd: 'dist',
            src: ['js/*.js', 'views/js/*.js', '!js/*.min.js', '!views/js/*.min.js'],
            dest: 'dist',
            ext: '.min.js',  // 更改拓展名为 *.min.js
            extDot: 'first'  // 从文件名第一个 `.` 开始匹配
        }]
      }
    },

    // 压缩CSS
    cssmin: {
      prod: {
        options: {
          report: 'gzip',
          banner: '/*! <%= pkg.name %> <%= grunt.template.today("dd-mm-yyyy") %> */\n',
          beautify: {
            // 中文ascii化，防止中文乱码
            ascii_only: true
          }
        },
        files: [
          {
            expand: true, // expand 设置为true用于启用下面的选项：
            cwd: 'dist',
            src: ['css/*.css', 'views/css/*.css', '!css/*.min.js', '!views/css/*.min.js'],
            dest: 'dist',
            ext: '.min.css',
            extDot: 'first'
          }
        ]
      }
    },

    // 压缩图片
    imagemin: {
      prod: {
        options: {
          optimizationLevel: 7,
        //   pngquant: true
        //   use: [mozjpeg()]
        },
        files: [
          {expand: true, cwd: 'dist', src: ['img/*.{png,jpg,jpeg,gif,webp,svg}','views/images/*.{png,jpg,jpeg,gif}'], dest: 'dist'}
        ]
      }
    },

    // 处理html中css、js 引入合并问题
    usemin: {
      html: ['dist/index.html', 'dist/views/pizza.html']
    },

    // 压缩HTML
    htmlmin: {
      options: {
        removeComments: true,
        removeCommentsFromCDATA: true,
        collapseWhitespace: true,
        collapseBooleanAttributes: true,
        removeAttributeQuotes: false,
        removeRedundantAttributes: true,
        useShortDoctype: true,
        removeEmptyAttributes: true,
        removeOptionalTags: false
      },
      html: {
        files: [
          {expand: true, cwd: 'dist', src: ['*.html', 'views/*.html'], dest: 'dist'}
        ]
      }
    }

  });

  // 自定义任务
  grunt.registerTask('prod', [
    'copy',                 //复制文件
    // 'concat',               //合并文件
    'useminPrepare',        // usemin准备工作
    'imagemin',             //图片压缩
    'cssmin',               //CSS压缩
    'uglify',               //JS压缩
    'usemin',               //HTML处理
    'htmlmin'               //HTML压缩
  ]);

  grunt.registerTask('publish', ['clean', 'prod']);
};
```

usemin 相关:

> 想要使用usemin自动替换被压缩合并为 *.min.js/css 的文件，需要在src(原始目录)对应 *.html 中设置最终要被替换的文件名，规则为：
>
> ```
> <!-- build:<type>(alternate search path) <path> -->
> ... HTML Markup, list of script / link tags.
> <!-- endbuild -->
> ```

- 原始的index.html:

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      ...
      <!-- build:css css/style.min.css -->
      <link href="css/style.css" rel="stylesheet">
      <!-- endbuild -->
      <!-- build:css css/print.min.css -->
      <link href="css/print.css" rel="stylesheet">
      <!-- endbuild -->
      ...
      <!-- build:js js/perfmatters.min.js -->
      <script async src="js/perfmatters.js"></script>
      <!-- endbuild -->
    </head>
    ...
  ```

- 原始的 views——>pizza.html

  ```html
  <!DOCTYPE HTML>
  <html>
  <head>
    <!-- build:css css/style.min.css -->
    <link rel="stylesheet" href="css/style.css">
    <!-- endbuild -->
    <!-- build:css css/bootstrap-grid.min.css -->
    <link rel="stylesheet" href="css/bootstrap-grid.css">
    <!-- endbuild -->
    ...
    
    
    <!-- build:js js/main.min.js -->
    <script type="text/javascript" src="js/main.js"></script>
    <!-- endbuild -->
  </body>
  ```

- [yeoman-grunt-usemin](https://github.com/yeoman/grunt-usemin)

发布:

```shell
grunt publish
```

完成



### 其他

- [grunt配置任务](http://www.gruntjs.net/configuring-tasks)
- [Gruntfile 实例](http://www.gruntjs.net/sample-gruntfile)
- [自动化任务运行器 Grunt 迅速上手](http://blog.jobbole.com/51586/)
- [imooc-前端自动化工具](http://www.imooc.com/learn/30)

其中推荐学习 `imooc` 的前端自动化工具 课程，虽然是几年前的东西，但是里面讲的内容对想要入门grunt的同学仍有很大帮助！
