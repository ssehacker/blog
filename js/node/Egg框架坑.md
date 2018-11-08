# Egg框架踩坑记录.md

### Node版本
官网上说支持`Node8`及以上版本，但是在linux上运行`npm install`的会报错:
```
> egg-ci@1.8.0 postinstall /starlab/www/test/node/egg-example/node_modules/egg-ci
> node install.js

fs.js:646
  return binding.open(pathModule._makeLong(path), stringToFlags(flags), mode);
                 ^

Error: EACCES: permission denied, open '/starlab/www/test/node/egg-example/.travis.yml'
    at Object.fs.openSync (fs.js:646:18)
    at Object.fs.writeFileSync (fs.js:1299:33)
    at Object.<anonymous> (/starlab/www/test/node/egg-example/node_modules/egg-ci/install.js:87:6)
    at Module._compile (module.js:652:30)
    at Object.Module._extensions..js (module.js:663:10)
    at Module.load (module.js:565:32)
    at tryModuleLoad (module.js:505:12)
    at Function.Module._load (module.js:497:3)
    at Function.Module.runMain (module.js:693:10)
    at startup (bootstrap_node.js:188:16)
npm WARN co-mocha@1.2.2 requires a peer of mocha@>=1.18 <6 but none is installed. You must install peer dependencies yourself.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.3 (node_modules/fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.3: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})

npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! egg-ci@1.8.0 postinstall: `node install.js`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the egg-ci@1.8.0 postinstall script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /starlab/server/node-cache/_logs/2018-05-15T08_20_56_284Z-debug.log
```

后面经过升级为版本`node > 8.4`就OK了。

### QueryString处理问题
理想状态下，我们如果接口请求是`GET /foo?p=a&q=foo&q=bar`，则应该`ctx.request.query`解析为:
```json
{
	"p": "a",
	"q": ["foo", "bar"]
}
```

但是`egg.js`会把第二个`q=bar`舍去，解析为`ctx.query`：
```json
{
	"p": "a",
	"q": "foo"
}
```

同时会添加一个对象`ctx.queries`，并完全递归按数组解析为：
```json
{
	"p": ["a"],
	"q": ["foo", "bar"]
}
```

作者解释原因参见[这里](https://github.com/eggjs/egg/issues/1645)：
>由于解析多层级的 querystring 存在被攻击的风险，而对于每一个 query 的值可能是 string 又可能是 array 容易导致业务逻辑在处理时忽略而导致报错（例如 ctx.query.foo.slice() 当 foo 是一个数组时会报错），所以 egg 默认处理为要么为 string，要么全为 array，且默认 ctx.query 只取一级。

个人觉得这么处理不太好，还是喜欢之前社区有个中间件[koajs/qs](https://github.com/koajs/qs) 的处理。或者直接使用[ljharb/qs](https://github.com/ljharb/qs)来处理。不然的话，只能数组对象从`ctx.queries`中取，非数组对象从`ctx.query`中取值了。




