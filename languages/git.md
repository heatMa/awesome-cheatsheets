# git命令

```
# 查看git配置 git config --list
```

## git key

```shell
ssh-keygen -t rsa -b 4096 -C "rongbo.ma@plus.ai"
eval `ssh-agent -s` # 如果没有这条命令，ssh-add .ssh/id_rsa下面指令可能会失败
ssh-add .ssh/id_rsa
cat .ssh/id_rsa.pub   # 登录https://github.com/settings/keys, add ssh key，且设置authorize

```



# git添加多个账户

[参考链接](https://blog.csdn.net/sihai12345/article/details/118858582)中有两个问题：

1. 没有`ssh-add .ssh/id_rsa`这一步，需要有的，要把新生成的密匙添加进去。

2. ~/.ssh/config格式不对，需要缩进

   ```
   # one (first account)
   Host one.github.com
       HostName github.com
       PreferredAuthentications publickey
       User one
       IdentityFile ~/.ssh/id_rsa_one
   
   # two(second account) 
   Host two.github.com
       HostName github.com
       PreferredAuthentications publickey
       User two
       IdentityFile ~/.ssh/id_rsa_two
   ```

## 按上述博客我的配置流程

1. ssh-keygen -t rsa -f ~/.ssh/id_rsa_two -C 986944809@qq.com

​	因为我原来已经有生成过一次了，这次会生成id_rsa_two  id_rsa_two.pub

2. ssh-add id_rsa_two 
3. cat .ssh/id_rsa.pub   # 登录https://github.com/settings/keys, add ssh key
4. 在 ~/.ssh/config添加如下，id_ras_two是新生成的

```
Host heatMa
  HostName github.com
  User heatMa
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa_two
```

5. 测试是否连接成功
   ssh -T git@heatMa  #headMa是上面配置中的Host。

   ```
   Hi heat! You've successfully authenticated, but GitHub does not provide shell access.
   ```

6. 取消git全局配置

   ```
   # 取消全局 用户名/邮箱 配置
   git config --global --unset user.name
   git config --global --unset user.email
   # 设置局部 用户名/邮箱 配置，要加个--local?
   git config --local user.name heatMa
   git config --local user.email 986944809@qq.com
   ```

7. git clone写法更改

   原来是`git clone git@github.com:heatMa/awesome-cheatsheets.git`，现在改成

   `git clone git@heatMa:heatMa/awesome-cheatsheets.git` # 这里的heatMa是第4步中的Host名字