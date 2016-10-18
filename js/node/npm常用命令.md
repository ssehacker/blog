# npm 常用命令
#### npm link

开发NPM模块的时候，有时我们会希望，边开发边试用，比如本地调试的时候，require('myModule')会自动加载本机开发中的模块。Node规定，使用一个模块时，需要将其安装到全局的或项目的node_modules目录之中。对于开发中的模块，解决方法就是在全局的node_modules目录之中，生成一个符号链接，指向模块的本地目录。

npm link就能起到这个作用，会自动建立这个符号链接。

请设想这样一个场景，你开发了一个模块myModule，目录为src/myModule，你自己的项目myProject要用到这个模块，项目目录为src/myProject。首先，在模块目录（src/myModule）下运行npm link命令。

```
src/myModule$ npm link
```

上面的命令会在NPM的全局模块目录内，生成一个符号链接文件，该文件的名字就是package.json文件中指定的模块名。

```
/path/to/global/node_modules/myModule -> src/myModule
```

这个时候，已经可以全局调用myModule模块了。但是，如果我们要让这个模块安装在项目内，还要进行下面的步骤。

切换到项目目录，再次运行npm link命令，并指定模块名。

```
src/myProject$ npm link myModule
```
上面命令等同于生成了本地模块的符号链接。

```
src/myProject/node_modules/myModule -> /path/to/global/node_modules/myModule
```
然后，就可以在你的项目中，加载该模块了。

```
var myModule = require('myModule');
```
这样一来，myModule的任何变化，都可以直接反映在myProject项目之中。但是，这样也出现了风险，任何在myProject目录中对myModule的修改，都会反映到模块的源码中。

如果你的项目不再需要该模块，可以在项目目录内使用npm unlink命令，删除符号链接。

```
src/myProject$ npm unlink myModule
```

>以上内容来自[`阮一峰`的博客](http://javascript.ruanyifeng.com/nodejs/npm.html#toc18)，他写的比较通俗易懂，我就直接搬过来了。

值得注意的是：使用时npm link时，如果你能正常运行，但是在别人电脑上npm install myModule却遇到某些package找不到，很有可能是因为这些package在myModule 是以--save-dev的形式安装的。你可以把它改为: --save，这样别人安装你这个模块的时候，会把dependencies下的包download下来。但是devDependencies的不会。

#### 如何发布package
1.登陆npm并输入用户名和密码
```
npm login
```
确认是否为自己：
```
npm config ls
```

2.升级package
```
npm version <update_type>
```
这里的update_type 是该版本号，如: 0.0.3，
经过这个操作后，package.json 中version就变为了 0.0.3。

3.发布package
```
npm publish
```
发布成功后，在https://npmjs.com/package/<package> 下可找到自己的package

#### 配置npm镜像的源
有的时候，npm package直接从官网上下载会比较慢，这个时候，我们可以自己通过[rlidwka/sinopia](https://github.com/rlidwka/sinopia) 搭建一个代理服务器，然后把源配置为我们服务器的IP。
不过，淘宝已经有一个公用的源， 地址是： `https://registry.npm.taobao.org`

我们可以在安装某个package的时候，附加参数registry：
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

也可以全局设置:
```
npm config set registry https://registry.npm.taobao.org
```
不需要的时候，需要手动删除：
```
npm config delete registry
```


![npm](https://www.npmjs.com/static/images/collaboration-security.svg)

