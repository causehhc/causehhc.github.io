+++
author = "Hugo Authors"
title = "test"
date = "2021-04-26"
description = "Sample article showcasing basic Markdown syntax and formatting for HTML elements."
tags = [
    "markdown",
    "css",
    "html",
    "themes",
]
categories = [
    "themes",
    "syntax",
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]
+++

# VFT
本项目包含3部分  
1. 系统基本功能
2. 用户功能
3. 特色功能
4. 用户网页路由
5. MySql数据库设计
6. Redis数据库设计
# 系统基本功能
1. 根据RSS收集信息  
2. 整合信息（聚类）
3. 添加or删除源  
4. 定时根据源更新信息
# 用户功能
1. Login、Logout  
2. 订阅or取消源  
3. 收藏or取消信息  
4. 关注or取关其他用户  
# 特色功能
1. 基于文本聚类的力导向图
# 用户网页路由
1. /login  
2. /  
3. /dynamic  
4. /export  
5. /anls  
6. /src/[SID]
7. /user/[UID]
# MySql数据库设计
1. src：源表
    - SID：唯一标识ID
    - SName：RSS别名
    - SURL：RSS源
    - SUpdated：RSS更新时间（nullable）
2. user：用户表
    - UID：唯一标识ID
    - UName：用户名（unique）
    - UPassword：密码
3. info：信息表  
    - IID：唯一标识ID
    - SID：唯一标识ID（fk-src）
    - ITitle：标题
    - ILink：原文链接
    - ISummer：摘要
    - IUpdated：该条信息的刷新时间
4. usertoinfo：用户收藏信息  
    - ID：唯一标识ID  
    - UID：唯一标识ID（fk-user）
    - IID：唯一标识ID（fk-info）
5. usertosrc：用户订阅源  
    - ID：唯一标识ID
    - UID：唯一标识ID（fk-user）
    - SID：唯一标识ID（fk-src）
6. usertouser：用户关注其他用户  
    - ID：唯一标识ID
    - UID：唯一标识ID（fk-user）
    - UIDsHost：唯一标识ID（fk-user）


