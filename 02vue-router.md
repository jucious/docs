- App.vue是我们的主组件，页面入口文件 ，所有页面都是在App.vue下进行切换的。也是整个项目的关键，app.vue负责构建定义及页面组件归集。
* ### vue-router使用 ###
    - npm install vue-router
    - router.js
    ``` js
    import VueRouter from 'vue-router'
    import Vue from 'vue'
    Vue.use(VueRouter)//全局使用router
    export defaut new VueRouter({
        routes:[{
            path:'/home',
            components:home,
            name:'name'
        },
        {
            path: '',
            redirect: '/home'//重定向
        }]
    })
    ``` 
    - main.js
    ``` js
        import router from ''
        new Vue({
            router
        }).$mount('#app)
    ```
    - html
    router-link传入'to'指定链接，渲染成一个a链接；router-view用于渲染路由匹配的组件。
    - 路由组件传参
        ```
            new VueRouter({
                routes:[
                    {path:'city/:id',component:'city'}
                ]
            })
            //在页面中获取参数：this.$route.params.id
            //注意使用v-bind
            //父页面：<router-link v-bind:to="/city/+'item.id'"></router-link>
        ```
    - Router的实例方法
        - $.router.go()
        - $.router.back()
        - $.router.forward()
        - $.router.push()
        ps:$router是指全局router的实例，而$route是当前激活路由的状态信息，$route.params.name可以获取路由的参数。
    - router-link props
        ``` html
        //带查询参数
        <router-link :to="{path:'/login',query:{name:username}}"></router-link>
        <!-- 命名的路由 -->
        <router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>  
        ```
        - 命名视图
            ``` js
                //tab切换，一个页面有多个组件
                const router = new VueRouter({
                routes: [
                    {
                    path: '/',
                    components: {
                        default: Foo,
                        a: Bar,
                        b: Baz
                    }
                    }
                ]
                })
                //设置router-link的
                <router-view class="view one"></router-view>
                <router-view class="view two" name="a"></router-view>
                <router-view class="view three" name="b"></router-view>
            ```
* ### 相关扩展 ###
    - es6-import、export、export defaut
    export 可以输出变量、function、class
    ```
    export{a as b} //将a重命名为b
    ```
    ps:import和export都注意添加{}、import导入都模块名必须与export输出的模块名保持一致。
    - import 整体输入模块
    import * as module from 'src'
    执行方法：*.m1()，通过\*去获取src中的方法。
    - export defaut
    相当于 export a as defaut,export defaut+一个变量，变量需提前定义；可以+匿名或非匿名函数。



