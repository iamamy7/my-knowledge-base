# mdBook 使用与 GitHub 自动发布指南

这篇指南记录本知识库的固定使用方法。以后忘记怎么操作时，直接打开这一页照着做，不需要重新学习整套流程。

## 一句话理解 mdBook

mdBook 会把 `src` 文件夹中的 Markdown 笔记转换成一个带左侧目录、搜索和主题切换功能的网站。

日常只需要记住这条路线：

```text
写 Markdown → 本地预览 → Commit → Push → GitHub 自动发布
```

不需要每次手动生成网站，也不需要每次创建 Release。

## 本知识库的位置

本机文件夹：

```text
/Users/wangjing/Desktop/my-knowledge-base
```

当前使用的 mdBook 版本：

```text
0.5.4
```

## 认识最重要的文件

```text
my-knowledge-base/
├── book.toml                 知识库名称和主题设置
├── src/                      所有需要长期保存的知识内容
│   ├── README.md             知识库首页
│   ├── SUMMARY.md            左侧目录和文章顺序
│   └── engineering/          工程学习内容
├── book/                     自动生成的网站，不要手动修改
└── .github/workflows/        GitHub 自动发布配置
```

最重要的规则：

- 正文全部写在 `src` 中。
- 新文章必须添加到 `src/SUMMARY.md`，才会显示在左侧目录。
- 不要修改 `book` 文件夹，因为它会在下次构建时被覆盖。

## 每次开始写作

### 第一步：打开本地预览

打开 Mac 的“终端”，输入：

```bash
cd /Users/wangjing/Desktop/my-knowledge-base
mdbook serve --open
```

浏览器会打开本地知识库，通常是：

```text
http://localhost:3000
```

保持终端窗口运行。修改并保存文章后，网页会自动刷新。

结束预览时，在终端按：

```text
Control + C
```

### 第二步：写文章

在 `src` 对应分类文件夹中创建 Markdown 文件。例如：

```text
src/engineering/github-actions.md
```

文件名建议使用小写英文，多个单词之间使用 `-`。文章内容和标题可以全部使用中文。

文章示例：

```markdown
# GitHub Actions 学习笔记

## 它是什么

GitHub Actions 可以在代码上传后自动测试、构建和发布项目。

## 今天学到的内容

- Workflow 是自动化流程。
- Push 可以触发自动发布。
- 运行结果可以在 Actions 页面查看。
```

### 第三步：把新文章加入目录

打开 `src/SUMMARY.md`，在合适位置加入链接：

```markdown
- [GitHub Actions](engineering/github-actions.md)
```

缩进代表上下级关系：

```markdown
- [GitHub 学习](engineering/git-and-github.md)
  - [GitHub Actions](engineering/github-actions.md)
```

如果文章已经创建但左侧没有出现，首先检查 `SUMMARY.md`。

### 第四步：检查网站能否生成

通常只需观察本地预览是否正常。需要单独检查时，在知识库文件夹运行：

```bash
mdbook build
```

看到 `Finished` 代表生成成功。

## 添加图片

建议把图片统一放在：

```text
src/images/
```

如果文章位于 `src/engineering/`，可以这样引用：

```markdown
![图片说明](../images/example.png)
```

图片说明不要留空，这对以后搜索和无障碍阅读都有帮助。

## 与 GitHub 的关系

GitHub 在这里承担三项工作：

1. 保存知识库的云端副本。
2. 记录每一次修改历史。
3. 通过 GitHub Actions 自动生成并发布网站。

对应关系是：

```text
mdBook：把 Markdown 生成网站
Git：记录每次修改
GitHub：保存云端版本
GitHub Actions：自动运行 mdbook build
GitHub Pages：提供可以访问的网站地址
```

## 第一次连接 GitHub

这部分只需要操作一次。

### 1. 使用 GitHub Desktop 添加知识库

打开 GitHub Desktop，选择：

```text
File → Add Local Repository
```

选择：

```text
/Users/wangjing/Desktop/my-knowledge-base
```

如果提示它还不是 Git 仓库，点击创建仓库的入口。

创建时建议：

- Name：`my-knowledge-base`
- Local Path：`/Users/wangjing/Desktop`
- Git Ignore：`None`，因为项目已经有 `.gitignore`
- License：`None`

确认最终文件夹仍然是 `/Users/wangjing/Desktop/my-knowledge-base`，避免意外创建重复文件夹。

### 2. 创建第一个 Commit

在 GitHub Desktop 中检查 Changes，填写：

```text
初始化个人知识库
```

点击 `Commit to main`。

Commit 的意思是保存一个本地版本，还没有上传到 GitHub。

### 3. 发布仓库

点击 `Publish repository`。

发布前确认：

- 仓库名为 `my-knowledge-base`。
- 所属账号正确。
- 知识库包含私人内容时，保持 Private。
- 不要上传密码、API Key、客户数据和私人证件。

如果希望任何人都能通过 GitHub Pages 阅读知识库，需要根据仓库可见性和 GitHub 套餐决定是否公开。不要为了发布网站而误把私人笔记公开。

### 4. 开启 GitHub Pages 自动发布

在 GitHub 仓库网页进入：

```text
Settings → Pages → Build and deployment
```

把 Source 设为：

```text
GitHub Actions
```

项目已经包含：

```text
.github/workflows/mdbook.yml
```

它会在内容 Push 到 `main` 后自动构建和发布网站。

## 以后每次更新的固定流程

第一次连接完成后，日常只需要：

```text
1. 打开 GitHub Desktop，选择 my-knowledge-base
2. 点击 Fetch origin，先检查云端是否有更新
3. 打开 mdBook 本地预览
4. 修改或新增 Markdown
5. 检查 SUMMARY.md 和网页效果
6. 回到 GitHub Desktop 查看 Changes
7. 填写本次修改说明
8. 点击 Commit to main
9. 点击 Push origin
10. 等待 GitHub Actions 自动发布
```

不需要手动运行部署命令。`Push origin` 后，GitHub 会自动完成后面的工作。

## 在哪里查看自动发布结果

打开 GitHub 仓库的 `Actions` 页面：

- 黄色圆点：正在构建。
- 绿色对勾：构建和发布成功。
- 红色叉号：运行失败，需要点进去看具体步骤。

工作流也支持手动运行：

```text
Actions → 自动发布 mdBook 知识库 → Run workflow
```

手动运行只用于重新发布或排查问题，平时不需要点击。

## Commit 怎么写

Commit 信息只需简短说明“这次改了什么”。例如：

```text
新增 GitHub Actions 学习笔记
补充数据库基础知识
调整产品设计目录
修正文档中的错误链接
```

不需要每改一个字就 Commit。完成一个小主题后保存一次即可。

## 是否需要每次创建 Release

不需要。

- Commit：日常保存，经常使用。
- Push：把本地 Commit 上传到 GitHub，经常使用。
- Release：重要里程碑才使用，例如知识库公开发布第一版。

## 常见问题

### 新文章没有出现在目录里

检查是否已经添加到 `src/SUMMARY.md`，以及文件路径是否正确。

### 浏览器没有自动更新

确认终端里的 `mdbook serve --open` 仍在运行；必要时停止后重新运行。

### GitHub Actions 显示红色失败

先点击失败记录，查看是“生成知识库”还是“发布到 GitHub Pages”失败。

- 生成失败：通常是文件、配置或链接问题，先在本地运行 `mdbook build`。
- 发布失败：检查 `Settings → Pages` 的 Source 是否为 `GitHub Actions`。

### 本地出现很多 `book` 文件变化

`book` 是自动生成内容，不应上传。确认项目根目录的 `.gitignore` 包含：

```text
book/
```

### 换电脑后怎么办

在新电脑安装 GitHub Desktop 和 mdBook，然后从 GitHub Clone 仓库。知识内容和自动发布配置都会一起下载。

## 最短备忘录

只记住以下内容也可以：

```text
开始预览：mdbook serve --open
内容位置：src/
目录文件：src/SUMMARY.md
本地保存：Commit
上传云端：Push
网站发布：GitHub Actions 自动完成
```

