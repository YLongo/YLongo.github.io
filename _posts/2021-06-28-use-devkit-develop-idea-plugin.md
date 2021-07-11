> Mac下使用devKit开发IDEA插件（https://plugins.jetbrains.com/docs/intellij/using-dev-kit.html）

1. 下载IDEA社区版

2. 下载IDEA社区版源码

   > [IDEA版本与对应的源码分支](https://plugins.jetbrains.com/docs/intellij/build-number-ranges.html#intellij-platform-based-products-of-recent-ide-versions)

3. 构建IDEA源码

   > 官方详细指南（https://upsource.jetbrains.com/idea-ce/file/idea-ce-17812b1102973a61b8b73ee7fdbea12cf8036cd6/README.md）

   - 执行`getPlugins.sh`脚本，会生成一个`android`文件夹，会去`git clone` android相关的代码
   - 脚本执行完成之后，进入该文件，`git check` 与IDEA源码相同的分支
   - 使用IDEA社区版打开IDEA源码
   - 使用`gradle`进行构建

4. 根据官方图文配置[SDK](https://plugins.jetbrains.com/docs/intellij/setting-up-environment.html#configuring-intellij-platform-sdk)

5. 新建一个插件（https://plugins.jetbrains.com/docs/intellij/creating-plugin-project.html）

6. 测试时通过`add configuraion`新建一个`plugin`的配置，然后运行

