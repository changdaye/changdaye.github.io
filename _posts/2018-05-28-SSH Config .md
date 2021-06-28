---
layout:     post
title:     	SSH Config
subtitle:    "\"管理多主机\""
date:       2018-05-28
author:     Mr Chang
header-img: img/post-bg-e2e-ux.jpg
catalog: true
tags:
    - SSH Config
    
---


# 使用

1. 一般我们使用ssh连接远程主机的时候，使用命令是：
    
        ssh root@ip 
        
        ssh –i [identity-file] -p [port] user@hostname
    
2. 但是如果ip地址过多，其实根本记不住

3. 然后我们就可以用到config管理配置了

        vim ~/.ssh/config
    
        增加以下配置
        
        Host <alias>
        HostName <ip-address>
        Port <port>
        User <username>
        IdentityFile <path_to_your_private_key>
    
4. 之后就可以使用以下命令直接登录

        ssh <alias>


   