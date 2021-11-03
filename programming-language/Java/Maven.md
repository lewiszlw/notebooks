# 解决Maven依赖冲突
查看该包相关的依赖树
```shell
mvn dependency:tree -Dverbose -Dincludes=org.glassfish:javax.el
```

# Maven发布项目
1.修改项目版本号
```shell
mvn versions:set -DnewVersion=1.0.6
```
2.发布到 maven 远程仓库
```shell
mvn clean -U deploy -Dmaven.test.skip=true
```