# 常用命令
### git cherry-pick 将指定的 commit 应用于其他分支
```
git checkout master
git cherry-pick 1873e52
```
将其他分支的 commit 1873e52 应用到当前分支 master。

### git show 查看各种类型对象，如 commit、tag等
```
git log
git show 0608623
```
查看 commit 0608623 改动。

# xxx