# My Blog

基于 Hugo + PaperMod 的个人技术博客。

## 在线地址

https://coyilla.github.io/

## 技术栈

- Hugo（extended）
- PaperMod 主题
- GitHub Pages + GitHub Actions 自动部署  

## 相关文档
[分类规范](docs/categories-guide.md)
[如何发帖·省流版](docs/how_to_post.md)

## 如何部署

### 克隆仓库
```  
git clone https://github.com/coyilla/coyilla.github.io.git
cd coyilla.github.io
```
### 拉取主题子模块
```
git submodule update --init --recursive
```
### 启动本地预览（含草稿）
```
hugo server -D
```
### 创建文章
```
hugo new content posts/{{category}}/文章名.md
```
编辑文章，写完把 draft 改为 false
### 图片附件存放

图片统一放在 `static/assets/` 目录中，例如：

```text
static/
└── assets/
    └── file.png
```

在 Markdown 中使用网站根目录对应的访问路径引用：

```markdown
![图片说明](/assets/file.png)
```

#### 为什么不能写成 `static/...`

`static/` 是 Hugo 的源文件目录，不是浏览器可访问的公开路径。

构建时：

```text
static/assets/file.png
```

会原样发布为：

```text
public/assets/file.png
```

因此浏览器使用的是：

```text
/assets/file.png
```

其中：

- `static` 不出现在最终 URL 中
- `/` 表示网站根目录
- `assets` 对应 `static/assets`
- 文件名对应实际存放在 `static/assets/` 中的文件

本博客上线后，图片最终地址为：

```text
https://coyilla.github.io/assets/file.png
```

#### 上传图片的规则

以后新增图片时：

1. 将图片复制到 `static/assets/`
2. 建议使用英文、数字、短横线命名，避免空格和中文
3. 在文章 Markdown 中使用 `/assets/文件名.png` 引用
4. 修改后运行 `hugo server -D` 本地确认图片能够显示
5. 确认无误后提交并推送到 GitHub

临时上传方式：

```powershell
Copy-Item "图片路径" -Destination "static/assets/"
```

示例：

```powershell
Copy-Item "C:\Users\cnjy\Pictures\截图.png" -Destination "static/assets/截图.png"
```

不建议继续使用 `/content/assets/...` 或 `../../assets/...`：

- `content/assets/` 默认不会作为静态公开资源发布
- `../../assets/...` 是相对路径，文章 URL 变化后可能指向错误位置
- 根目录绝对路径 `/assets/...` 更稳定，不依赖文章所在的目录层级

`static/assets/` 中的图片会随 `hugo` 构建一起复制到 `public/assets/`，不需要手动修改或提交 `public/`目录。

### 本地预览确认
```
hugo server -D
```
### 提交并推送
```
git add .
git commit -m "add: 文章标题"
git push
```

## 碎碎念

刚刚，我问AI，为什么个人博客的README需要教会其他人如何部署，它的回答是：  

**写给“未来的你”看**  

未来的我，你好

hello, Coyilla from the future.