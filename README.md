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

图片统一放在 `assets/` 目录中，例如：

```text
assets/
└── file.png
```

在 Markdown 中使用网站根目录对应的访问路径引用：

```markdown
![图片说明](../../../assets/file.png)
```

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