---
title: 简易版 vuex
url: https://www.yuque.com/wcywxq/pgkpvh/myzomg
---

<a name="q6cxH"></a>

## store 注册

```javascript
let Vue

// Vue 插件必须要有 install 函数
export function install(_Vue) {
	Vue = _Vue // 拿到 Vue 的构造器，存起来
  // 通过 mixin 注入到每一个 vue 实例
  Vue.mixin({ beforeCreate: vuexInit })
  
  function vuexInit() {
  	const options = this.$options
    if (options.store) {
    	this.$store = typeof options.store === 'function'
      	? options.store() : options.store
    } else if (options.parent && options.parent.$store) {
    	this.$store = options.parent.$store
    }
  }
}
```

<a name="NDHYk"></a>

## store 的响应式

```javascript
export class Store {
  constructor(options = {}) {
    const state = options.state;

    resetStoreVM(this, state);

    // 利用 getters 来收集衍生数据 computed

    this.getters = {};

    _.forEach(this.getters, (name, getterFn) => {
      Object.defineProperty(this.getters, name, {
        get: () => getterFn(this.state),
      });
    });

    // 定义的行为，分别对应同步和异步行为处理'
    this.mutations = {};
    this.actions = {};

    _.forEach(options.mutations, (name, mutation) => {
      this.mutations[name] = (payload) => {
        mutation(this.state, payload);
      };
    });

    _.forEach(options.actions, (name, action) => {
      this.actions[name] = (payload) => {
        action(this, payload);
      };
    });

    this._modules = new ModuleCollection(options);
    installModule(this, this.state, [], this._modules.root);
  }

  get state() {
    return this._vm._data.$$state;
  }

  commit(type, payload) {
    this.mutations[type](payload);
  }

  dispatch(type, payload) {
    this.actions[type](payload);
  }
}

function resetStoreVM(store, state) {
  // 因为 vue 实例的 data 是响应式的，因此可以利用这一点，实现 state 的响应式
  store._vm = new Vue({
    data: {
      $$state: state,
    },
  });
}

function installModule(store, state, path, root) {
  // getters
  const getters = root._rawModule.getters;
  if (getters) {
    _.forEach(getters, (name, getterFn) => {
      Object.defineProperty(store.getters, name, {
        get: () => getterFn(root.state),
      });
    });
  }

  // mutations
  const mutations = root._rawModule.mutations;
  if (mutations) {
    _.forEach(mutations, (name, mutation) => {
      let _mutations = store.mutations[name] || (store.mutations[name] = []);
      _mutations.push((payload) => {
        mutation(root.state, payload);
      });

      store.mutations[name] = _mutations;
    });
  }

  // actions
  const actions = root._rawModule.actions;
  if (actions) {
    _.forEach(actions, (name, action) => {
      let _actions = store.actions[name] || (store.actions[name] = []);
      _actions.push((payload) => {
        action(store, payload);
      });

      store.actions[name] = _actions;
    });
  }

  // 递归
  _.forEach(root._children, (name, childModule) => {
    installModule(this, this.state, path.concat(name), childModule);
  });
}
```

<a name="JSDhH"></a>

## 多个 module 拆分

```javascript
class Module {
	constructor(rawModule) {
  	this.state = rawModule || {}
    this._rawModule = rawModule
    this._children = {}
  }
  
  getChild(key) {
  	return this._children[key]
  }
  
  addChild(key, module) {
  	this._children[key] = module
  }
}

// module-collection 用来收集 module
class ModuleCollection {
	constructor(options = {}) {
  	this.register([], options)
  }
  
  register(path, rawModule) {
  	const newModule = new Module(rawModule)
    if (path.length === 0) {
    	this.root = newModule
    } else {
    	const parent = path.slice(0, -1).reduce((module, key) => {
      	return module.getChild(key)
      }, this.root)
      parent.addChild(path[path.length - 1], newModule)
    }
    
    // 如果有 modules，开始递归注册
    if (rawModule.modules) {
    	_.forEach(rawModule.modules, (key, rawChildModule) => {
      	this.register(path.concat(key), rawChildModule)
      })
    }
  }
}
```
