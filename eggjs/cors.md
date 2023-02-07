# cors

eggjs添加egg-cors插件，解决跨域问题

需要在下面两个文件中增加内容

```js
// config/plugin.js
module.exports = {
  cors: {
    enable: true,
    package: 'egg-cors',
  },
};
```

```js
// config config.default.js
module.exports = appInfo => {
  /**
   * built-in config
   * @type {Egg.EggAppConfig}
   **/
  const config = exports = {};

  const userConfig = {
    security: {
      domainWhiteList: ['http://192.168.48.55:8081'],
    },
    cors: {
      origin: '*',
      allowMethods: 'GET,HEAD,PUT,POST,DELETE,PATCH',
    },
  };

  return {
    ...config,
    ...userConfig,
  };
};
```
