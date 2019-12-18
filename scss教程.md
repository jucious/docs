## scss教程 ##
* #### 使用变量 ####
    ``` scss
        $link-color: blue;
        a {
        color: $link_color;
        }
        _等同于-
    ```
* #### 嵌套规则 ####
    - 父选择器的标识&
    ``` scss
        #content aside {
            color: red;
            body.ie & { color: green }
        }
        ps：可以在&之前添加选择器
    ```
    - 子选择器、兄弟选择器
     ``` scss
        article {
            ~ article { border-top: 1px dashed #ccc }
            > section { background: #eee }
            dl > {
                dt { color: #333 }
                dd { color: #555 }
            }
            nav + & { margin-top: 0 }
        }
    ```
    - 嵌套属性
    ``` scss
        p {
            font{
                size:12px;
                style:normal
            }
        }
    ```



    ```
* #### rules与指令  #### 
    - import导入scss
    "@import "rounded-corners", "text-shadow";"
    - 局部嵌套
        example.scss 
        ``` scss
        .example {
            color: red;
        }
        ```
        main.scss
        ``` scss
            .main{
                @import "example"
            }
        ```
    - media嵌套
        ``` css
        .sidebar {
            width: 300px;
            @media screen and (orientation: landscape) {
                width: 500px;
            }
        }
        //嵌套在选择器里
        ```
        ``` css
        @media screen {
            .sidebar {
                @media (orientation: landscape) {
                width: 500px;
                }
            }
        }
        //@media 的 queries 允许互相嵌套使用，编译自动加and，注意不要漏了@media
        ```
    - @extend
        ``` css
        .error {
            border: 1px #f00;
            background-color: #fdd;
            }
            .seriousError {
            @extend .error;
            border-width: 3px;
        }//利用@extent，.seriousError就可以继承.error的样式，html就不需要写两个类。

        //还可以延展到更复杂的选择器
            .hoverlink {
                @extend a:hover;
            }
        ```


* #### 混合指令  ####    
    - @mixin @include
    @mixin定义可以重复使用的scss
    ``` css
        //可以传参
        @mixin sexy-border($color, $width) {
            border: {
                color: $color;
                width: $width;
                style: dashed;
            }
        }
        //用@include来引用
        p { @include sexy-border(blue, 1in); }
        //不确定参数数量时候（参数name...）
    ```