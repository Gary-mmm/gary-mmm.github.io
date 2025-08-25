---
title: Hexo的多设备部署
tags:
  - tech
date: 2025-07-27 10:13:38
---


又是麻烦的环境配置过程，记录一下以供参考。因为是配置完环境后写的而不是一步一步写的，所以细节上可能会出现部分问题。

## 在老电脑A上

多设备同步本质上还是使用了git来管理。在老电脑A上安装完环境之后，`hexo d`把静态文件推送到`master`分支。这里的文件包括：
- source（md文件等）
- archieves
- css
- fancybox
- js
- index.html

然后在`github`上另外设置一个`hexo`分支并设为主分支。并且使用`git push`将源文件以及配置文件一并上传至该分支。
```
git checkout -b hexo origin/hexo     # 如果还没本地分支
# 或者
git checkout hexo                    # 如果已经有了
```

```
git add .
git commit -m "更新配置文件"
```

```
git push origin hexo
```
这时候可以上github确认一下是否成功。

## 在新电脑B上

我希望把A的配置文件移到B上，只要使用git就可以完成，复制粘贴费力而且容易遗漏。在git pull完之后使用`npm install hexo`,`npm install`,`npm install hexo-deployer-git`命令代替`hexo init`.

上面步骤中，npm install其实就是读取了packages.json里面的信息，自动安装依赖，有的小伙伴可能只执行npm install就行了，不过按照上面的三步是最稳妥的。如果安装完之后可以正常使用hexo命令就成功了。

下面给一些命令参考
```
git stash #可以保存未提交的修改（如果有）
git stash pop #取出暂存的修改,可能需要解决冲突
git checkout -b hexo #新建分支hexo

```
其中遇到换行符转换的问题，根据gpt的建议使用
`git config --global core.autocrlf input`

> 你的 Git 设置为在 Windows 上将换行符自动转换为 CRLF（\r\n），> 而这些文件当前使用的是 Unix 风格的 LF（\n）换行。

> Git 提示你: “等你下次修改或 checkout 的时候，Git 会自动把这些文件的换行从 LF 转成 CRLF。”

> 这是因为 Git 的 core.autocrlf 选项通常在 Windows 默认是 true，所以 Git 会：提交时转成 LF（Unix 标准）,检出到工作目录时转回 CRLF.

## 最后的结果

hexo分支上储存的文件：
- .github
- scaffolds
- source
- themes
- .gitignore
- _config.landscape.yml
- _config.yml
- package-lock.json
- package.json

每次更新完源文件后`push`一下，保持hexo同步，不影响master分支。

最后`hexo d`保持master分支上的同步即可。

参考文献：
[Hexo在多台电脑上提交和更新](https://blog.csdn.net/K1052176873/article/details/122879462)