# Adachi-BOT

## 说明

[原项目](https://github.com/SilveryStar/Adachi-BOT)的[该版本](https://github.com/SilveryStar/Adachi-BOT/tree/ver1.4.6)已经不再维护，此项目当前会持续更新。

插件开发和资源文件提交请查阅[手册](docs/README.md)。常见问题请参阅 [FAQ](https://github.com/Arondight/Adachi-BOT/issues?q=label%3Adocumentation)。

> 1. 资源请提交到[resources\_custom](resources\_custom/)目录，这些资源文件会覆盖到[resources](resources)目录。
> 2. 资源和代码请先提交`dev`分支。

## 使用

### 部署

#### 准备环境

> 机器人至少需要`2GiB`内存或交换空间以运行无头浏览器。

首先你需要有一份较新的 [Node.js](https://nodejs.org/en/download/)，机器人无法在较低版本的 Node.js 上运行。

<details>

##### CentOS、RHEL

```
sudo yum -y remove nodejs
curl -fsSL https://rpm.nodesource.com/setup_16.x | sudo -E bash -
sudo yum -y install nodejs
```

##### Ubuntu、Debian

```
sudo apt -y remove nodejs
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt -y install nodejs
```

</details>

#### 部署项目

```
git clone https://github.com/Arondight/Adachi-BOT.git
cd ./Adachi-BOT/
npm install
```

如果`puppeteer`模块下载`Chromium`失败，那么`Adachi-BOT`将无法正常运行……

<details>

此时你有三种选择。首先删除`./node_modules/`目录。

其一，使用系统自带的`Chromium`，这里以`CentOS`为例，执行以下命令。

> 这里需要找到`Chromium`的二进制可执行文件路径，而非启动脚本或其链接的路径。

```
sudo yum -y install epel-release
sudo yum -y install chromium
grep PUPPETEER_EXECUTABLE_PATH ~/.bashrc || ( echo 'export PUPPETEER_EXECUTABLE_PATH=/usr/lib64/chromium-browser/chromium-browser' | tee -a ~/.bashrc )
source ~/.bashrc
PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true npm install
```

其二，通过任意合法途径获得一个可以访问国际互联网的`http`代理，然后执行以下命令。

```
npm_config_proxy=http://<ip>:<port> npm install
```

其三，尝试改用`Firefox`，执行以下命令。

```
PUPPETEER_PRODUCT=firefox npm install
```

</details>

### 配置

需要编辑以下文件，根据注释填入合适的配置。

| 文件 | 作用 |
| --- | --- |
| [config/setting.yml](config/setting.yml) | 基本配置选项 |
| [config/cookies.yml](config/cookies.yml) | 米游社Cookie |

### 运行

#### 手动运行

进入`Adachi-BOT`项目所在目录。

> 首次运行需要先执行`npm start`，完成QQ的新设备认证，随后`Ctrl+C`停止机器人，然后再执行以下命令。

```
PM2_HOME=$(pwd)/bot.pm2 ./node_modules/.bin/pm2 start ./app.js --name bot
PM2_HOME=$(pwd)/bot.pm2 ./node_modules/.bin/pm2 save
```

#### 开机启动

进入`Adachi-BOT`项目所在目录。手动运行后执行。

```
PM2_HOME=$(pwd)/bot.pm2 ./node_modules/.bin/pm2 startup
```

### 更新

进入`Adachi-BOT`项目所在目录。

```
git stash push .
git pull -p
git stash pop
npm install
PM2_HOME=$(pwd)/bot.pm2 ./node_modules/.bin/pm2 stop bot
PM2_HOME=$(pwd)/bot.pm2 ./node_modules/.bin/pm2 update
PM2_HOME=$(pwd)/bot.pm2 ./node_modules/.bin/pm2 start bot
```

### 其他操作

<details>

#### 查看状态

进入`Adachi-BOT`项目所在目录。

```
PM2_HOME=$(pwd)/bot.pm2 ./node_modules/.bin/pm2 list bot
```

#### 查看日志

进入`Adachi-BOT`项目所在目录。

```
PM2_HOME=$(pwd)/bot.pm2 ./node_modules/.bin/pm2 log bot
```

#### 手动停止

进入`Adachi-BOT`项目所在目录。

```
PM2_HOME=$(pwd)/bot.pm2 ./node_modules/.bin/pm2 stop bot
```

</details>

## 功能

### 所有功能

> 具体命令请查看[这里](src/plugins/tools/help.js)。

| 功能 | 形式 | 权限管理 |
| --- | --- | --- |
| 展示米游社ID下的游戏账号 | 插件 | ✔️ |
| 展示UID对应的游戏账号 | 插件 | ✔️ |
| 展示UID对应的深渊战绩 | 插件 | ✔️ |
| 米游社ID绑定和改绑 | 插件 | ❌ |
| 圣遗物掉落和强化 | 插件 | ✔️ |
| 圣遗物截图评分 | 插件 | ✔️ |
| 展示角色官方数据 | 插件 | ✔️ |
| 祈愿十连（支持定轨） | 插件 | ✔️ |
| 今天该刷什么 | 插件 | ❌ |
| 今天吃什么 | 插件 | ❌ |
| 掷骰子 | 插件 | ❌ |
| 点歌 | 插件 | ✔️ |
| 伟大的升华 | 插件 | ❌ |
| 主人通过机器人与机器人的好友或群聊天 | 插件 | ✔️ |
| 群广播和好友广播 | 插件 | ❌ |
| 管理功能 | 插件 | ❌ |
| 随机复读群信息 | 自有功能 | ❌ |
| 停止响应指定群 | 自有功能 | ❌ |
| 自我介绍 | 自有功能 | ❌ |
| 上线通知 | 自有功能 | ❌ |

### 图片示例

> 1. 有些样式可能已经变更。
> 2. 仅展示了部分功能。

<details>
  <summary>展示玩家信息</summary>
  <div align="center">

![米游社](images/米游社.png)

  </div>
</details>

<details>
  <summary>查询我的角色</summary>
  <div align="center">

![我的](images/我的.png)

  </div>
</details>

<details>
  <summary>查询深渊战绩</summary>
  <div align="center">

![深渊](images/深渊.png)

  </div>
</details>


<details>
  <summary>圣遗物掉落和强化</summary>
  <div align="center">

![圣遗物](images/圣遗物.png)
![强化](images/强化.png)

  </div>
</details>

<details>
  <summary>祈愿十连</summary>
  <div align="center">

![十连](images/十连.png)

  </div>
</details>

<details>
  <summary>游戏数据查询</summary>
  <div align="center">

![角色](images/角色.png)
![武器](images/武器.png)

  </div>
</details>

## Licenses

[MIT](LICENSE)

