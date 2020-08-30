# Vuex

## 1.What

- 组件**共享数据**（状态）集中管理
- 数据是**响应式**的

![vuex](Vuex.assets/vuex.png)

## 2.安装并引入

```bash
npm i vuex
```

`main.js`

```javascript
import Vue from 'vue'
import store from './store'

new Vue({
  // ...
  store
})
```

`store.js`

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
  state: { ... },
  getters: { ... },
  mutations: { ... },
  actions: { ... }
})

export default store
```

## 3.state

> 状态（数据），相当于`data`

### 访问

```javascript
this.$store.state.%data%
```

```javascript
import { mapState } from 'vuex'

computed: {
  ...mapState([ '%data1%', '%data2%', ... ])
}
```

```javascript
import { mapState } from 'vuex'

computed: {
  ...mapState({
    // rename
    countAlias: 'count',

    // function
    count: state => state.count,

    // compute
    countPlusLocalState(state) {
      return state.count + this.localCount
    }
  })
}
```

## 4.getter

> 相当于`computed`

### 定义

```javascript
getters: {
  %name%(state, getters) => {
    return ...
  }
}
```

### 访问

```javascript
this.$store.getters.%data%
```

```javascript
import { mapGetters } from 'vuex'

computed: {
  // 同 state
  ...mapGetters( ... )
}
```

## 5.mutation

> 只能通过 `mutation` 修改 `state`。且不能包含异步操作

### 定义

```javascript
mutations: {
  %name%(state, payload) {
    state.count = payload
  }
}
```

### 访问

```javascript
this.$store.commit('%mutation%', payload)
```

```javascript
import { mapMutations } from 'vuex'

methods: {
  ...mapMutations( ... )
}
```

## 6.action

> 相当与 `mutation` ，但提交的是 `mutation`，而不是直接变更 `state`。可以包含任意异步操作

### 定义

```javascript
actions: {
  %name%(context, payload) {
    // context.commit / context.dispatch / context.state / context.getter
    // ... 异步操作
    context.commit('%mutation%')
  }
}
```

### 访问

```javascript
this.$store.dispatch('%action%', payload)
```

```javascript
import { mapActions } from 'vuex'

methods: {
  ...mapActions( ... )
}
```

### 返回 `Promise`

```javascript
actions: {
  actionA() {
    return new Promise( ... )
  }
}
```

```javascript
this.$store.dispatch('actionA').then( ... )
```

## 7.module

> 将 store 分割成模块

```javascript
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```

### 模块动态注册

```javascript
import Vuex from 'vuex'

const store = new Vuex.Store({ /* 选项 */ })

// 注册模块 `myModule`
store.registerModule('myModule', {
  // ...
})

// 注册嵌套模块 `nested/myModule`
store.registerModule(['nested', 'myModule'], {
  // ...
})
```

你也可以使用 `store.unregisterModule(moduleName)` 来动态卸载模块。注意，你不能使用此方法卸载静态模块（即创建 store 时声明的模块）。

注意，你可以通过 `store.hasModule(moduleName)` 方法检查该模块是否已经被注册到 store。

### action

根节点 `state`： `context.rootState`

根节点 `getter`： `context.rootGetters`

根节点 `commit` 和 `dispatch`

```javascript
actions: {
  // 在这个模块中，dispatch 和 commit 也被局部化了
  // 他们可以接受 `root` 属性以访问根 dispatch 或 commit
  someAction({ dispatch, commit }) {
    dispatch('someOtherAction') // -> 'foo/someOtherAction'
    dispatch('someOtherAction', null, { root: true }) // -> 'someOtherAction'

    commit('someMutation') // -> 'foo/someMutation'
    commit('someMutation', null, { root: true }) // -> 'someMutation'
  }
}
```

注册全局 `action`

```javas
foo: {
  namespaced: true,

  actions: {
    someAction: {
      root: true,
      handler(namespacedContext, payload) { ... } // -> 'someAction'
    }
  }
}
```

### getter

```javascript
%getterName%(state, getters, rootState, rootGetters) {
  ...
}
```

### 命名空间

默认情况下，模块内部的 `action`、`mutation` 和 `getter` 是注册在**全局命名空间**的——这样使得多个模块能够对同一 `mutation` 或 `action` 作出响应。

如果希望你的模块具有更高的封装度和复用性，你可以通过添加 `namespaced: true` 的方式使其成为带命名空间的模块。当模块被注册后，它的所有 `getter`、`action` 及 `mutation` 都会自动根据模块注册的路径调整命名。

```javascript
const store = new Vuex.Store({
  modules: {
    account: {
      namespaced: true,
      // 模块内容（module assets）
      state: () => ({ ... }), // 模块内的状态已经是嵌套的了，使用 `namespaced` 属性不会对其产生影响
      getters: {
        isAdmin() { ... } // -> getters['account/isAdmin']
      },
      actions: {
        login() { ... } // -> dispatch('account/login')
      },
      mutations: {
        login() { ... } // -> commit('account/login')
      },

      // 嵌套模块
      modules: {
        // 继承父模块的命名空间
        myPage: {
          state: () => ({ ... }),
          getters: {
            profile() { ... } // -> getters['account/profile']
          }
        },
      }
    }
})
```

#### 带命名空间的绑定函数

```javascript
computed: {
  ...mapState('some/nested/module', {
    a: state => state.a,
    b: state => state.b
  })
},
methods: {
  ...mapActions('some/nested/module', [
    'foo', // -> this.foo()
    'bar' // -> this.bar()
  ])
}
```

或

```javascript
import { createNamespacedHelpers } from 'vuex'
const { mapState, mapActions } = createNamespacedHelpers('some/nested/module')
```



## 8.项目结构

```bash
├── main.js
└── store
    ├── index.js          # 我们组装模块并导出 store 的地方
    ├── actions.js        # 根级别的 action
    ├── mutations.js      # 根级别的 mutation
    ├── getters.js        # 根级别的 getter
    ├── mutation-types.js # mutation 事件类型常量
    ├── action-types.js   # action 事件类型常量
    └── modules
        ├── moduleA.js
        └── moduleB.js
```

## 9.插件

### vuex-persistedstate（vuex数据持久化）

> github: https://github.com/robinvdvleuten/vuex-persistedstate

```bash
npm i vuex-persistedstate
```

```javascript
import persistedState from 'vuex-persistedstate'

export default new Vuex.Store({
    // ...
    plugins: [persistedState()]
})
```

