# 智能辅助决策支持大模型平台 · 运行说明

这份文档是写给第一次接触这个项目的人看的。跟着下面的步骤走，你就能在本地把这个项目跑起来。

***

## 一、这个项目是什么？

这是一个用 **Vue 3 + Vite** 写的网页前端项目，页面名称是「智能辅助决策支持大模型」，主要还原了 Figma 设计稿里的 7 个页面：

- `4.1` 大模型训练微调

- `4.2` 大模型训练微调

- `5.1 ~ 5.5` 训练数据集生成系列页面（数据集管理 / 空状态 / 文件预览 / 配置 / 数据生成服务）

> 简单理解：它是一个只有前端、还没有接后端的「纯展示页面」。你看到的按钮、下拉、表格大多是为了还原设计稿，不一定会真正操作数据。

***

## 二、运行前需要准备什么？

这个项目是 JavaScript 项目，运行它只需要装两个东西：

| 需要的东西   | 说明                     | 版本要求           |
| ------- | ---------------------- | -------------- |
| Node.js | JavaScript 运行环境        | 18 或更高，推荐 20+  |
| npm     | 包管理工具，装 Node.js 时会一起装上 | 随 Node.js 自动安装 |

### 检查有没有装好

打开终端（Windows 按 `Win + R` 输入 `cmd` 回车；或按 `Win` 键搜索「PowerShell」打开），输入：

```bash
node -v
npm -v
```

- 如果能打印出版本号（例如 `v20.x.x`），说明装好了。

- 如果提示「不是内部或外部命令」，说明还没装，请先到官网下载安装：<https://nodejs.org/>（下载 LTS 长期支持版，一路下一步即可）。

***

## 三、获取代码

### 方式 1：已经有代码文件夹

如果代码已经在电脑上（例如 `E:\sl\intelligent`），直接进入该文件夹即可。

### 方式 2：从 GitHub 下载

在终端里输入：

```bash
git clone https://github.com/slzlk/intelligent.git
cd intelligent
```

> `cd intelligent` 表示「进入 intelligent 这个文件夹」。后面所有命令都要在这个文件夹里执行。

***

## 四、安装依赖（只需要做一次）

进入项目文件夹后，在终端输入：

```bash
npm install
```

这一步会根据 `package.json` 把项目需要的东西（Vue、Vite 等）下载到 `node_modules` 文件夹里。第一次运行需要联网，等它跑完即可（一般几分钟）。

> 出现 `node_modules` 文件夹说明安装成功。以后再次运行项目不需要重复这一步，除非删掉了 `node_modules`。

***

## 五、启动项目（日常开发用这个）

在项目文件夹里输入：

```bash
npm run dev
```

成功后会看到类似输出：

```text
  VITE v5.x.x  ready in xxx ms
  ➜  Local:   http://localhost:5173/
```

然后用浏览器打开终端里显示的地址（默认是 <http://localhost:5173/>），就能看到页面了。

> 提示：开发服务器是「热更新」的，你改了代码保存后，浏览器会自动刷新，不用手动重启。

***

## 六、如何切换查看不同页面？

项目一共 7 个页面，默认打开第 1 页（4.1）。想在浏览器里直接看某一页，可以在地址后面加 `?page=xxx`：

- `?page=fineTuneBase` —— 4.1 大模型训练微调

- `?page=fineTuneExpand` —— 4.2 大模型训练微调

- `?page=datasetManage` —— 5.1 数据集管理

- `?page=datasetEmpty` —— 5.2 预训练数据集生成（空状态）

- `?page=datasetPreview` —— 5.3 文件预览

- `?page=datasetConfig` —— 5.4 配置

- `?page=datasetTask` —— 5.5 预训练数据生成服务

例子：

```text
http://localhost:5173/?page=datasetManage
```

***

## 七、打包与预览（准备好发布时用）

### 打包

```bash
npm run build
```

打包成功后会生成一个 `dist` 文件夹，里面就是最终要给服务器用的静态文件。

### 本地预览打包结果

```bash
npm run preview
```

用来在本地预览 `dist` 里的成品，看看打包后的效果是否正常。

***

## 八、项目结构速览（了解即可）

```text
intelligent/
├── index.html           # 网页入口
├── package.json         # 项目信息和脚本命令
├── vite.config.js        # Vite 配置
├── public/              # 静态资源
├── src/
│   ├── main.js          # 程序启动入口
│   ├── App.vue          # 主框架（页面外壳 + 页面切换）
│   ├── style.css         # 全局样式
│   ├── components/      # 复用的组件
│   ├── pages/           # 7 个页面的组件
│   └── assets/image/    # 图片等资源
└── dist/                # 打包产物（运行 npm run build 后生成）
```

***

## 九、常用命令速查表

| 命令                | 作用        | 什么时候用                     |
| ----------------- | --------- | ------------------------- |
| `npm install`     | 安装依赖      | 第一次运行，或删掉 node\_modules 后 |
| `npm run dev`     | 启动开发服务器   | 日常开发调试                    |
| `npm run build`   | 打包生成 dist | 准备部署                      |
| `npm run preview` | 本地预览打包结果  | 检查打包后效果                   |

***

## 十、常见问题

**问：`node -v`** **提示找不到命令？**
答：还没装 Node.js，到 <https://nodejs.org/> 下载 LTS 版安装后再试。

**问：`npm install`** **很慢或报网络错误？**
答：可以换成国内镜像再装，例如：

```bash
npm install --registry=https://registry.npmmirror.com
```

**问：运行** **`npm run dev`** **报错？**
答：先确认当前目录是不是项目根目录（能看到 `package.json` 的地方），再确认 `npm install` 是否成功跑完。还不行就把报错信息发给懂技术的同事帮忙看。
