---
title: 新建项目
date: 2022-07-20 16:34:31
tags: markdown, md
toc: true
comment: true	
---

## 新建项目

---

### php创建项目
*   从一个全新的系统或者一个虚拟机来作为开发环境为实例。
	1.   安装必备软件和环境（php, git, composer, mysql, nginx, ssh, docker, IDEA），IDEA（开发环境）可选 phpstorm, vs code, sublime text, Atom, 推荐使用phpstrom（集成功能比较齐全，安装一些插件之后便可），  服务器环境可使用phpStudy、lxmmp集成包，有多个不同版本php和nginx/apache可以选择
*   php新建项目或者把项目放到本地环境进行开发（这里phpStudy作为集成环境，nginx作为服务器来开发），还有数据库图形化访问软件（Dbeaver，navicat），其中navicat是收费软件
	1.   使用composer、git、svn把项目文件拉取到本地，其中composer适用于使用开源框架创建新建项目，git、svn等版本控制器适用于私有项目仓库。
	2.   配置本地调试环境，打开 `C:\Windows\System32\drivers\etc` 中的hosts文件，添加如下配置：
	```
	#  mytest.com
	172.0.0.1 mytest.com
	```   
	3.   配置项目的nginx配置文件，可以在nginx的安装目录 `F:\MySoftwares\PhpStudy\PHPTutorial\nginx` 下的 `conf\vhost.d` 文件夹下新建mytest的配置文件  `mytest.com.conf ` 修改文件为下列配置。一般都是静态文件出错404等，注意看项目的静态文件目录是否和项目文件中设置的静态文件路径是否一致，修改nginx中的静态文件路径和项目中的访问路径一致。
	```
	# mytest.com.conf的配置文件
	server {
		# 设置监听的端口
		listen 80;
		# 设置监听的域名
		server_name mytest.com;
		# 设置网站根目录
		root 'F:/mytest/public/';

		# 设置首页访问的默认文件
		location / {
			index index.html index.htm index.php default.html;

			# autoindex on 
			# 隐藏入口文件，设置页面伪静态配置
			if ( !-e $request_filename) {
				rewrite ^(.*)$ /index.php/$1 last;
				break;
			}
		}

		# 设置图片和字体文件不转发到fastcgi处理，并设置缓存1天
		location ~* \.(jpg|jpeg|png|gif|swf|oft|eot|svg|ttf|woff|woff2)$ {
			if (-f $request_filename) {
				# 设置文件访问的目录
				alias '/public/';
				# 设置缓存
				expires 30d;
				break;
			}	
		}

		# 设置js和css文件不转发到fastcgi处理，并设置缓存1天
		location ~* \.(js|css)$ {
			if (-f $request_filename) {
				alias '/public/';
				expires 1d;
				break;
			}
		}

		# 配置静态文件直接访问静态文件夹
		location ~* \.(gif|jpg|jpeg|bmp|png|css|map|js|swf|oft|eot|svg|ttf|woff|woff2)$ {
	        if (-f $request_filename) {
	            root 'F:/mytest/public';
	            # alias 'F:/mytest/public/';
	            # expires 30d;
	            break;
	        }
	    }

		# 监听所有的.php文件的请求转发到fast_cgi处理
		location ~ \.php(.*)$ {
			fastcgi_pass 127.0.0.1:9000;
			fastcgi_index index.php;
			fastcgi_split_path_info ^((?U).+\.php)(/?.+)$;
			fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
			fastcgi_param PATH_INFO $fastcgi_path_info;
			fastcgi_param PATH_TRANSLATED $document_root$fastcgi_path_info;
			include fastcgi_params;
		}

	}
	```
	4.   修改nginx的配置文件之后要先测试配置文件中的是否有错，没有之后就重启nginx服务。
	```SHELL
	sudo nginx -t
	sudo nginx -s reload
	```
	5.   在浏览器中打开mytest.com，就可以测试了。
	6.   可能遇到的问题：hosts文件没有配置，导致计算机访问DNS服务器返回真实的域名解析。nginx配置文件错误，等等   
*   配置项目的数据库连接   
	-   如果是新项目，那么就直接建库建表，再把数据库的连接数据写到项目的配置文件中。
	-   如果是已有的私有仓库项目，可以先用数据库图形化软件测试数据库连接是否成功。其中有可能需要通过跳板机连接（就是访问远程数据库连接要通过ssh连接），先测试数据库的ssh连接是否成功，再测试访问的数据配置是否成功。数据库配置文件一般仓库里的配置文件会有，不过最好还是先问一下上级，让上级给到配置文件再测试连接。

```SQL
use wangcmf;

drop table if exists `db_user`;

create table `db_user` (
	`id` int(10) not null auto_increment,
	`name` varchar(255) not null default '' comment '用户姓名',
	`mobile` varchar(20) not null default '' comment '手机号，唯一性标识',
	`password` varchar(255) not null default '' comment '用户密码',
	`type` tinyint(2) not null default '0' comment '0：普通用户，1：管理员',
	`status` int(10) not null default '0'	 comment '-1：已删除，0：禁用，1：正常',
	`create_time` int(10) not null default '0' comment '创建时间',
	`update_time` int(10) not null default '0' comment '更新时间',
	primary key(`id`) using btree,
	unique key `mobile` (`mobile`) using btree comment '手机号不能重复' 	 
) engine=InnoDB default charset=utf8mb4 row_format=compact;
```