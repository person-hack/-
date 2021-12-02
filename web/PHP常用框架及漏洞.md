# PHP常用框架及漏洞
## laraval
Laravel基于MVC架构，可以满足诸如事件处理、用户身份验证等各种需求，同时通过包管理实现模块化和可扩展的代码，并且对数据库管理有着健壮的支持
### 漏洞
        1.  Laravel 配置文件泄露

        2.  Laravel <= 8.4.2 存在远程代码执行漏洞

        3.  Laravel 存在SQL注入漏洞

        4.  Laravel 反序列化漏洞
 
## CakePHP
cakephp 是一个比较流行的PHP框架。类似其它语言的框架一样。（如java的hibernate框架）它的model层有一对多，多对一，多对多等关系。开发中在建表等数据层处理十分方便、快速开发(同样不用程序员写那些烦杂的关联链接的sql语句)。正是因为这些优点，它因此要付出代价。相对于其他PHP不采用框架的开发.cakephp 框架内部采用了大量的数组

### 漏洞
         1.  Cake PHP XML外部实体漏洞

         2.  Cake PHP XSS漏洞

         3.  Cake PHP 安全绕过漏洞

## Symfony2

Symfony2是一个基于PHP语言的Web开发框架，有着开发速度快、性能高等特点。

### 漏洞
        1.  Symfony2  xss漏洞

        2.  Symfony2  远程安全漏洞

        3.  Symfony2  组件漏洞

        4.  Symfony2  探查器漏洞
 
 ## CodeIgniter
 CodeIgniter 是一个为用 PHP 编写网络应用程序的人员提供的工具包。它的目标是实现让你比从零开始编写代码更快速地开发项目，为此，CI 提供了一套丰富的类库来满足通常的任务需求，并且提供了一个简单的接口和逻辑结构来调用这些库。CodeIgniter 可以将需要完成的任务代码量最小化，这样你就可以把更多的精力放到项目的开发上了。
 ### 漏洞
   1.  Yii 反序例化漏洞

## Aura
Aura 的主要目标是为 PHP 开发者提供一个高质量、可测试、标准化组件的框架。Aura 有相当大的一部分用户，使用方法和Cake PHP 类似。Aura 项目围绕着一系列高质量，经过良好测试，语义版本，符合标准的独立库包，可用于任何代码库
### 漏洞
        1.  Aura 跨站请求漏洞

        2.  Aura XSS漏洞

        3.  Aura 绝对路径遍历漏洞

        4.  Aura SQL注入漏洞
 
## Zend
它是一个用于快速开发现代Web 应用程序的开源MVC框架。Zend Framework有几个松散耦合的组件，所以它被称为“组件库”。Zend Framework提供任何PHP堆栈和Zend服务器来运行Zend框架应用程序
### 漏洞
        1.  Zend 任意文件读取漏洞

        2.  Zend 远程代码执行漏洞

        3.  Zend XML外部实体及身份绕过漏洞
  
## Kohana
kohana 框架是一个相对比较小众的php框架 ，是有一个开源组织开发的mvc框架。
### 漏洞
        1.  Kohana 任意代码执行漏洞

        2.  Kohana SQL注入漏洞

## Slim
Slim是一个PHP微框架，可以帮助您快速编写简单但功能强大的Web应用程序和API。

使用此框架应用程序快速设置并开始处理新的Slim Framework 3应用程序。此应用程序使用最新的Slim 3和PHP-View模板渲染器。它还使用Monolog记录器。
### 漏洞
        1.  Slim  XXE漏洞

## Typo3
Typo3内容管理系统，是基于PHP4/PHP5+MYsql的内容管理系统(框架)(CMS/CMF),兼容PHP4和PHP5.数据库系统除Mysql之外，也能运行于Oracle, MS-SQL, ODBC, LDAP 等其它数据库系统，支持Typo3的服务器系统：Apache或者IIS架设的服务器。

### 漏洞
        1.  Typo3 反序例化漏洞

        2.  Typo3 CMS 身份绕过漏洞

        3.  Typo3  CMS SQL注入漏洞

        4.  Typo3 跨站脚本漏洞

        5.  Typo3  OpenlD Extension任意站点重定向漏洞

        6.  Typo3 Extension Table Administration库数据库表修改漏洞

        7. TYPO3 Form Content Element授权绕过信息泄露漏洞

        8. TYPO3内容编辑向导权限检查漏洞

## ThinkPHP
ThinkPHP是一个快速、兼容而且简单的轻量级国产PHP框架，ThinkPHP从诞生以来一直秉承简洁实用的设计原则，在保持出色的性能和至简的代码的同时，也注重易用性。并且拥有众多原创功能和特性，在社区团队的积极参与下，在易用性、扩展性和性能方面不断优化和改进
### 漏洞
        1.  ThinkPHP2.x远程代码执行漏洞

        2.  ThinkPHP3.2.3 SQL注入漏洞

        3.  ThinkPHP3日志泄露

        4.  ThinkPHP5.0.23远程代码执行漏洞

        5.  ThinkPHP5.0.21远程命令执行漏洞

        6.  ThinkPHP5.1.X SQL注入漏洞

        7.  ThinkPHP6 SQL注入漏洞

## Swoole
Swoole 使 PHP 开发人员可以编写高性能高并发的 TCP、UDP、Unix Socket、HTTP、 WebSocket 等服务，让 PHP 不再局限于 Web 领域。
### 漏洞
        1.  Swoole 内存泄露漏洞
 
 ## Nette
 Nette一个PHP框架。
 ### 漏洞
         1.  Nette未授权远程代码执行漏洞
   
 ## Drupal
 Drupal 是用PHP语言写成的自由开源内容管理系统，常被视为内容管理框架，而非一般意义上的内容管理系统。 整套平台把所有内容视为一个“节点”，背后由大量“模块” 控制其显示、修改、排列、分类等方式。
 
 ### 漏洞
        1.  Drupal  远程代码执行漏洞

        2.  Drupal 7.31 SQL注入漏洞

        3.  Drupal  访问权限绕过漏洞

        4.  Drupal  目录遍历漏洞

        5.  Drupal  任意代码执行漏洞
 

参考:https://blog.csdn.net/qq_48985780/article/details/119700346
 








