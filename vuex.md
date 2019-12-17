* ### vuex教程 ###
    * #### 项目结构 ####
        ├── index.html
    ├── main.js
    ├── api
    │   └── ... # 抽取出API请求
    ├── components
    │   ├── App.vue
    │   └── ...
    └── store
        ├── index.js          # 我们组装模块并导出 store 的地方
        ├── actions.js        # 根级别的 action
        ├── mutations.js      # 根级别的 mutation
        ├── mutations-type.js # 根级别的 mutation常量 
        └── modules 
            ├── cart.js       # 购物车模块
            └── products.js   # 产品模块
    ![vuex](./images/vuex.png)
    * #### state ####
        state在computed中返回某个状态，使组件可以展示state
        ```
            export defaut{
                computed:{
                    count:this.$store.state.count
                }
            }
        ```
        使用mapState，帮助生成计算属性，mapState返回的是一个对象，使用...对象展开符，将多个对象合并成一个。
        ```
            import {mapState} from 'vuex'
            export defaut{
                computed:{
                    ...mapState({
                        count:state=>state.count//箭头函数省去return
                    })
                }
            }
           //如果 this.count 为 store.state.count,可以简写。
            export defaut{
                computed:{
                    ...mapState({
                        'count'
                    })
                }
            }
        ```
     * #### Getter ####
        Getter相当于computed，对数据进行处理，例如过滤等，并不改变state状态。
        ```js
            const store = new Vuex.store({
                state:{
                    count:[1,2,3,4,4]
                },
                getter:{
                    todosCount:state=>{
                        state.count.filter(item=>item>3)
                    }
                }
            })
            //arr.filter(function(item,index,arr){
                return item>1;
            })
        ```
        mapGetters 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性：
        ```js
        import {mapGetters} from 'vuex'
        computed:{
            ...mapGetters([
                'todosCount'
            ]}
        }
        //取别的名字，使用对象形式
        computed:{
            ...mapGetters({
                count:'todosCount'
            })
        }
        ```
      * #### Mutation ####
      提交Mutation是唯一可以改变state的方法。
    ``` js
      const store = new Vuex.store({
        state:{
            count:1
        },
        mutation:{
            //使用常量作为函数名
            [COUNT_ADD](state){
                state.count++
            }
        }
      })
      //在组件中提交mutation,methods映射为this.$store.commit('increment')，！需要在根节点注入store。
      methods(){
          ...mapMutations([
              'COUNT_ADD'
          ])
      }
    ```