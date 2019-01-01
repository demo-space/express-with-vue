# EXPRESS WITH VUE

最近有个项目，前后端分别用了 egg+umi，但是利用 egg-view-assets 这个 plugin，将前后端融合

怎么说呢？用 `egg-bin dev` 的命令启动项目，一个前端页面的 url 看起来这样 `http://localhost:7001/xxx/xxx`，一个后端 api 的路径看起来也是这样 `http://localhost:7001/xxx/xxx`，其实前端开的是另一个端口，然后将 js 塞进了后端模版里罢了，思路和我当时做 [技术号](https://segmentfault.com/mp) 功能有点类似（详见 [这里](https://www.cnblogs.com/zichi/p/8391993.html)）。普通的单页通常只有一个 index.html 页面，和普通单页不同的是，egg+umi，会将不同的路由映射到不同的 html 页面，也就是说，**同一个路由，既是后端路由，也是前端路由**

**这样的情况，会先匹配后端路由**

首先我们用命令 `npm run dev` 启动 express-demo 项目（默认 3000 端口），配置了两个路由 `/` 和 `/users`

然后我们用同样命令启动 vue-demo 项目（默认 4000 端口），配置了同样路由 `/` 和 `/users`。这个时候，内存中会存在一个 `http://localhost:4000/app.js` 的 js，我们将这个 js 放到 express 渲染的后端模版中，记得不要忘记加上 `<div id="app"></div>`，不然会找不到 root 元素

然后我们打开 `http://localhost:3000/` 看效果，这个时候点击页面中的 `users` 链接（不是后面那个强制跳转），url 变成 `http://localhost:3000/users`，但是 html 代码不会变化（单页），我们强刷页面，html 变化

html 代码不变，所以这个例子有个鸟用？所以实际中应该不会出现这种情况，**如果是后端路由设置的页面，前端路由之间应该是不会互通的**，也就是说如果后端路由设置了 `/` 以及 `/users`，那么前端路由之间，`/` 页面是不会路由到 `/users`，反之亦然，除非是 a 标签强刷类型的跳转

这点和实际项目中一致，项目中后端路由 `/manage/*` 以及 `/i`，分别对应 app 内的 webview 以及分享出去的页面，两个功能是完全不同的，不会互通，所以其实可以看作是两个单页应用