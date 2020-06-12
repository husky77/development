# 动态路由菜单加载

> 路由分为两种：一种存储在本地文件中，大部分为固定的页面功能菜单；另一种为根据用户角色动态加载的菜单包含相应按钮权限信息。

* 固定路由菜单在文件`src/router/router.js`中
* 动态路由加载方法主要在文件`src/libs/util.js`中，主要通过`router.addRoutes(routes)`实现路由动态添加

```text
util.initRouter = function (vm) {
    const constRoutes = [];
    const otherRoutes = [];

    // 404路由需要和动态路由一起加载
    const otherRouter = [{
        path: '/*',
        name: 'error-404',
        meta: {
            title: '404-页面不存在'
        },
        component: 'error-page/404'
    }];

    // 判断用户是否登录
    let userInfo = Cookies.get('userInfo')
    if (userInfo == null || userInfo == "" || userInfo == undefined) {
        // 未登录
        return;
    }
    let accessToken = window.localStorage.getItem('accessToken')
    // 加载菜单
    axios.get(getMenuList, { headers: { 'accessToken': accessToken } }).then(res => {
        let menuData = res.result;
        if (menuData == null || menuData == "" || menuData == undefined) {
            return;
        }
        // 顶部菜单
        let navList = [];
        menuData.forEach(e => {
            let nav = {
                name: e.name,
                title: e.title,
                icon: e.icon
            }
            navList.push(nav);
        })
        if (navList.length < 1) {
            return;
        }
        // 存入vuex
        vm.$store.commit('setNavList', navList);
        let currNav = window.localStorage.getItem('currNav')
        if (currNav) {
            // 读取缓存title
            for (var item of navList) {
                if (item.name == currNav) {
                    vm.$store.commit('setCurrNavTitle', item.title);
                    break;
                }
            }
        } else {
            // 默认第一个
            currNav = navList[0].name;
            vm.$store.commit('setCurrNavTitle', navList[0].title);
        }
        vm.$store.commit('setCurrNav', currNav);
        for (var item of menuData) {
            if (item.name == currNav) {
                // 过滤
                menuData = item.children;
                break;
            }
        }
        util.initRouterNode(constRoutes, menuData);
        util.initRouterNode(otherRoutes, otherRouter);
        // 添加主界面路由
        vm.$store.commit('updateAppRouter', constRoutes.filter(item => item.children.length > 0));
        // 添加全局路由
        vm.$store.commit('updateDefaultRouter', otherRoutes);
        // 刷新界面菜单
        vm.$store.commit('updateMenulist', constRoutes.filter(item => item.children.length > 0));

        let tagsList = [];
        vm.$store.state.app.routers.map((item) => {
            if (item.children.length <= 1) {
                tagsList.push(item.children[0]);
            } else {
                tagsList.push(...item.children);
            }
        });
        vm.$store.commit('setTagsList', tagsList);
    });
};

// 生成路由节点
util.initRouterNode = function (routers, data) {

    for (var item of data) {
        let menu = Object.assign({}, item);
        // menu.component = import(`@/views/${menu.component}.vue`);
        menu.component = lazyLoading(menu.component);

        if (item.children && item.children.length > 0) {
            menu.children = [];
            util.initRouterNode(menu.children, item.children);
        }

        let meta = {};
        // 给页面添加权限、标题、第三方网页链接
        meta.permTypes = menu.permTypes ? menu.permTypes : null;
        meta.title = menu.title ? menu.title + " - XBoot前后端分离开发平台 By: Exrick" : null;
        meta.url = menu.url ? menu.url : null;
        menu.meta = meta;

        routers.push(menu);
    }
};
```

> **注意**：404路由需要和动态路由一起加载，避免直接访问动态路由路径链接进入页面时，动态路由还未加载导致先跳转到404错误页

* 加载组件`src/libs/lazyLoading.js`

```text
export  default (url) =>()=>import(`@/views/${url}.vue`)
```

* 存入Vuex

```text
// 动态添加主界面路由，需要缓存
updateAppRouter(state, routes) {
    state.routers.push(...routes);
    router.addRoutes(routes);
},
// 动态添加全局路由404、500等页面，不需要缓存
updateDefaultRouter(state, routes) {
    router.addRoutes(routes);
}
updateMenulist(state, routes) {
    state.menuList  =  routes;
},
```

* `src/main.js`中进行调用

```text
new Vue({
    el: '#app',
    render: h => h(App),
    mounted() {
        // 初始化菜单
        util.initRouter(this);
    }
})
```

