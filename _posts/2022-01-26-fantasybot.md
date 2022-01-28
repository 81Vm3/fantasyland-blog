---
layout: post
title: 国内首创！SAMP QQ群消息互通插件
subtitle: 可爱的fantasybot女仆
categories: 技术
tags: [技术, 介绍]
---

## 前言
老早就在discord看到SAMP游戏内消息和群消息互通的插件，它能使服务器向discord群组发送消息。

我突发奇想，能不能也给SAMP写一个QQ群消息互通插件呢？

## 思路
实际上，这是可行的。在百度一番搜索下，发现[mirai](https://mirai.mamoe.net/)是个不错的QQ机器人

问题来了，如何将mirai和SAMP结合起来呢？

SAMP给了我们一条思路 —— [SAMP Plugin SDK](https://github.com/maddinat0r/samp-plugin-sdk/tree/f6361d7439ae6b39b0b679fde5e7af53804bba9f)

通过SAMP插件SDK，开发者们可以用C++编写SAMP的插件，从而实现扩展功能

而[mirai-cpp](https://github.com/cyanray/mirai-cpp)，mirai的C++ SDK，给了我们一个可以将SAMP插件和mirai结合起来的机会

所以，要想编写出一个联动QQ群和SAMP的插件，就要结合mirai-cpp和SAMP的插件SAMP，从而实现第一步

## 过程
前置准备
 1. [SAMP Plugin SDK](https://github.com/maddinat0r/samp-plugin-sdk/tree/f6361d7439ae6b39b0b679fde5e7af53804bba9f)
 2. [Mirai](https://mirai.mamoe.net/)
 3. [mirai-api-http](https://github.com/project-mirai/mirai-api-http)
 4. [mirai-cpp](https://github.com/cyanray/mirai-cpp)
 5. [SAMPGDK，用于调用SAMP的原生函数](https://github.com/Zeex/sampgdk/tree/master/include)
 6. Visual Studio 2017
 7. 一个QQ小号
 8. C++ 知识
 9. 人脑

**本文将介绍的是编写插件的基本思路，并不介绍如何搭建mirai机器人服务**

**基本过程**
 1. 使用mirai-cpp的模板
 2. 将SAMP插件SDK 加入mirai-cpp模板
 3. 将SAMPGDK 加入mirai-cpp模板
 4. 在mirai-cpp的GroupMessage回调中，使用将SAMPGDK的SendClientMessageToAll，这实现了QQ群消息->游戏内
 5. 自己写一个native函数，用于发送QQ群消息
 6. 在你的AMX脚本中调用此函数，这实现了游戏内->QQ群

## 注意事项
从mirai服务的搭建，到插件的编写，再到博客的编写，总共花费了我30小时以上的时间

其中我栽的最大的坑就是插件与mirai二者结合，和编码的问题:
 * 服务器中文使用gb2312，而mirai使用utf-8
 * 新旧C++标准带来不兼容
 * SendMessage多线程问题

在这里特别记下两条预处理器定义，vs2017真的搞得我脑袋爆炸，如果不在项目设置里面定义就会报错
> HAVE_STDINT_H
> 
> SAMPGDK_AMALGAMATION

 * 不在预处理器定义HAVE_STDINT_H导致samp插件SDK重定义变量类型，**只有与mirai-cpp结合时才出现的bug**
 * 不在预处理器定义SAMPGDK_AMALGAMATION导致sampgdk无法调用native，**只有与mirai-cpp结合时才出现的bug**

## 预览:
女仆在qq群内发送服内消息
![](/assets/images/screenshots/fantasybot/20220128164259.png)

samp内可看到群员的消息
![](/assets/images/screenshots/sa-mp-024.png)

喵女仆
![](/assets/images/screenshots/fantasybot/76596270_p0_master1200.jpg)