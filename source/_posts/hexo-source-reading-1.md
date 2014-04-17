title: hexo源码解析系列（一）
date: 2014-03-02 13:54:23
tags:
categories:
keywords: hexo,源码

---

[前一篇](/blog/2014/02/28/hexo-source-reading-0/)分析bin目录下`hexo`文件的作用。本章着重分析hexo的初始化。

## Hexo类
### 方法
- bootstrap
- call
- constant

### 属性
- base_dir
- config
- core_dir
- env
- extend
- file
- lib_dir
- locals
- model
- plugin_dir
- post
- public_dir
- render
- rout
- scaffold
- scaffold_dir
- source
- source_dir
- theme
- theme_dir
- theme_script_dir
- util
- version

### 事件
- exit
- generateAfter
- generateBefore
- new
- processAfter
- processBefore
- ready
- server

## 初始化

bin目录下的`hexo`文件将当前工作目录`cwd`和命令行参数`args`传递给lib目录的`init`模块后，`init`模块执行初始化操作。
初始化的过程：

- 实例化Hexo类  
- 通过async模块加载各模块  
- 调用hexo对象的call方法执行命令。

``` js
module.exports = function(cwd, args, callback) {
  if (typeof callback !== 'function') callback = function() {};

  // 实例化Hexo类
  var hexo = global.hexo = new Hexo();
  var configfile = args.config || '_config.yml';

  // 初始化环境
  hexo.bootstrap(cwd, args);
  hexo.configfile = path.join(hexo.base_dir, configfile);

  // @sea https://github.com/caolan/async#eachSeries
  // 加载loaders目录下地模块
  async.eachSeries([
    'logger',
    'extend',
    'config',
    'update',
    'database',
    'plugins',
    'scripts'
  ], function(name, next) {
    require('./loaders/' + name)(next);
  }, function(err) {
    if (err) throw err;

    /**
     * Fired when Hexo is ready.
     *
     * @event ready
     * @for Hexo
     */

    hexo.emit('ready');

    // e.g., new, generate, deploy, publish.
    var command = args._.shift();

    if (command) {
      var c = hexo.extend.console.get(command);

      if (!c || (!hexo.env.init && !c.options.init)) {
        command = 'help';
      }
    } else {
      command = 'help';
    }

    if (hexo.env.silent && command === 'help') return callback();

    // 执行命令
    hexo.call(command, args, function(err) {
      if (err) hexo.log.e(err);

      /**
       * Fired when Hexo is about to exit.
       *
       * @event exit
       * @for Hexo
       */

      hexo.emit('exit');

      if (!err) return process.exit(0);

      var logPath = path.join(hexo.base_dir, 'debug.log'),
        FileStream = Logger.stream.File;

      async.series([

        function(next) {
          FileStream.prepare(logPath, next);
        },
        function(next) {
          FileStream.dump(logPath, hexo.log, next);
        }
      ], function(err) {
        if (err) return log.e(err);

        process.exit(1);
      });
    });
  });
};
```