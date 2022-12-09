---
title: Markdown笔记
date: 2022-07-20 16:34:31
tags: markdown, md
toc: true
comment: true	
---

# RESTful API

---

## RESTful API简介
*   定义
	-   RESTful API中的REST是英文Representational State Transfer（具体状态转移〉的缩写，是一种基于HTTP协议、URI（统一资源定位符）、JSON和XML这些现有协议与标准的，针对网络应用的设计和开发方式。  
*   RESTful设计
	-   资源。RESTful设计中，系统中所有对象都被抽象为资源，资源通过URI指向。例如：有一个电子商务汇总系统，用户、商品、订单和其他等都是组成系统的资源。  
	-   HTTP状态（HTTP Method）。在RESTful设计中，对于资源的具体操作类型由HTTP协议和各种动作组成。常见动作有：
		1.   GET：从服务器中获取一项或者多项资源。（相当于数据库操作的查找）
		2.   POST：在服务器新建一个资源。（相当于数据库操作的新增）
		3.   DELETE：从服务器删除资源。（相当于数据库操作的删除）
		4.   PUT/PATCH：在服务器上更新资源。区别在于：是否只提供修改的属性，PUT操作需要客户端提供完整修改后的对象，PATCH只需要提供要修改的属性即可。（相当于数据库操作的修改）  
*   实现接口权限验证
	-   开发用户接口实例之前，还需要设计一套严格的接口验证方法，保证API接口的安全性。常用的接口验证一般包括
		1.   接口时效性验证。为了防止大量的重复请求，可以设置每次的请求时效。小到以秒为单位，大到半个小时。
		2.   接口参数完整性验证。一般的接口攻击，都会截获请求，附带额外的参数	进行攻击，需要进行过滤验证，此时需要客户端把所有的请求参数根据一个特定的算法，生成一个签名字符串，并在请求时一并发送。服务器接收的请求之后先把对这个签名字符串进行校验
		3.   用户唯一Token，用户每次登录都会生成唯一的Token信息，注销或者会话超时时Token都会被注销，防止被非法获取。  
*   数据库和用户表创建   
	-   新建数据库   
	```SQL
	CREATE DATABASE restful_api;
	```   
	-   创建用户数据表   
	```SQL
	USE restful_api;
	DROP TABLE IF EXISTS `user`;
	CREATE TABLE `user` (
		`id` INT(10) NOT NULL AUTO_INCREMENT COMMENT '用户id，自动增长',
		`mobile` VARCHAR(255) NOT NULL COMMENT '用户手机号',
		`name` VARCHAR(255) NOT NULL COMMENT '用户名',
		`email` VARCHAR(255) NOT NULL COMMENT '用户邮箱',
		`status` TINYINT(4) NOT NULL COMMENT '用户状态',
		`create_time` INT(10) NOT NULL COMMENT '用户创建时间',
		`update_time` INT(10) COMMENT '用户更新时间',
		PRIMARY KEY(`id`)
	) ENGINE = InnoBD DEFAULT CHARSET = utf8;
	```   
*   对用户资源（用户模型）的操作   
	|  标志  | 请求类型 |  生成路由规则  | 对应操作方法（默认） |     描述     |
	| :----: | :------: | :------------: | :------------------: | :----------: |
	| index  |   GET    |     users      |        index         | 显示用户列表 |
	| create |   GET    |  users/create  |        create        | 新增用户页面 |
	|  save  |   POST   |     users      |         save         | 保存用户信息 |
	|  read  |   GET    |   users/:id    |         read         | 查看用户信息 |
	|  edit  |   GET    | users/:id/edit |         edit         | 编辑用户页面 |
	| update |   PUT    |   users/:id    |        update        | 更新用户信息 |
	| delete |  DELETE  |   users/:id    |        delete        | 删除用户信息 |   



