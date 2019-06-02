## Github Pages搭建步骤
1. 拥有github账户
2. 创建博客仓库
3. 进入仓库提交index.html
4. 开启仓库Github Pages(注意一个账户的Github Pages只能选择一个你喜欢的仓库进行发布，也就是一个账户只能有一个项目拥有Github Page.)
    - 进入仓库
    - 选择settings->下拉到Github Pages
    - 设置Source读取的分支
    - 默认域名为 `http(s)://< username>.github.io/< projectname>` ([配置专属域名](https://help.github.com/articles/using-a-custom-domain-with-github-pages/))  

>小技巧：请求如何不添加REPOSITORY_NAME？  
**将REPOSITORY_NAME(仓库名称)修改为上面的域名USERNAME.github.io即可 **

#### 注意事项
- Project pages site will be available at  `http(s)://< username>.github.io/< projectname>` . Github 使用免费仓库，那么 Github Pages 所在仓库必须为 Public。同理，若想在仓库私有的同时使用 Github Pages，则需要交相关的费用，使该仓库升级为Pro版本。
