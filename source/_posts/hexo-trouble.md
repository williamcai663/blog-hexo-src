---
title: hexo deploy 提示 permission denied的解决方法
date: 2018-04-22 22:17:50
tags: 随笔
---


- 出现的问题

    hexo deploy ,提示Error: Permission denied (publikey),如下图：

   ![error](assets/blogImg/public_key_error.JPG)

- 查看用户配置的ssh的public key 是否存在

    执行命令： ssh -vT git@github.com,结果如下：

 ![testkeyfailue](assets/blogImg/testkeyfailue.JPG)

    提示 permission denied
    进入 cd ~/.ssh,查看没有 id_rsa id_rsa_pub ,所以需要生成public_key。

- 生成publish key
  
 使用Git Bash生成新的ssh key。

$ cd ~  #保证当前路径在”~”下

$ ssh-keygen -t rsa -C "xxxxxx@yy.com"  #建议填写自己真实有效的邮箱地址

Generating public/private rsa key pair.

Enter file in which to save the key (/c/Users/xxxx_000/.ssh/id_rsa):   #不填直接回车

Enter passphrase (empty for no passphrase):   #输入密码（可以为空）

Enter same passphrase again:   #再次确认密码（可以为空）

成功的生成如下提示：

 ![testkeyfailue](assets/blogImg/generatekey.JPG)

- 添加 ssh key 到 Github

登录GitHub系统；点击右上角账号头像的“▼”→Settings→SSH and GPG keys→New SSH key
进入c:/Users/xxxx_000/.ssh/目录下，打开id_rsa.pub文件，全选复制公钥内容。
把复制的内容放入key中，Title 自定义，点击确定。

- 验证public key 是否生效

再次执行 $ ssh -vT "xxxxxx@yy.com",成功了提示 success,效果如下：

 ![testsuccess](assets/blogImg/testsuccess.JPG)