---
title: git相关操作
date: 2020-07-22 17:00:21
tags: 
    - clone
    - ssh
    - branch
categories:
    - git
---
# git创建项目，并绑定本地项目
    Git init
    Git add .
    Git commit -m ‘first’
    git remote add origin https://github.com/kukuboy/actity.git
    git push -u origin master
    git pull
    git push --set-upstream origin master

# git多分支管理

在github上创建新分支dev,一个两个分支管理，master用于生产，dev用于开发，开发一个版本后再合并即可。


        // 查看所有分支
        git branch -a
            * master
            remotes/origin/dev
            remotes/origin/master
        // 创建本地分支
        git branch dev
        // 切换到dev分支
        git checkout dev
        // 查看分支对应管理
        $ git branch -vv
            * dev    3593014 [origin/dev] first commit
            master 29b0ba9 [origin/master] first commit
        // 本地dev绑定远程dev
        git branch --set-upstream-to=origin/dev



# github 绑定ssh
>使用ssh可以在每次使用时不用重复的填写密码
## windows操作系统下
>使用git bash的终端

1.  查看现有
        
        ls -al ~/.ssh

2. 若出现此项目，说明已存在可跳过生成，也可重新生成
 
        id_rsa // 私钥
        id_rsa.pub // 公钥

3. 生成，替换自己邮件

        $ ssh-keygen -t ed25519 -C "your_email@example.com"
           注：如果您使用的是不支持 Ed25519 算法的旧系统，请使用以下命令：
        
            $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

        //提示您“Enter a file in which to save the key（输入要保存密钥的文件）”时，按 Enter 键。 这将接受默认文件位置。

        Enter a file in which to save the key (/c/Users/you/.ssh/id_rsa):[Press enter]

        //在提示时输入安全密码。 

        Enter passphrase (empty for no passphrase): 
        Enter same passphrase again: 

3. 将 SSH 密钥添加到 ssh-agent

        ssh-add ~/.ssh/id_rsa

确保 ssh-agent 正在运行。可手动启动

            //在后台启动 ssh-agent
            $ eval $(ssh-agent -s)
            > Agent pid 59566

4. 添加到个人账户中

    1. 打开github 并登录
    2. 打开个人setting
    3. 找到SSH and GPG keys
    4. 新建并命名
    5. 进入git-bash复制对应
           //此操作可直接复制
           clip < ~/.ssh/id_rsa
    6. 添加保存即可


# git更改https为ssl

        // 查看之前的地址

        $ git remote -v
            origin  https://baidu.com/aaa/aaa.git (fetch)
            origin  https://baidu.com/aaa/aaa.git (push)
        
        // 复制新的ssl地址放在下面语句后执行
        $ git remote set-url origin git @github.com:baidu/aaa.git 
