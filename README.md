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

## 留言板配置（需手动做三步）

留言板用的是 [Giscus](https://giscus.app)，把 GitHub Discussions 当留言后端，不需要自己写服务器。**没配置之前页面上只会显示一段提示文字，不会报错。**

1. 打开仓库 **Settings → General → Features**，勾上 **Discussions**
2. 进入仓库的 **Discussions** 标签，点 **New category** 新建一个分类，名字填 `留言板`，类型选 **Announcements**
3. 打开 https://giscus.app ，在 repository 里填 `hulue-lin/caolinhao`，分类选刚建的 `留言板`，然后从它给出的代码里复制两个值：
   - `data-repo-id` → 形如 `R_kgDOxxxxxxx`
   - `data-category-id` → 形如 `DIC_kwDOxxxxxxx`

4. 把这两个值填进 `index.html` 里的 `GISCUS` 配置：

```js
var GISCUS = {
  repo: 'hulue-lin/caolinhao',
  repoId: 'R_kgDO...',        // 填这里
  category: '留言板',
  categoryId: 'DIC_kwDO...'   // 填这里
};
```

保存并推送后，留言板就会自动出现。只有两步都填了才会去加载外部脚本，否则保持提示状态。

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
