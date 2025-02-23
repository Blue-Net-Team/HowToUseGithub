# ssh的使用

在GitHub中，ssh一般用于连接远程仓库，以便于进行拉取和推送。ssh仓库的推送速度比https仓库的推送速度要快，所以一般我会使用ssh来连接远程仓库。但是ssh需要进行密钥对的配置

## 生成密钥对

在本地电脑的命令行(cmd)中输入以下命令

```shell
ssh-keygen -t rsa -b 4096 -C <email>
```

这个email是注册GitHub账号使用的邮箱，然后回车一次，要求输入保存密钥对的文路径，一般使用默认(直接回车就是默认)，然后再回车两次，就会生成密钥对。后面两次的回车就是不设置密码，如果设置密码，每次推送都需要输入密码，所以一般不设置密码。

在路径`C:\Users\<username>\.ssh`下会生成两个文件，一个是`id_rsa`，一个是`id_rsa.pub`，这两个文件就是密钥对，`id_rsa`是私钥，`id_rsa.pub`是公钥。

## 在GitHub设置ssh公钥

登录GitHub，点击右上角的头像，选择`Settings`，然后选择`SSH and GPG keys`，然后点击`New SSH key`，在`Title`中输入一个名字(为了便于管理公钥)，然后在`Key`中输入`id_rsa.pub`(公钥)的内容，然后点击`Add SSH key`，就可以添加ssh公钥了。这个过程可能需要用户配置2FA，这个在另一个文件[2FA的使用](./../2FA_about/README.md)中有介绍。

## 验证GitHub的ssh

在命令行中输入以下命令

```shell
ssh git@github.com -vT
```

期间会询问对端有指纹，是否继续，输入`yes`即可。

如果命令正常运行并且结束的时候结尾出现类似于

```shell
Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.
```

说明ssh验证成功，可以正常使用ssh连接GitHub仓库了。

### 可能遇到的问题

1. `Permission denied (publickey).`：这个问题一般是因为ssh公钥没有添加到GitHub中，或者添加的公钥有问题，需要重新添加公钥。

2. `Timeout, server <ip> not responding.`：这个问题一般是因为网络问题，需要检查网络是否正常，如果网络是正常的，在路径`C:\Users\<username>\.ssh`下新建`config`文件，在里面添加以下内容

    ```
    Host github.com
        HostName ssh.github.com
        User git
        Port 22
        IdentityFile ~/.ssh/id_rsa
    ```

    然后再进行尝试`ssh git@github.com -vT`。

    如果还是不行，把上面的端口改为443，再尝试一次。


完成了以上的步骤，就可以使用ssh连接GitHub仓库了。
