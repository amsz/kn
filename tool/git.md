# Git

## Gitlab

#### labels 分类

* 优先级 P1、P2、P3
* 类型 Feature、Develop、Improve、Test、Bug
* 系统模块

#### 彻底删除文件

通常是提交了密码之类的敏感数据到 git 库中

```shell
git filter-branch --force --index-filter \
  'git rm -rf -r --cached --ignore-unmatch PATH-TO-YOUR-FILE-WITH-SENSITIVE-DATA' \
  --prune-empty --tag-name-filter cat -- --all

git push origin --force --all
git push origin --force --tags
```

防止敏感数据再次被提交

```shell
echo "YOUR-FILE-WITH-SENSITIVE-DATA" >> .gitignore
git add .gitignore
git commit -m "Add YOUR-FILE-WITH-SENSITIVE-DATA to .gitignore"
```

清理本地缓存

```shell
git for-each-ref --format='delete %(refname)' refs/original | git update-ref --stdin
git reflog expire --expire=now --all
git gc --prune=now
```

参考 https://help.github.com/articles/removing-sensitive-data-from-a-repository

💊 当前 [gitlab 还不支持](https://gitlab.com/gitlab-org/gitlab-ce/issues/30093)