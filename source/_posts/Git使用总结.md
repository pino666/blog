---
title: Git使用笔记
tags: 
    - Javascript
    - Git
# cover: /assets/two.jpg
---
{% blockquote %}
本文参考于Git官方文档 https://git-scm.com/book/zh/v1/Git-%E5%9F%BA%E7%A1%80-%E5%8F%96%E5%BE%97%E9%A1%B9%E7%9B%AE%E7%9A%84-Git-%E4%BB%93%E5%BA%93
{% endblockquote %}

## 初始化新仓库
### 对现有项目进行初始化
若想对现有项目使用Git进行管理，则需在项目所在目录执行 git init 即可～
执行此过程后，当前目录会出现一个名为.git的目录，所有Git需要的数据和资源都放在这个目录中。但是现在并没有开始追踪管理项目中的任何一个文件，还需执行<font color="#dd0000"> git add . </font>命令去追踪所有文件或某些特定文件。
<table><tr><td bgcolor=#D1EEEE>$ git init </td></tr></table>

### 从现有仓库进行克隆
若远程仓库已有创建好的项目，你想对此项目进行一些修改，那么需使用<font color="#dd0000"> git clone </font>复制一份文件到你本地～
<table><tr><td bgcolor=#D1EEEE>$ git clone git远程仓库地址 </td></tr></table>

## 记录每次更新
<table><tr><td bgcolor=#D1EEEE>注： 使用git仓库的项目中的所有文件无外乎两种状态：已跟踪或未跟踪～
已跟踪的文件指已经被纳入版本控制的文件，在上次快照中有记录，他们的状态也许是已修改或在暂存区或未更新等等；除了已跟踪文件，剩下的即未跟踪文件～ </td></tr></table>
<font color="#dd0000" size="4">下面是项目中文件状态的生命周期图:</font>
![文件的生命周期](/assets/git.jpg)
<font color="#dd0000" size="4">举例解释：</font>在一项目中，需添加一新增文件README.md，那么编写完毕后，README.md文件即为未跟踪文件，此时可用<font color="#dd0000">git status(简写：gst)</font>查看当前文件的状态，如图所示：<font color="#dd0000">README.md文件出现在“Untracked files”的文件列表中～</font>
![文件的状态](/assets/untrack.jpg)
此时，需将git开始跟踪此新文件，使用命令<font color="#dd0000">git add README.md(git add .：添加所有文件)</font>此时再使用git status命令，则README文件已被跟踪,并出现在 <font color="#dd0000">“Changes to be committed”<font>下面，说明现在为已暂存状态(已修改)，如图所示：
![文件的状态](/assets/yizancun.jpg)
