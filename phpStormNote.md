---
title: PHPStorm笔记
date: 2020-07-10 15:17:34
tags: 
---

## PHPStormNote

---

### PHPStorm常用快捷操作
*   创建自定义快捷方式
	-   先为当前快捷键方式模板新建一个副本（可自定义名称），之后选中副本，在副本中修改
	-   选中要使用的快捷方式，右键其选中添加键盘组合，按下需要的使用的按键组合，再点击应用之后再确定
*   常用新建文件快捷方式
	-   新建文件夹 `Alt+Insert, Ctrl+N`
	-   新建文件	`Ctrl+Alt+Insert`
	-   新建行
	```
	在当前行之前新建行： Ctrl+Alt+Enter
	在当前行之后新建行： Ctrl+Shift+Enter
	```
*   常用查找快捷方式
	-   文件内查找 `Ctrl+F`
	-   查找文件 `Ctrl+Shift+N`
	-   查找类 `Ctrl+N`
	-   查找方法 `Ctrl+Shift+Alt+N`
	-   查找设置项 `Ctrl+Shift+A`
*   文件切换
	-   当前视图中文件切换`Shift+Tab, Ctrl+E`
*   自定义文件模板和代码片段
	-   PHPStorm 文件模板，在查找设置项中输入 File and CodeTemplates，就可以看到PHPStorm中的对应文件类型的预设模板
	-   自定义活动模板（代码片段）
		+   查找设置项中输入live templates，新添加一个模板组，再添加一个模板，填写对应的缩写和描述，再输入代码片段内容之后，再选择define，选中对应的文件类型（如：php），确定之后就可以在对应的文件类型中使用代码模板了。
*   方法重构
	-   选中重复的代码段，之后按住 `Ctrl+Alt+M`，填写方法名在选中需要的项再点击重构就可以了
*   变量多点编辑
	-   对某个关键字同时进行修改（需要选中对应的数量）`Ctrl+R`
	-   对选中的方法中的多用同名变量进行修改`Shift+F6`

---

### PHPStorm集成Xdebug调试插件
*   安装PHP Xdebug插件扩展
	-   先打印当前php配置信息，然后复制到[xdebug自动版本选择](https://xdebug.org/wizard)填入之后会给对应的版本，下载它给的版本就可以了
	```PHP
	<?PHP
		echo phpinfo();
	```
	-   下载之后放到对应扩展文件夹中（网站会后显示路径，放在路径中就可以了）
	-   修改php配置文件（php.ini）（网站中也会显示你当前的php配置文件路径）
	```INI
	; Xdebug扩展文件路径
	zend_extension="F:\MySoftwares\PhpStudy\PHPTutorial\php\php-7.2.1-nts\ext\php_xdebug-3.1.5-7.2-vc15-nts.dll"
	; Xdebug的日志文件路径
	xdebug.output_dir = "F:\MySoftwares\PhpStudy\PHPTutorial\tmp\xdebug"
	xdebug.log=xdebug.log
	; 开启调试，选择对应的调试模式
	xdebug.mode=debug,trace
	xdebug.start_with_request = yes
	; 选择主机
	xdebug.client_host=127.0.0.1
	; 选择端口
	xdebug.client_port = 9055
	; 选择协议
	xdebug.remote_handler=dbgp
	; IDEA调试KEY
	xdebug.idekey=PHPSTORM
	```
	-   重启服务，再次打印php配置文件查看xdebug是否已经配置生效（也可直接在命令函中运行`php -v`查看php配置文件是否有错误）
*   在PHPStorm中配置Xdebug
	-   在PHPStorm文件\-\>设置\-\>语言与框架\-\>PHP\-\>调试\-\>xdebug项中填写配置的端口号
	-   再在PHPStorm文件\-\>设置\-\>语言与框架\-\>PHP\-\>DBPg代理中，填入之前之前配置文件中的对应项
	-   再在PHPStorm文件\-\>设置\-\>语言与框架\-\>PHP\-\>服务器中，添加要调试的项目对应的server配置（项目绑定的域名和端口号），应用，确定
	-   再在PHPStorm运行\-\>编辑配置，新建添加PHP Web页面，添加之前新建服务器，填入项目对应的url（域名，端口号和入口文件）确定
	-   给要调试的代码设置断点，点击虫子图标进行调试

---

### composer安装及使用
*   composer简介
	-   composer是PHP用来管理依赖关系(dependency)的工具。通常在开发PHP项目时会引用一些第三方的类库工具，而第三方的类库工具可能还会依赖一些其他的工具。使用composer会大大简化依赖管理操作
*   composer安装
	-   window安装，[下载composer](https://getcomposer.org/Composer-Setup.exe)，默认会下载最新版本的composer，之后一直默认安装就可以了
	-   linux安装，[下载composer](https://getcomposer.org/installer)
	```shell
	# 下载composer
	curl -sS https://getcomposer.org/installer | php
	# 移动到系统文件夹中
	mv composer.phar /usr/local/bin/composer
	```
	-   之后打开cmd，运行
	```SHELL
	# 查看composer的版本
	composer -V / --version
	```
*   composer配置
	-   composer修改仓库源为阿里云源
	```SHELL
	# -g 为全局配置
	composer config -g repo.packagist composer https://mirrors.aliyun.com/composer
	# 取消配置
	composer config -g --unset repos.packagist
	# 仅修改当前工程配置，进当前项目使用该镜像地址
	composer config repo.packagist composer https://mirrors.aliyun.com/composer
	```
	-   调试
	```SHELL
	# composer 命令增加 -vvv 可输出详细的信息
	composer -vvv require alibabacloud/sdk
	```
*   composer管理常用命令
	-   升级
	```SHELL
	composer self-update
	```
	-   执行诊断命令
	```SHELL
	composer diagnose
	```
	-   清除缓存
	```SHELL
	composer clear
	```
	-   若项目之前已经通过其他源安装，则需要更新composer.lock文件。
	```SHELL
	composer update --lock
	```
*   composer的使用
	-   composer.json文件
		+   通过项目根目录下的composer.json文件来安装需要的依赖。composer.json文件描述了项目的依赖关系
		```JSON
		// composer.json文件示例
		{
			"require": {
				"monolog": "1.2/*"
			}
		}
		```
		+   之后运行composer install命令进行安装
		```SHELL
		composer install 
		```
	-   composer.lock文件
		+   在安装依赖之后，composer将把安装时确切的版本号列表写入composer.lock文件，该文件将锁定该项目的依赖的特定版本
		+   一般情况需要提交项目的composer.json和composer.lock文件到版本库中。因为在执行install命令时会自动检查	锁文件是否存在。如果存在，它将下载指定版本（忽略.json文件中定义的版本信息），这样当任何人项目建立时都将下载指定版本信息的依赖，避免不同版本的依赖对项目产生的影响
	-   search
		+   search 命令可以搜索包
		```SHELL
		compser search monolog
		# 该命令会输出包及其描述信息，如果只想输出包名可以使用 --only-name 参数
		composer search --onlyp-name monolog
		```
	-   require命令
		+   可直接运行require packageName 命令直接安装
		```SHELL
		composer require phpmailer/phpmailer
		```
		+   Composer 会先找到合适的版本，然后更新composer.json文件，在 require 那添加 monolog/monolog 包的相关信息，再把相关的依赖下载下来进行安装，最后更新 composer.lock 文件并生成 php 的自动加载文件。
	-   update命令
		+   需要注意的时，包能升级的版本会受到版本约束的约束，包不会升级到超出约束的版本的范围。例如如果 composer.json 里包的版本约束为 ^1.10，而最新版本为 2.0。那么 update 命令是不能把包升级到 2.0 版本的，只能最高升级到 1.x 版本。
	```SHELL
	# 更新所有依赖
	composer update
	# 更新指定的包
	composer update phpmailer/phpmailer
	# 更新多个包
	composer update phpmailer/phpmailer monolog/monolog
	# 通过通配符匹配包
	composer update phpmailer/phpmailer symfony/*
	```
	-   remove
		+   remove 命令用于移除一个包及其依赖（在依赖没有被其他包使用的情况下），如果依赖被其他包使用，则无法移除
		```SHELL
		composer remove monolog/monolog
		```
	-   show 
		+   show命令可以列出当前项目使用到的包信息
		```SHELL
		# 列出所有已经安装的包
		composer show
		# 可以通过通配符进行筛选
		composer show monolog/*
		# 显示具体某个包的信息
		composer show monolog/monolog
		```
*   使用composer建立项目
	-   create-project，会在当前目录下以新建项目目录并把需要的包包含在项目目录里，并且生成composer.json和composer.lock文件
	```SHELL
	composer create-project monolog/monolog monolog
	```
*   提交自定义包到composer
	-   项目中频繁使用到的代码段，通常被封装成全局的方法和类库，以提高代码的复用性。但初始化项目，手动引入比较麻烦，此时可以提交到composer上自动安装
	-   本地创建composer包
		+   在github中创建应用仓库
		+   使用composer在本地初始化
		+   在本地开发类库，并与composer建立对应关系
		+ 	提交到github应用仓库
		+   提交github仓库地址到packagist后完成发布
	-   把要复用的类封装在一个新建仓库中（本地仓库或者远程仓库都可以），一般是在GitHub新建空仓库之后再把空仓库（记得要有.gitignore文件）克隆到本地
	-   在clone的仓库目录使用`composer init`初始化仓库，期间会有需要填写的类项
	```
	# 填写包名，格式为：供应商/包名，以确定包的唯一性
	Package name (<vendor>/<name>) [xxx/xxx] : 
	# 包描述，简略描述该包/类的主要功能
	Description [] : xxx
	# 包作者和包作者邮箱
	Author [xxx <xxx@example.com>, n to skip] :
	# 包发布的最低要求，填写dev可以让GitHub上的代码直接同步到Packagist.org，填写stable则需要在打了tag后才能发布
	Minimum Stability [] : stable
	# 包类型，默认选择library即可，还有项目、插件等类型
	Packag Type ( e. g. library, project, metapackage, composer-plugin) [] : library
	# 授权类型，可以不填，按回车略过
	License [] : MIT
	# 是否定义当前的依赖项，可以输入no后回车跳过
	Would you like to define your dependencies (require) interactively [yes]?
	# 选择搜索依赖的包，这里输入php
	Search for a package : php
	# 输入最低版本约束，这里填写php版本
	Enter the version constraint to require (or leave blank to use the latest version): >=5.3
	Search for a package:
	# 是否有其他的约束，此时没有，略过
	Would you like to define your dev dependencies (require-dev) interactively [yes]?
	Search for a package:
	# composer.json文件预览
	{
		"name": "xxx/xxx",
		...
		"minimum-stability": "stable"
	}
	# 确认是否创建composer.json文件
	Do you confirm generation [yes]? yes
	# 是否将vendor类库目录加入Git的忽略文件中，可选
	Would you like 	the vendor directory added to your .gitignore [yes]? yes
	# 随后查看仓库目录结构
	```
	-   生成的composer.json文件中的定义项，可以自行修改或者添加，为了方便把类库地址和composer的自动加载对应，可以手动配置，如映射类库的命名空间的实际目录，命名空间的定义不一定要和文件夹目录完全一致，可以根据需求自行修改
	-   编写包核心类库（把复用的类封装在这里）
	-   建立类库与composer关系（把编写的类库让composer自动加载生成在vendor文件中）
	```SHELL
	# composer将根据composer.json文件的定义的依赖自动加载到vendor文件夹中
	composer install
	```
	-   测试类功能是否正常实现
	-   把类库提交到GitHub仓库，并在packagist中发布类库
		+   把需要忽略版本同步的文件及文件夹添加到.gitignore文件中，如：日志文件，vendor，IDEA环境文件，测试文件等
		+   把编写好的文件提交到本地仓库分支上，再同步推送到GitHub远程仓库分支上
		```SHELL
		git add .
		git commit -m 'xxxx'
		git push -u origin main
		```
		+   查看Github仓库，复制仓库地址
		+   注册登录[packagist](https://packagist.org/)，登录成功之后点击[submit](https://packagist.org/packages/submit)，填入GitHub仓库地址检查是否有重名包，提交
	-   给仓库打上tag，发布正式版本
		+   给本地参考打tag
		```SHELL
		git tag -a v1.0.0 -m='v1.0.0'
		git tag
		git push origin --tags
		```
		+   更新packagist。刷新类库地址，已经有版本号了
	-   测试类库
		+   新建项目目录，使用composer安装新发布的类库依赖
		```SHELL
		composer search xxx/xxd-demo
		composer require xxx/xxx-demo
		```
		+   在入口文件，引入composer自动加载脚本，使用编写的类库的功能
	-   遇到的问题
		+   包同名问题，一般是作者同名问题，把composer.json文件中的name和author\-\>name修改之后，再同步到远程仓库，再到提交页面测试是否同名
		+   search和require安装依赖类库找不到类库问题，一般是composer仓库配置源问题，把仓库源删除之后再把源修改为Packagist源
		```
		composer config --unset repos.packagist
		composer clear
		composer config repo.packagist composer https://repo.packagist.org
		composer search xxx/xxx-demo
		composer require xxx/xxx-demo
		```