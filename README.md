# caolinhao

曹琳浩的个人主页 —— 单文件静态网站，纯手写 HTML / CSS / JavaScript，零依赖、零构建。

**在线地址**：https://hulue-lin.github.io/caolinhao/

## 这是什么

一个用来自我介绍的单页网站。整个站点只有一个 `index.html`，所有样式和脚本都内嵌在里面，双击就能在浏览器打开，不需要安装任何东西。

包含这些栏目：

| 栏目 | 内容 |
|---|---|
| 首屏 | 姓名、学院专业、可点击切换的关键词轮播、数据条 |
| 关于我 | 自我介绍、学校 / 家乡 / 专业 / 联系方式等信息卡 |
| 平时喜欢 | 在玩的游戏和在看的书，含 UID / ID |
| 玩个小游戏 | 跳转到一个在线小游戏 |
| 正在学习 | 目前主攻的课程 |
| 经历 | 时间线（大学 / 高中 / 接下来的计划） |
| 留言板 | 基于 GitHub Discussions 的留言区 |
| 联系 | 电话 / 微信、邮箱、QQ |

## 技术说明

- **无依赖**：没有框架、没有 CDN、没有构建步骤、没有 node_modules
- **自适应**：手机 / 平板 / 桌面都做了适配，窄屏下有专门的性能优化
- **深色 / 浅色双主题**：右上角按钮切换，选择会记在浏览器里，首次访问跟随系统偏好
- **交互特效**：鼠标轨迹（线性拖尾）和点击动效（三层圆环扩散），用 canvas 手写
- **无障碍**：支持键盘 Tab 导航、可见焦点框、`prefers-reduced-motion` 下自动关闭动画
- **不空转**：canvas 特效没有内容可绘制时自动停止；页面切到后台时打字机停下

## 本地查看

直接双击 `index.html` 即可。

如果想用本地服务器预览（行为更接近线上）：

```powershell
cd D:\dsh\my-profile
python -m http.server 8000
# 然后访问 http://localhost:8000
```

## 怎么修改

所有内容都在 `index.html` 一个文件里，用编辑器打开搜索关键词就能定位。

**改主题配色** —— 文件开头的 `<style>` 里有两组 CSS 变量：

```css
:root{ --accent:#38bdf8; ... }                    /* 深色主题 */
html[data-theme="light"]{ --accent:#0f766e; ... } /* 浅色主题 */
```

改这两组里的颜色值，整站配色会跟着变。

**改关键词轮播** —— 搜索 `var words`：

```js
var words = ['正在学 C 语言', '正在啃高等数学', '信息对抗技术专业'];
```

**改基本信息** —— 搜索 `所在学校`、`家乡`、`电话` 等，直接改对应的文字。

**改头像** —— 目前是圆里的一个「琳」字。想换成图片：把图片命名为 `avatar.jpg` 放进本目录，然后把

```html
<span>琳</span>
```

换成

```html
<img src="avatar.jpg" alt="头像">
```

同时把上面几行的 `--chars:1;` 注释掉（那是给文字模式算字号用的）。

## 留言板说明（已配置完成）

留言板用的是 [Giscus](https://giscus.app)，把 GitHub Discussions 当留言后端，不需要自己写服务器，也没有额外费用。

**当前状态：已配置完成，无需再操作。** 配置值保存在 `index.html` 里的 `GISCUS` 对象：

```js
var GISCUS = {
  repo: 'hulue-lin/caolinhao',
  repoId: 'R_kgDOVHnlAA',
  category: 'Announcements',
  categoryId: 'DIC_kwDOVHnlAM4DIae7'
};
```

前置条件也都已完成：仓库是公开的、giscus App 已安装、Discussions 已启用。

### 如果以后想换分类

1. 打开 https://giscus.app ，填入仓库名
2. 页面 ↔ discussion 映射选 `pathname`，分类按需选择
3. 往下滚到「启用 giscus」，从生成的代码里复制 `data-repo-id` 和 `data-category-id`
4. 替换上面 `GISCUS` 里的对应值

> giscus 只会列出 **Announcements 类型**的分类。本站用的是 GitHub 默认的 `Announcements`，这对访客完全无感 —— 只有仓库维护者在 GitHub 后台才看得到分类名。

### 主题联动

giscus 自带主题，本站的深色 / 浅色切换会通过 `postMessage` 通知它一起换，不需要刷新页面。所以代码里没用官网默认的 `preferred_color_scheme`，而是按当前主题显式传 `dark` / `light`。

### 如果留言板一直不显示

按顺序检查：

- 浏览器控制台（F12）有没有报错
- 仓库 Settings → Features 里 Discussions 是否还勾着
- https://github.com/apps/giscus 的安装是否还在（有没有被撤销授权）
- 有没有装广告拦截插件把 `giscus.app` 拦了

## 部署

站点托管在 **GitHub Pages**，从 `main` 分支的根目录直接发布。

改完内容后推送即可，Pages 会自动重新构建（约 1–2 分钟）：

```powershell
git add -A
git commit -m "更新内容"
git push
```

线上看不到变化时按 `Ctrl+F5` 强制刷新，避免浏览器缓存。

仓库根目录的 `.nojekyll` 是给 Pages 用的空文件，作用是不让 Pages 用 Jekyll 处理这个静态站点，**不要删**。

## 目录结构

```
caolinhao/
├── index.html    站点全部内容（HTML + CSS + JS）
├── .nojekyll     告诉 GitHub Pages 跳过 Jekyll
└── README.md     本文件
```
