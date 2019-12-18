### Vue webpack搭建项目

* #### 安装webpack和webpack-dev-server以及Vue ###
    - npm i webpack webpack-dev-server webpack-cli --save-dev
    - npm i vue --save
    - 直接生成一个项目结构 --vue init webpack my-project(项目名)
* #### 配置 ###
    - npm init -y // 用来初始化生成一个新的 package.json 文件，-y ( yes )表示跳过提问阶段。
* #### 配置webpack.config.js ###
    - 根目录下新建dist>webpack.config.js。
配置 入口、出口路径、打包后文件名和devServer的相关配置项
        ```js
        var path = require('path');
        var webpack = require('webpack');

        module.exports = {
        //项目入口文件
        entry: './src/main.js',
        //{app:''}指定输出文件名
        output: {
            //打包出口路径
            path: path.resolve(__dirname, './dist'),
            //通过devServer访问路径
            publicPath: '/',
            //ps 开发环境'/',最顶层，生产环境，输出的目录路径
            //打包后的文件名
            filename: 'main.js'
        },
        mode:'development',
        devServer: {
            historyApiFallback: true,
            overlay: true
        }
        };
        ```
* #### 根目录下新建index.html ###
    - 引入dist打包的js
* #### 新建入口文件 ###
    - 新建src，存放静态文件和各种组件，新建入口文件main.js
* #### webpack准备工作(自定义npm) ###   
    - package.json 增加 scripts：  
        ``` js
        "scripts": {
        "dev": "webpack-dev-server --open --hot --config build/webpack.config.js",
        "build": "webpack --config build/webpack.config.js"
        }
        ```
        ps：hot指自动刷新；报错点：缺少--config build/webpack.config.js，没有进入js。
* #### 配置webpack解析（resolve） ###
    - webpack.config.js中增加：
        ``` js
            resolve: {
        //路径别名
        alias: {
            'vue$': 'vue/dist/vue.esm.js',
            '@':path.resolve(__dirname, './src'),
        },
        //自动解析扩展，引入时不需要加
        extensions: ['.js', '.vue', '.json']
        },
        ```
        配置文档：E:\training\vue\vue2-elm\node_modules\vue\dist\README.md；
        通过alias创建 import 或 require 的别名，来确保模块引入变得更简单；
extensions会让webpack自动查找特定后缀的文件，在项目中引入文件时将不必再书写文件后缀。
* ### 引入vue ###
    - 在index.html中添加挂载点dom（#app）
        ``` js
        <div id="app">{{message}}</div>
        ```
        - main.js引入vue
        ``` js
        import Vue from 'vue'
        var app = new Vue({
            el: "#app",
            data: {
                message: 'hello webpack!!'
            }
        })
        ```
* ### 配置loader——使用 css/scss js、vue、img###
    - webpack只能解析js，因此需要配置解析器
    - 安装scss和相应样式文件解析器
        ```js
        npm i node-sass css-loader vue-style-loader sass-loader -–save-dev
        ```
    - babel-loader(注意不同版本)
        ```
        //webpack 3.x | babel-loader 8.x | babel 7.x
        npm install babel-loader@8.0.0-beta.0 @babel/core @babel/preset-env webpack
        //webpack 3.x babel-loader 7.x | babel 6.x
        npm install babel-loader babel-core babel-preset-env webpack
        ```
    - vue-loader 
        ```
        npm install -D vue-loader vue-template-compiler
        ```
    - 作用
    css-loader用来解析css文件，
    style-loader用来解析dom中通过<style></style>注入的样式，
    而vue-style-loader是vue官方基于style-loader开发的适用于vue的样式解析，
    sass-loader用来解析sass/scss文件
    - 在webpack.config.js中配置解析器
        ```js
        module: {
        rules: [{
        test: /\.css$/,
        use: [
            'vue-style-loader',
            'css-loader'
        ]
        },{
            test: /\.scss$/,
            use: [
            'vue-style-loader',
            'css-loader',
            'sass-loader'
            ]
        },
        {
            test: /\.sass$/,
            use: [
            'vue-style-loader',
            'css-loader',
            'sass-loader?indentedSyntax'
            ]
        },
        {
            test: /\.vue$/,
            loader: 'vue-loader'
        },{
            test: /\.js$/,
            loader: 'babel-loader'
        }]
    }
    ```
    - 图片打包
        1、npm i -D file-loader
        ``` js
        {
            test: /\.(png|jpg|gif|svg)$/,
            loader: 'file-loader',
            options: {
                name: 'images/[name].[ext]'//图片名+图片格式
            }
        }
        ```
        2、url-loader
            ``` js
                {
                    test: /\.(png|jpg|gif|svg)$/,
                    loader: 'url-loader',
                    options: {
                        name: './images/[name].[ext]',
                        limit: 8192//小于size的图片转换成base64
                    }
                }
            ```
* ### 开发环境和生产环境配置 ###
    -结构展示
     webpack-demo
    |- package.json
    - |- webpack.config.js
    + |- webpack.common.js
    + |- webpack.dev.js
    + |- webpack.prod.js
    |- /dist
    |- /src
        |- index.js
        |- math.js
    |- /node_modules
    - 不同环境配置、合并配置方法
        - 安装webpack-merge
        npm install --save-dev webpack-merge（将通用的配置与不同环境的配置合并一起。）
            ``` js
            const merge = require('webpack-merge');
            module.exports = merge(common, {
                devtool: 'inline-source-map',//在生产环境鼓励使用source-map
                devServer: {
                    contentBase: './dist'
                }
            });
            ```
        - 方式一
            - 在生产和开发者环境引入commonconf，merge合并设置，
                DefinePlugin可以创建全局变量，DEV在全局都可以访问。
                ``` js
                    plugins:[
                    new webpack.DefinePlugin({
                        'process.env.NODE_ENV': JSON.stringify('development'),
                        DEV:JSON.stringify('development'),
                    })]
                ```
            - package.json设置
                分别设置配置文件
                ```
                "scripts": {
                    "start": "webpack-dev-server --open --config  webpack.dev.js",
                    "build": "webpack --config webpack.prod.js"
                },
                ```
        - 方式二
            - 在commonconf判断是那种环境，并merge设置对象。
                ```
                    module.exports = env => {
                        let config = env.production===true?prodConf:devConf;
                        return merge(commonConf,config)
                    }//传入env对象
                ```
            - package.json设置
            ```
                "scripts": {
                    "dev": "webpack-dev-server --env.development --open --config build/webpack.common.js",
                    "build": "webpack --config --env.production build/webpack.common.js"
                },
            ```
           
        - 设置环境变量
            安装 npm install --save-dev cross-env
            ```
                 "scripts": {
                        "dev": "cross-env NODE_ENV=development webpack-dev-server --open --config build/webpack.common.js",
                        "build": "cross-env NODE_ENV=production webpack --config build/webpack.common.js"
                    },//cross-env NODE_ENV写在最前面，不然会报错
            ```
            common.config.js
            通过process.env.NODE_ENV获取环境变量。
    - 代码分离
        生成的输出文件由chunks组成。
        - 入口起点：使用 entry 配置手动地分离代码。
            缺点：entry设置多个入口，模块可能会被重复引入，不能动态拆分代码。
        - 防止重复：使用 SplitChunksPlugin 去重和分离 chunk。
            SplitChunksPlugin，如果一个模块被不止一个chunk使用，可以利用代码分离，使模块共享。 
            ```
                optimization:{
                    splitChunks: {
                        chunks: "async",
                        minSize: 30000,
                        minChunks: 1,
                        maxAsyncRequests: 5,
                        maxInitialRequests: 3,
                        automaticNameDelimiter: '~',
                        name: true,
                        cacheGroups: {
                            vendors: {
                                test: /[\\/]node_modules[\\/]/,
                                priority: -10
                            },
                        default: {
                                minChunks: 2,
                                priority: -20,
                                reuseExistingChunk: true
                            }
                        }
                    }
                }
            ```
            - cacheGroups是核心配置，vender和default。
            test可以配置正则和写入function作为打包规则。其他属性均可继承splitChunks，这里必须说一下 priority，设置包的打包优先级，非常重要！ 
            - chunks:
            all: 不管文件是动态还是非动态载入，统一将文件分离。当页面首次载入会引入所有的包
            async： 将异步加载的文件分离，首次一般不引入，到需要异步引入的组件才会引入。
            initial：将异步和非异步的文件分离，如果一个文件被异步引入也被非异步引入，那它会被打包两次（注意和all区别），用于分离页面首次需要加载的包。
            - automaticNameDelimiter： 连接符
            假设我们生成了一个公用文件名字叫vendor，a.js,和b.js都依赖他，并且我们设置的连接符是"~"那么，最终生成的就是 vendor~a~b.js
        - 动态导入：通过模块的内联函数调用来分离代码。

* ### webpack相关插件使用 ### 
    - HtmlWebpackPlugin(用于生成html5文件，会自动将js和css添加在html中)
        - 安装方法
        npm install --save-dev html-webpack-plugin
        - 用法
        ```js
        plugins:[
            new HtmlWebpackPlugin({
                title: 'Hello',
                minify: { // 压缩HTML文件
                    removeComments: true, // 移除HTML中的注释
                    collapseWhitespace: true, // 删除空白符与换行符
                    minifyCSS: true// 压缩内联css
                },
                filename: 'index.html',
                template: './index.html',//html模板所在路径
                inject:true//(true, body, head, false.)js放的位置，true相当于body,false不生成js。
            })
        ]
        ``
    - uglifyjs-webpack-plugin(用于压缩js)
        - 安装方法
        npm install --save-dev uglifyjs-webpack-plugin
        - 用法
        ```
        plugins:[
            new UglifyJSPlugin()
        ]
        ```
    - html-withimg-loader（打包图片）
        npm install html-withimg-loader --save-dev
        ```
            loaders: [
                {
                    test: /\.(htm|html)$/i,
                    loader: 'html-withimg-loader'
                }
            ]
        ```
    - clean-webpack-plugin（npm run build自动清除dist目录）
        npm install clean-webpack-plugin --save-dev
        new CleanWebpackPlugin()
    - copy-webpack-plugin
        npm install copy-webpack-plugin --save-dev
    - DefinePlugin(webpack 内置)
    - mini-css-extract-plugin(将css提取成独立的文件夹)
    npm install -D mini-css-extract-plugin
    ```
        var MiniCssExtractPlugin = require('mini-css-extract-plugin')

        module.exports = {
        // other options...
        module: {
            rules: [
            // ... other rules omitted
            {
                test: /\.css$/,
                use: [
                process.env.NODE_ENV !== 'production'
                    ? 'vue-style-loader'
                    : MiniCssExtractPlugin.loader,
                'css-loader'
                ]
            }
            ]
        },
        plugins: [
            // ... Vue Loader plugin omitted
            new MiniCssExtractPlugin({
            filename: 'static/[name].css'
            })
        ]
        }
    ```
    - CopyWebpackPlugin
* ### 报错问题 ###  
    - 快速删除node-module
    npm install rimraf -g
    rimraf node_modules
 ![截图](/images/02.jpg)
    - 下载对应的win32-x64-xx_binding.node https://github.com/sass/node-sass/releases；
    告诉程序使用本地的.node文件，无需到网上下载
    set SASS_BINARY_PATH=D:/win32-x64-57_binding.node
    （解决问题思路：npm install，看到直接从网上下载48.node，所以有版本冲突报错）
    - 错误2
    Built files are meant to be served over an HTTP server. Opening index.html over file:// won't work.

    默认配置中, 把assetsPublicPath: '/'改成assetsPublicPath: './',dist文件夹里的文件必须放在服务器的根目录, 如果你想本地打开的话, 可以在npm run build完成之后执行以下命令:npm install -g http-server

    
    