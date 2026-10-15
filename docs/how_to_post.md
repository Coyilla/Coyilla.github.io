# 写文章流程
1. 新建文章  
自动生成 drafts: true  
`hugo new content posts/new_post_name/index.md ` 
2. 用 VSCode 或记事本打开，写 Markdown  
重点：写完务必把 `draft: true` 改为 `draft: false`  
3. 本地预览（确认排版、代码高亮没问题）  
`hugo server -D`  
4. 提交代码   
`git add .`  
`git commit -m "发第二篇"`  
5. 推送，GitHub Actions 自动上线（等1分钟看 Actions 绿勾）  
`git push`  