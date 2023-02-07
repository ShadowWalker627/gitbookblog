# 日志

```js
// app/controller/user.js
class UserController extends Controller {
  async list() {
    const { app, ctx } = this;
    ctx.logger.info('ctx.logger');
    // 会输出到 logs/app_name/app_name-web.log
    // 2023-01-11 16:00:53,122 INFO 2932 [-/192.168.48.55/-/336ms GET /wechat?url=http://192.168.48.55:8081/blank] [controller.wechat] 日志内容
    // 时间 log类型  ip  请求类型 请求url  文件路径 日志内容
    this.logger.info('this.logger');
    // 会输出到 logs/app_name/app_name-web.log 这个没有带上日志的文件路径
    // 时间 log类型  ip  请求类型 请求url 日志内容
    ctx.logger.info(`ctx.logger: ${signatureStr}`);
    ctx.body = [ { name: 'TZ' } ];
  }
}
```
