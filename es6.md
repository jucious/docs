### ES6教程
* #### 数组 #### 
    - 数组实例的 entries()，keys() 和 values()
    ``` js
        //遍历key、value 记得加括号
        let index of arr.keys()
        let ele of arr.values()
        let [index,ele] of arr.entries()
    ```
* #### 对象 ####
    - 可枚举性
        obj.keys()

* #### async函数 ####
    - async 返回一个promise对象
        async内部return的值，会成为then函数的参数。
        ```
        async function f() {
        return 'hello world';
        }
        f().then(v => console.log(v))
        ```
    - await只能出现在async函数里面
        await异步完成之后，async才会返回promise对象。 