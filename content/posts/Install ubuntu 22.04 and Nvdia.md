+++
author = "causehhc"
title = "Install ubuntu 22.04 and Nvdia"
date = "2024-02-24"
description = ""
tags = ["Ubuntu"]
categories = ["嵌入式"]
+++

## 1. 目标机硬件参数

## 2. 制作ubuntu启动盘
使用rufus-3.19.exe将ubuntu-22.04.2-desktop-amd64.iso刷到u盘

## 3. 准备目标及存储空间
100GB：磁盘管理 && DiskGenius

## 4. 划分
/: all

## 5. setting
- 禁用自动更新
- apt-get update
- apt-get install openssh-server
- sudo ubuntu-drivers install
- sudo apt-get install nvidia-cuda-toolkit

## 6. use rdp