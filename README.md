### pino 的 blog

### 本地启动
hexo serve

###新建文章
hexo new [layout] "pageTitle"

###新建页面
hexo new page "pageName"

###部署
hexo deploy

###生成静态页面至public目录
hexo generate

###清除缓存
hexo clean

###常用复合命令（部署）
hexo clean && hexo g -d
（清除缓存，并自动的将public文件夹里的内容提交到配置好的git仓库）
