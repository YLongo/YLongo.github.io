sbt仓库镜像设置: `${HOME}/.sbt/repositories`

```sbt
[repositories]
    local
    aliyun-repo: https://maven.aliyun.com/nexus/content/groups/public
    huaweicloud-maven: https://repo.huaweicloud.com/repository/maven/
    maven-central: https://repo1.maven.org/maven2/
    sbt-plugin-repo: https://repo.scala-sbt.org/scalasbt/sbt-plugin-releases, [organization]/[module]/(scala_[scalaVersion]/)(sbt_[sbtVersion]/)[revision]/[type]s/[artifact](-[classifier]).[ext]
```



sbt全局配置：

- 如果使用的`cmd`，则配置`${SBT_HOME}\conf\sbtconfig.txt`

  ```bash
  # sbt configuration file for Windows
  
  -Xms1024m
  -Xmx1024m
  -Xss4M
  -XX:ReservedCodeCacheSize=128m
  
  -Dsbt.override.build.repos=true
  -Dsbt.boot.directory=D:\sbt-1.4.5\boot
  -Dsbt.ivy.home=D:\sbt-1.4.5
  ```

- 如果使用的`gitbash`，则配置`${SBT_HOME}\conf\sbtopts`

  ```bash
  # ------------------------------------------------ #
  #  The SBT Configuration file.                     #
  # ------------------------------------------------ #
  -J-Xms1024m
  -J-Xmx1024m
  -J-Xss4M
  -J-XX:ReservedCodeCacheSize=128m
  
  -Dsbt.override.build.repos=true
  -Dsbt.boot.directory=$SBT_HOME/boot
  -Dsbt.ivy.home=$SBT_HOME
  ```

因为`sbtconfig.txt`是专门给`Windows`平台用的

