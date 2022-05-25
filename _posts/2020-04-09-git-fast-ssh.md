在有代理的情况下，加速`git clone`的速度。

- `ssh`

  在用户目录下的`.ssh`文件夹下新建`config`：

  - Windows添加如下命令：

    ```ssh
    Host github
        HostName ssh.github.com
        ProxyCommand connect -S 127.0.0.1:1080 -a none %h %p
    ```
  - Mac添加如下命令

    ```config
    Host github
        HostName ssh.github.com
        ProxyCommand nc -x 127.0.0.1:1086 %h %p
    ```
- `https`

  在用户目录下的`.gitconfig`文件中添加如下语句：

  ```properties
  [http "https://github.com/"]
      proxy = socks5://127.0.0.1:1086
  [https "https://github.com/"]
      proxy = socks5://127.0.0.1:1086
  ```

> `1080`、`1086`为代理本地监听的端口号。不同的平台可能端口号不同
>
> 加`HostName`前缀是为了保证，只在`github`下才走代理，不然是全局都这样



---



如果提交代码时报如下错误或者其它稀奇古怪的问题：

```shell
kex_exchange_identification: Connection closed by remote host
Connection closed by UNKNOWN port 65535
fatal: 无法读取远程仓库。

请确认您有正确的访问权限并且仓库存在
```

如果使用了代理：

- 查看`~/.ssh/config`中配置的代理socks端口是否正确
- 查看git全局配置路径`$HOME/.gitconfig`中配置的代理是否正确
- 如果`ping github.com`显示的IP地址是`127.0.0.1`，可以尝试更改DNS地址为`114.114.114.114`
