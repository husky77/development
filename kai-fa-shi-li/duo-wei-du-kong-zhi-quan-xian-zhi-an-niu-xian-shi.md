# 多维度控制权限至按钮显示

### 按钮权限

> 将用户操作权限列表存入路由数据结构的meta字段中，注册全局指令，当点初次渲染时，判断传入的参数是否在当前路由页面权限列表中，若不存在，则直接删除该节点

* 动态路由JSON数据示例

```text
component: "access/access"
icon: "md-lock"
id: "16392767785668608"
level: 2
name: "access_index"
parentId: "16392452747300864"
path: "index"
permTypes: ["add", "edit", "delete"]
title: "权限按钮测试页"
```

* 从后台读取菜单数据构造路由节点时将permTypes存入，详见`src/libs/util.js`

```text
// 生成路由节点
util.initRouterNode = function (routers, data) {
    ...
     // 给页面添加权限
    meta.permTypes = menu.permTypes ? menu.permTypes : null;
    ...
};
```

* 定义全局命令组件，判断页面拥有权限删除节点，详见`src/libs/hasPermission.js`

```text
const hasPermission = {
    install (Vue, options) {
        Vue.directive('has', {
            inserted (el, binding, vnode) {
                let permTypes = vnode.context.$route.meta.permTypes;
                if (!permTypes.includes(binding.value)) {
                    el.parentNode.removeChild(el);
                }
            }
        });
    }
};

export default hasPermission;
```

* 全局注册组件，详见`src/main.js`

```text
import  hasPermission  from  '@/libs/hasPermission'

Vue.use(hasPermission);
```

> 上述方式的优点：每个页面只处理其拥有的权限，无大量权限数据需要判断  
> 缺点：不在动态路由中的页面节点不受控制，解决方案：见下方使用角色权限

### 角色权限

> 原理大致同上，该方式虽然不能细化权限控制，但其优点为可全局直接使用，无需配置

* 用户成功登录后将其角色信息存入LocalStorage中，详见`src/views/login.vue`

```text
this.setStore("roles", roles);
```

* 定义全局命令组件，判断页面拥有权限删除节点，详见`src/libs/hasRole.js`

```text
import { getStore } from './storage';

const hasRole = {
    install (Vue, options) {
        Vue.directive('hasRole', {
            inserted (el, binding) {
                let roles = getStore("roles");
                if (!roles.includes(binding.value)) {
                    el.parentNode.removeChild(el);
                }
            }
        });
    }
};

export default hasRole;
```

* 全局注册组件，详见`src/main.js`

```text
import  hasRole  from  '@/libs/hasRole'

Vue.use(hasRole);
```

### 扩展

> 如需使用多个权限为与、或规则，自行参考下方代码使用，由于作者没有用到，有需要的自行加入代码

```text
// 必须包含列出的所有权限，元素才显示
export const hasPermission = {
  install (Vue) {
    Vue.directive('hasPermission', {
      bind (el, binding, vnode) {
        let permTypes = vnode.context.$route.meta.permTypes;
        let value = binding.value.split(',')
        let flag = true
        for (let v of value) {
          if (!permTypes.includes(v)) {
            flag = false
          }
        }
        if (!flag) {
            el.parentNode.removeChild(el)
        }
      }
    })
  }
}

// 当不包含列出的权限时，渲染该元素
export const hasNoPermission = {
  install (Vue) {
    Vue.directive('hasNoPermission', {
      bind (el, binding, vnode) {
        let permTypes = vnode.context.$route.meta.permTypes;
        let value = binding.value.split(',')
        let flag = true
        for (let v of value) {
          if (permTypes.includes(v)) {
            flag = false
          }
        }
        if (!flag) {
            el.parentNode.removeChild(el)
        }
      }
    })
  }
}

// 只要包含列出的任意一个权限，元素就会显示
import { getStore } from './storage';

export const hasAnyPermission = {
  install (Vue) {
    Vue.directive('hasAnyPermission', {
      bind (el, binding, vnode) {
        let permissions = vnode.context.$store.state.account.permissions
        let value = binding.value.split(',')
        let flag = false
        for (let v of value) {
          if (permissions.includes(v)) {
            flag = true
          }
        }
        if (!flag) {
            el.parentNode.removeChild(el)
        }
      }
    })
  }
}

// 必须包含列出的所有角色，元素才显示
import { getStore } from './storage';

export const hasRole = {
  install (Vue) {
    Vue.directive('hasRole', {
      bind (el, binding, vnode) {
        let roles = getStore("roles");
        let value = binding.value.split(',')
        let flag = true
        for (let v of value) {
          if (!roles.includes(v)) {
            flag = false
          }
        }
        if (!flag) {
            el.parentNode.removeChild(el)
        }
      }
    })
  }
}

// 只要包含列出的任意一个角色，元素就会显示
export const hasAnyRole = {
  install (Vue) {
    Vue.directive('hasAnyRole', {
      bind (el, binding, vnode) {
        let roles = getStore("roles");
        let value = binding.value.split(',')
        let flag = false
        for (let v of value) {
          if (roles.includes(v)) {
            flag = true
          }
        }
        if (!flag) {
            el.parentNode.removeChild(el)
        }
      }
    })
  }
}
```

* 以上方式使用示例：

```text
<div v-hasPermission="'add','edit'">XBoot</div>
<div v-hasAnyPermission="'add','edit'">XBoot</div>
<div v-hasRole="'ROLE_ADMIN','ROLE_USER'">XBoot</div>
<div v-hasAnyRole="'ROLE_ADMIN','ROLE_USER'">XBoot</div>
```

