# npm包管理器详解

#### npm与nvm的关系
npm是node的包管理器，而nvm是用来管理npm和node版本的，利用nvm，你可以在同一台机器上方便的切换node／npm的版本，便于在多个版本间调试代码。

值得注意的是，npm2 和 npm3之间有比较重大的升级，其中一个主要的更改：
>While npm2 installs all dependencies in a nested way, npm3 tries to mitigate the deep trees and redundancy that such nesting causes. npm3 attempts this by installing some secondary dependencies (dependencies of dependencies) in a flat way, in the same directory as the primary dependency that requires it.[参见官网](https://docs.npmjs.com/how-npm-works/npm3)

通俗一点的说法就是： 假设有一个package C 依赖于B,那么`npm install C `之后，如果是`npm2`，那么会在node_module下面只会找到C模块，在node_module/C/node_module下面才能找到B模块。 而`npm3`安装，则在node_module能找到C模块以及它依赖的B模块。
![示意图](https://docs.npmjs.com/images/npm3deps4.png)
#### 卸载全局安装的node
如果你是在官网上下载的node，那么它默认是全局安装的。
node path：  `/usr/local/bin/node`
npm path: `/usr/local/lib/node_modules/npm`
可以利用如下命令卸载：
```
npm ls -g --depth=0 #查看已经安装在全局的模块，以便删除这些全局模块后再按照不同的 node版本重新进行全局安装

sudo rm -rf /usr/local/lib/node_modules #删除全局 node_modules 目录
sudo rm /usr/local/bin/node #删除 node
cd  /usr/local/bin && ls -l | grep "../lib/node_modules/" | awk '{print $9}'| xargs rm #删除全局 node 模块注册的软链

```
#### nvm 与 n
同nvm一样，n也是npm的版本管理器，两者的比较如下： 

[creationix/nvm](https://github.com/creationix/nvm)
- nvm 不是一个 npm package，而是一个独立软件包
- 对全局模块的管理（这点很重要）
- 不支持windows （但可以通过nvm-windows 或者nodist 解决，官网上有说明）

[tj/n](https://github.com/tj/n)
- 由TJ大神开发的，它也是一个node的模块，所以支持各个node所支持的平台，包括windows
- 全局模块管理支持度很弱

综上所述：推荐用nvm。

#### 安装
1. 运行如下安装脚本：
`curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.0/install.sh | bash`
> 截止笔者写这篇文章为止， nvm最新版本为0.32，

2. 上述脚本还会在`~/.bash_profile, ~/.zshrc, ~/.profile 或~/.bashrc`下追加如下：

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
```

为了让它立即生效， 你可能需要运行如下命令：
```
source ~/.bashrc
```
> 这里.bashrc 也有可能是.bash_profile, .zshrc, .profile， 又你的运行平台（操作系统）决定

#### 使用
安装最新的node： 
```
nvm install node
```

安装特定版本的node：
```
nvm install 4.4.3
# or 

nvm install 4
```

切换版本
```
nvm use node  #最新的node
nvm use 4  #version 4
```

显示所有版本
```
nvm ls
```

显示各个版本的安装路径：

```
which node
```

更多内容，请直接参见[nvm官网](https://github.com/creationix/nvm#usage)





