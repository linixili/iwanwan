### fighting! wanwan 

# webpack 

    概念化： 本质上，webpack 是一个用于现代 JavaScript 应用程序的静态模块打包工具。当 webpack 处理应用程序时，它会在内部构建一个 依赖图(dependency graph)，此依赖图对应映射到项目所需的每个模块，并生成一个或多个 bundle。

## js 由来浅谈
      1994年，网景公司（Netscape）发布了Navigator浏览器0.9版。这个版本的浏览器只能用来浏览，不具备与访问者互动的能力。网景公司急需一种网页脚本语言，使得浏览器可以与网页互动。
      1995年5月，网景公司做出决策，未来的网页脚本语言必须"看上去与Java足够相似"，但是比Java简单，使得非专业的网页作者也能很快上手。这个决策实际上将Perl、Python、Tcl、Scheme等非面向对象编程的语言都排除在外了。Brendan Eich被指定为这种"简化版Java语言"的设计师，只用了10天设计出来了。
      整体的设计思路
      （1）借鉴C语言的基本语法；
      （2）借鉴Java语言的数据类型和内存管理；
      （3）借鉴Scheme语言，将函数提升到"第一等公民"（first class）的地位；
      （4）借鉴Self语言，使用基于原型（prototype）的继承机制。

## js 模块化
    模块化其实是一种规范、约束。这种规范约束能让我们的代码更具可观性和后续维护性。这种方式会大大的提高我们的工作效率，同时减少了后面维护的时间。
        随着ajax技术的兴起前端页面交互日益复杂，项目代码量日益膨胀，js本身并没有设计模块化这方面功能，后续发展推出了主要有闭包模块化模式，CommonJS，Amd，ES2015模块化规范


　  
## js 模块化打包工具
    gulp grunt fis3 rollup webpack

## webpack核心概念
    - entry
        基础配置：
            {
                生成的文件名: 入口模块路径
            }
        入口起点，指示webpack应该使用哪个模块作为， 来作为其构建内部的开始。
        可以配置多个入口，需要在output 配置 [] 占位符 分别生成对应的文件。
    - output
        基础配置：
            {
                path: Path.resolve(__dirname, 'dist'),
                filename: [name].[hash].js,
                publicPath: '前置域名多用于配置CDN服务器域名'
            }
        output 属性告诉 webpack 在哪里输出它所创建的 bundle，以及如何命名这些文件。
        主要输出文件的默认值是 ./dist/main.js，其他生成文件默认放置在 ./dist 文件夹中。

    - modules

        webpack 天生支持如下模块类型
        ECMAScript 模块
        CommonJS 模块
        AMD 模块
        Assets
        WebAssembly 模块
        通过loader可以使webpack支持多种语言和处理器语法编写的模块，loader向webpack描述了如何处理非原生模块并且将相关依赖引入到bundles中

        在webpack 中每一个文件即是一个模块
        module : {
            rules: [
                {
                    test: '/\.(jpg|png|jpeg)$/',
                    use: ['url-loader'],
                    options: {
                        // placeholder
                        name: '[name]_[hash].[ext]',
                        limit: 2048,
                        outputPath: 'images/'
                    }
                }
            ]
        }

    - resolve
        基础配置：
        {
            alias: {
                '' : ''
            },
            extensions: ['.js', '.vue'],
            mainFiles: ['index'],
            module: ['node_modules', path.resolve(__dirname, 'src')]
            plugins: [
                new DirectoryNamedWebpackPlugin(true)
            ]
        }
        可以设置模块如何被解析
        resolve.alias 用来给require / import 引入的模块起别名，使引入更简单
        resolve.extensions  允许省略的扩展名 并尝试按顺序来解析这些扩展名
        resolve.mainFiles 解析文件夹要使用的文件名
        resolve.module 告知webpack 解析模块时要查找的目录
        resolve.plugin 编译阶段引用插件做额外的处理
        
    - plugin
        plugin用于执行范围更广的任务。包括：打包优化，资源管理，注入环境变量。
        结合webpack 打包生命周期钩子当中 去注册执行任务
        打包优化
            terserWebpackPlugin 多进程打包js
            happyPack 在webpack 调用loader 进行编译的过程中 多进程提升编译效率
            htmlWebpackPlugin 用于生成html文件并自动引入打包后的入口文件
            extract-text-webpack-plugin 将css提取为独立的css文件
            fs-ts-webpack-plugin 用于异步编译typescript 提升构建效率
            webpack-bundle-analyzer 查看打包后的分包体积
            ...
        资源管理
            cleanWebpackPlugin 用于清除上次打包留下的文件

    - loader
        loader 用于对模块进行源代码转换， 转换成浏览器能够识别的文件，语言类型
        jpg|jpeg|png     url-loader file-loader 
        css/scss/less    style-loader css-loader  scss/less-loader postcss-loader
            css-loader 用于识别模块外链样式，最终转换成一个数组
            style-loader 通过js去创建一个style标签，内部包含样式
            postcss-loader 用于适配个浏览器厂商前缀
    - optimization
        优化代码体积， 打包bundle
        {   
            minimize: true // 默认为true 压缩js 代码
            minimizer: [] // 可以自定义 uglifyPlugin
            splitChunks: {
                chunk: 'all',
                minSize: 1000,
                maxSize: 0,
                minChunks: 1, //被引入超过1次则做代码分割
                maxAsyncRequests: 5, //并发分割模块数最大5个
                maxInitRequests: 5, //入口加载分割模块数最大5个
                cacheGroup: { // 缓存组
                    venders: {
                        test: /[\\/]node_modules[\\/]/,
                        priority: -10,
                        filename: 'vender.js'
                    },
                    default: {
                        priority: -20, // 权重
                        reuseExistingChunk: true, // 复用模块
                        filename: 'common.js'
                    }
                }
            } // 根据不同的策略来打包bundle 
        }
        
    - devtool
        配置sourceMap
        'cheap-module-eval-source-map' : false // 前者对应dev环境，不会生成单独的source-map 文件， 而会打包到bundler中， 并且只会将错误定位到行

    - devServer
        webpack 本地服务器
        {
            contentBase: path.join(__dirname, 'dist'),
            compress: true,
            port: 9000,
            proxy: {
                '/api': 'http://localhost:3000',
            }
        }
## babel eslint + prettier pre-comit + husky typescript 相关配置
https://lins-iww.github.io/lins_blog/webpack/
　