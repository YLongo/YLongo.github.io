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

