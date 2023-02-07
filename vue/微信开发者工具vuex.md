# 微信开发者工具中使用vue devtools

项目中安装以下两个模块

```shell
npm install @vue/devtools -D # 安装 vue devtools
npm install npm-run-all -D # 便于启动
```

public/index.html中增加

```html
<% if (NODE_ENV === 'development') { %>
  <script src="http://localhost:8098"></script>
<% } %>
```

package.json中配置启动命令

```json
"scripts": {
  "start-devtools": "vue-devtools",
  "dev": "vue-cli-service serve --mode development",
  "devtools": "run-p start-devtools dev"
}
```

启动项目

```shell
npm run devtools
```
