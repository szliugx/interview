php高级面试题

服务器运行环境
PHP常用的几种运行环境，各自的优缺点是什么。是否有独立配置优化服务器的经历，如何配置优化的。
```
window: wamp wnmp
mac: mamp mnmp
linux: lamp lnmp lampn
php和mysql没什么区别，操作系统不同，web服务器不同，开始用apache后面用nginx，主要发现nginx反向代理方便及轻量级
```

PHP语言的熟悉程度
PHP4、PHP5的主要区别，PHP5.3 的重大更新有哪些。之前的项目用的PHP是什么版本的，为什么选择这个版本。
```
公司开发用的一直是5.6的大版本，主要觉得它是php7之前比较新、稳定的版本，7出后，一直没有生产环境使用，平时个人开发时用
```

开发模式和 OOP
是否参与过多人协作项目，在多人协作项目中担任什么角色。Bug 如何管理，需求如何确定。项目采用什么开发框架，为什么采用这个开发框架。代码如何管理，测试发布的流程是如何做的。
```
主要承担项目主程以及需求分析、项目设计、任务分解及耗时预估的角色
Bug 禅道或者开源程序trac上管理
需求有项目组所有人员进行需求分析
项目采用YII2进行开发，公司统一规定（当然个人更喜欢laravel进行开发，路由、队列、任务比较棒）
代码管理主要是svn，个人开发时用git
开发完成开发任务时转测试时，根据测试需求进行测试任务，测试通过线上发布（无自动化测试，静态代码检查）
```
安全意识
是否遇到过安全问题，常见的安全问题有哪些需要注意的。XSS 和 SQL 注入分别如何防范。
```
网站上没有对用户输入代码进行过滤htmlspecialcharts，用户通过iframe引入他自己的网站
开启_CSRF或者token校验
SQL注入 输入时没有过滤，通过pdo的占位符方法
```

性能优化和系统架构
之前的系统 QPS 是多少。多少台服务器（顺便问问架构），每日的 PV 是多少。如果换你来优化，该怎么做。性能的瓶颈如何定位，给定一个使用场景，如何规划服务器架构。是否使用过缓存，是否使用过 NoSQL，Memcache\Redis 等适用场景是什么。消息队列是否了解过。
```
QPS最开始测时是100多一点
每日平常uv 1-2w ，活动时更高 
整个项目组10台左右
web 两台
微信中控 2台
基础服务 2台
crm系统 2台
卡券系统 2台
商品系统 2台
一般采用的一台机子多服务部署

采用了redis缓存展示数据，采用了influxDB记录监控日志，mongoDB记录业务操作数据
消息队列： beanstalk
```


整理了一份PHP高级工程师的笔试题，问题很全面。基本上这些题都答得不错，那么你应该可以胜任大部分互联网企业的PHP职位了。下面直接上题：

1. 基本知识点

HTTP协议中几个状态码的含义:503,500,401,200,301,302。。。(答案参考这篇文章：深入理解HTTP协议（二）——协议详解篇)
```
200 请求正常
301 永久跳转
302 临时跳转
401 未授权
500 服务器内部错误
503 服务器负载过大，暂时不能处理请求
```
include,require,include_once,require_once 的区别。（答案参考这篇文章：PHP基础之流程控制9——require/include/require_once/include_once）
```
1.include()和require()的区别：
include()引入文件时，如果遇到错误会提示错误并继续执行；require()引入文件遇到错误时也提示错误但会终止程序的运行。
require()语句一般放在PHP脚本页面的最前面，php在执行前，就先读入require()引入的文件，文件的内容会变成此脚本的一部分，一旦出现错误，则立即退出程序。
include()语句一般在用到时才引入，所以通常是放在流程控制的处理区段中，php脚本在执行到它时，才会将文件包含进来。
即require()是预加载机制，位置在脚本最前面，一开始就引入所有可能用到的文件；include()是即用即加载，位置灵活。

2.include()、require()和include_once()、require_once()的区别：
include()、require()执行即包含文件，不会对引入的文件进行比较判断，可能会出现重复包含的情况；而include_once()、require_once()在包含时会先判断文件是否
已经包含过了，如果已包含，则不再包含文件，这样的引入文件方式即可以节省资源，又可避免重复定义的错误。
```
PHP/Mysql中几个版本的进化史，比如mysql4.0到4.1，PHP 4.x到5.1的重大改进等等。

HEREDOC介绍.
```
Heredoc技术，在正规的PHP文档中和技术书籍中一般没有详细讲述，只是提到了这是一种Perl风格的字符串输出技术。但是现在的一些论坛程序，和部分文章系统，都巧妙的使用heredoc技术，来部分的实现了界面与代码的准分离，phpwind模板就是一个典型的例子。
.以<<<End开始标记开始，以End结束标记结束，结束标记必须顶头写，不能有缩进和空格，且在结束标记末尾要有分号 。开始标记和开始标记相同，比如常用大写的EOT、EOD、EOF来表示，但是不只限于那几个，只要保证开始标记和结束标记不在正文中出现即可。

2.位于开始标记和结束标记之间的变量可以被正常解析，但是函数则不可以。在heredoc中，变量不需要用连接符.或,来拼接，如下：

$v=2;
$a= <<<EOF
"abc"$v
"123"
EOF;
echo $a; //结果连同双引号一起输出："abc"2 "123"

3.heredoc常用在输出包含大量HTML语法d文档的时候。比如：函数outputhtml()要输出HTML的主页。可以有两种写法。很明显第二种写法比较简单和易于阅读。

function outputhtml(){
echo "<html>";
echo "<head><title>主页</title></head>"; 
echo "<body>主页内容</body>";
echo "</html>;
}

function outputhtml()
{
echo <<<EOT
   <html>
   <head><title>主页</title></head>
   <body>主页内容</body>
   </html>
EOT;
}
```


写出一些php魔术方法。
```
魔术方法

__construct()， __destruct()， __call()， __callStatic()， __get()， __set()， __isset()， __unset()， __sleep()， __wakeup()， __toString()， __invoke()， __set_state()， __clone() 和 __debugInfo() 等方法在 PHP 中被称为"魔术方法"（Magic methods）。在命名自己的类方法时不能使用这些方法名，除非是想使用其魔术功能。

Caution
PHP 将所有以 __（两个下划线）开头的类方法保留为魔术方法。所以在定义类方法时，除了上述魔术方法，建议不要以 __ 为前缀。
__sleep() 和 __wakeup()

public array __sleep ( void )
void __wakeup ( void )
serialize() 函数会检查类中是否存在一个魔术方法 __sleep()。如果存在，该方法会先被调用，然后才执行序列化操作。此功能可以用于清理对象，并返回一个包含对象中所有应被序列化的变量名称的数组。如果该方法未返回任何内容，则 NULL 被序列化，并产生一个 E_NOTICE 级别的错误。

Note:
__sleep() 不能返回父类的私有成员的名字。这样做会产生一个 E_NOTICE 级别的错误。可以用 Serializable 接口来替代。
__sleep() 方法常用于提交未提交的数据，或类似的清理操作。同时，如果有一些很大的对象，但不需要全部保存，这个功能就很好用。

与之相反，unserialize() 会检查是否存在一个 __wakeup() 方法。如果存在，则会先调用 __wakeup 方法，预先准备对象需要的资源。

__wakeup() 经常用在反序列化操作中，例如重新建立数据库连接，或执行其它初始化操作。

Example #1 Sleep 和 wakeup

<?php
class Connection 
{
    protected $link;
    private $server, $username, $password, $db;
    
    public function __construct($server, $username, $password, $db)
    {
        $this->server = $server;
        $this->username = $username;
        $this->password = $password;
        $this->db = $db;
        $this->connect();
    }
    
    private function connect()
    {
        $this->link = mysql_connect($this->server, $this->username, $this->password);
        mysql_select_db($this->db, $this->link);
    }
    
    public function __sleep()
    {
        return array('server', 'username', 'password', 'db');
    }
    
    public function __wakeup()
    {
        $this->connect();
    }
}
?>
__toString()

public string __toString ( void )
__toString() 方法用于一个类被当成字符串时应怎样回应。例如 echo $obj; 应该显示些什么。此方法必须返回一个字符串，否则将发出一条 E_RECOVERABLE_ERROR 级别的致命错误。

Warning
不能在 __toString() 方法中抛出异常。这么做会导致致命错误。
Example #2 简单示例

<?php
// Declare a simple class
class TestClass
{
    public $foo;

    public function __construct($foo) 
    {
        $this->foo = $foo;
    }

    public function __toString() {
        return $this->foo;
    }
}

$class = new TestClass('Hello');
echo $class;
?>
以上例程会输出：

Hello
需要指出的是在 PHP 5.2.0 之前，__toString() 方法只有在直接使用于 echo 或 print 时才能生效。PHP 5.2.0 之后，则可以在任何字符串环境生效（例如通过 printf()，使用 %s 修饰符），但不能用于非字符串环境（如使用 %d 修饰符）。自 PHP 5.2.0 起，如果将一个未定义 __toString() 方法的对象转换为字符串，会产生 E_RECOVERABLE_ERROR 级别的错误。

__invoke()

mixed __invoke ([ $... ] )
当尝试以调用函数的方式调用一个对象时，__invoke() 方法会被自动调用。

Note:
本特性只在 PHP 5.3.0 及以上版本有效。
Example #3 使用 __invoke()

<?php
class CallableClass 
{
    function __invoke($x) {
        var_dump($x);
    }
}
$obj = new CallableClass;
$obj(5);
var_dump(is_callable($obj));
?>
以上例程会输出：

int(5)
bool(true)
__set_state()

static object __set_state ( array $properties )
自 PHP 5.1.0 起当调用 var_export() 导出类时，此静态 方法会被调用。

本方法的唯一参数是一个数组，其中包含按 array('property' => value, ...) 格式排列的类属性。

Example #4 使用 __set_state()>（PHP 5.1.0 起）

<?php

class A
{
    public $var1;
    public $var2;

    public static function __set_state($an_array) // As of PHP 5.1.0
    {
        $obj = new A;
        $obj->var1 = $an_array['var1'];
        $obj->var2 = $an_array['var2'];
        return $obj;
    }
}

$a = new A;
$a->var1 = 5;
$a->var2 = 'foo';

eval('$b = ' . var_export($a, true) . ';'); // $b = A::__set_state(array(
                                            //    'var1' => 5,
                                            //    'var2' => 'foo',
                                            // ));
var_dump($b);

?>
以上例程会输出：

object(A)#2 (2) {
  ["var1"]=>
  int(5)
  ["var2"]=>
  string(3) "foo"
}
__debugInfo()

array __debugInfo ( void )
This method is called by var_dump() when dumping an object to get the properties that should be shown. If the method isn't defined on an object, then all public, protected and private properties will be shown.

This feature was added in PHP 5.6.0.

Example #5 Using __debugInfo()

<?php
class C {
    private $prop;

    public function __construct($val) {
        $this->prop = $val;
    }

    public function __debugInfo() {
        return [
            'propSquared' => $this->prop ** 2,
        ];
    }
}

var_dump(new C(42));
?>
以上例程会输出：

object(C)#1 (1) {
  ["propSquared"]=>
  int(1764)
}
add a note add a note

```


一些编译php时的configure参数。
```
具体参数含义可以用./configure --help来查看。
列表如下（部分参数未得到解释）：
# 指定 PHP 安装目录
--prefix=/usr/local/php 
# 指定php.ini位置
--with-config-file-path=/usr/local/php/etc 
# MySQL安装目录，对mysql的支持
--with-mysql=/usr/local/mysql
mysqli扩展技术不仅可以调用MySQL的存储过程、处理MySQL事务，而且还可以使访问数据库工作变得更加稳定。
--with-mysqli=/usr/local/mysql/bin/mysql_config   
整合 apache，apxs功能是使用mod_so中的LoadModule指令，加载指定模块到 apache，要求 apache 要打开SO模块
--with-apxs2=/usr/local/apache/bin/apxs 
# 选项指令 --with-iconv-dir 用于 PHP 编译时指定 iconv 在系统里的路径，否则会扫描默认路径。
--with-iconv-dir=/usr/local 
 
--with-freetype-dir   打开对freetype字体库的支持 
--with-jpeg-dir   打开对jpeg图片的支持 
--with-png-dir   打开对png图片的支持 
--with-zlib-dir   打开zlib库的支持，用于http压缩传输
--with-libxml-dir   打开libxml2库的支持
--disable-rpath    关闭额外的运行库文件 
--enable-bcmath    打开图片大小调整,用到zabbix监控的时候用到了这个模块
--enable-shmop --enable-sysvsem  这样就使得你的PHP系统可以处理相关的IPC函数了。
--enable-inline-optimization  优化线程
--with-curl    打开curl浏览工具的支持 
--with-curlwrappers    运用curl工具打开url流 
--enable-mbregex
--enable-fpm 打上PHP-fpm 补丁后才有这个参数，CGI方式安装的启动程序
--enable-mbstring    多字节，字符串的支持 
--with-mcrypt                    mcrypt算法扩展
--with-mhash                     mhash算法扩展
--with-gd    打开gd库的支持 
--enable-gd-native-ttf   支持TrueType字符串函数库
--with-openssl      openssl的支持，加密传输https时用到的
--enable-pcntl   freeTDS需要用到的，可能是链接mssql 才用到
--enable-sockets     打开 sockets 支持
--with-xmlrpc    打开xml-rpc的C语言 
--enable-zip   打开对zip的支持 
--enable-ftp   打开ftp的支持 
 
--with-bz2    打开对bz2文件的支持        
--without-iconv   关闭iconv函数，字符集间的转换 
--with-ttf     打开freetype1.*的支持，可以不加了 
--with-xsl     打开XSLT 文件支持，扩展了libXML2库 ，需要libxslt软件 
--with-gettext     打开gnu 的gettext 支持，编码库用到 
--with-pear    打开pear命令的支持，PHP扩展用的 
--enable-calendar    打开日历扩展功能
--enable-exif    图片的元数据支持 
--enable-magic-quotes    魔术引用的支持 
 
--disable-debug    关闭调试模式 
--with-mime-magic=/usr/share/file/magic.mime      魔术头文件位置
CGI方式安装才用的参数
 
--enable-fastCGI            支持fastcgi方式启动PHP
--enable-force-CGI-redirect        重定向方式启动PHP
--with-ncurses         支持ncurses 屏幕绘制以及基于文本终端的图形互动功能的动态库
--with-gmp  应该是支持一种规范
--enable-dbase                     建立DBA 作为共享模块
--with-pcre-dir=/usr/local/bin/pcre-config      perl的正则库案安装位置
--disable-dmalloc
--with-gdbm                     dba的gdbm支持
--enable-sigchild
--enable-sysvshm
--enable-zend-multibyte         支持zend的多字节
--enable-wddx
--enable-soap
```
向php传入参数的三种方法。
```
<?php
    /*
     * 方法一 使用$argc $argv
     *  在命令行下运行 /usr/local/php/bin/php ./getopt.php -f 123 -g 456
     */
//    if ($argc > 1){
//        print_r($argv);
//    }

    /**
     * 运行结果
     *
     sync@MySUSE11:~/web_app/channel3/interface> /usr/local/php/bin/php ./getopt.php -f 123 -g 456
        Array
        (
            [0] => ./getopt.php
            [1] => -f
            [2] => 123
            [3] => -g
            [4] => 456
        )
     */





     /*
     * 方法二 使用getopt函数（）
     *  在命令行下运行 /usr/local/php/bin/php ./getopt.php -f 123 -g 456
     */

//    $options = "f:g:";
//    $opts = getopt( $options );
//    print_r($opts);

    /**
     * 运行结果
     *
     sync@MySUSE11:~/web_app/channel3/interface> /usr/local/php/bin/php ./getopt.php -f 123 -g 456
        Array
        (
            [f] => 123
            [g] => 456
        )
     */



    /*
     * 方法三 提示用户输入，然后获取输入的参数。有点像C语言
     *  在命令行下运行 /usr/local/php/bin/php ./getopt.php
     */
    fwrite(STDOUT, "Enter your name: ");
    $name = trim(fgets(STDIN));
    fwrite(STDOUT, "Hello, $name!");
    /**
     * 运行结果
     *
     sync@MySUSE11:~/web_app/channel3/interface> /usr/local/php/bin/php ./getopt.php
     Enter your name: francis
     Hello, francis!
     */

?>
```
(mysql)请写出数据类型(int char varchar datetime text)的意思; 请问varchar和char有什么区别;
```
MySQL 数据类型
MySQL中定义数据字段的类型对你数据库的优化是非常重要的。
MySQL支持多种类型，大致可以分为三类：数值、日期/时间和字符串(字符)类型。
数值类型
MySQL支持所有标准SQL数值数据类型。
这些类型包括严格数值数据类型(INTEGER、SMALLINT、DECIMAL和NUMERIC)，以及近似数值数据类型(FLOAT、REAL和DOUBLE PRECISION)。
关键字INT是INTEGER的同义词，关键字DEC是DECIMAL的同义词。
BIT数据类型保存位字段值，并且支持MyISAM、MEMORY、InnoDB和BDB表。
作为SQL标准的扩展，MySQL也支持整数类型TINYINT、MEDIUMINT和BIGINT。下面的表显示了需要的每个整数类型的存储和范围。
类型	大小	范围（有符号）	范围（无符号）	用途
TINYINT	1 字节	(-128，127)	(0，255)	小整数值
SMALLINT	2 字节	(-32 768，32 767)	(0，65 535)	大整数值
MEDIUMINT	3 字节	(-8 388 608，8 388 607)	(0，16 777 215)	大整数值
INT或INTEGER	4 字节	(-2 147 483 648，2 147 483 647)	(0，4 294 967 295)	大整数值
BIGINT	8 字节	(-9 233 372 036 854 775 808，9 223 372 036 854 775 807)	(0，18 446 744 073 709 551 615)	极大整数值
FLOAT	4 字节	(-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38)	0，(1.175 494 351 E-38，3.402 823 466 E+38)	单精度
浮点数值
DOUBLE	8 字节	(-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308)	0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308)	双精度
浮点数值
DECIMAL	对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2	依赖于M和D的值	依赖于M和D的值	小数值
日期和时间类型
表示时间值的日期和时间类型为DATETIME、DATE、TIMESTAMP、TIME和YEAR。
每个时间类型有一个有效值范围和一个"零"值，当指定不合法的MySQL不能表示的值时使用"零"值。
TIMESTAMP类型有专有的自动更新特性，将在后面描述。
类型	大小
(字节)	范围	格式	用途
DATE	3	1000-01-01/9999-12-31	YYYY-MM-DD	日期值
TIME	3	'-838:59:59'/'838:59:59'	HH:MM:SS	时间值或持续时间
YEAR	1	1901/2155	YYYY	年份值
DATETIME	8	1000-01-01 00:00:00/9999-12-31 23:59:59	YYYY-MM-DD HH:MM:SS	混合日期和时间值
TIMESTAMP	4	1970-01-01 00:00:00/2037 年某时	YYYYMMDD HHMMSS	混合日期和时间值，时间戳
字符串类型
字符串类型指CHAR、VARCHAR、BINARY、VARBINARY、BLOB、TEXT、ENUM和SET。该节描述了这些类型如何工作以及如何在查询中使用这些类型。
类型	大小	用途
CHAR	0-255字节	定长字符串
VARCHAR	0-65535 字节	变长字符串
TINYBLOB	0-255字节	不超过 255 个字符的二进制字符串
TINYTEXT	0-255字节	短文本字符串
BLOB	0-65 535字节	二进制形式的长文本数据
TEXT	0-65 535字节	长文本数据
MEDIUMBLOB	0-16 777 215字节	二进制形式的中等长度文本数据
MEDIUMTEXT	0-16 777 215字节	中等长度文本数据
LONGBLOB	0-4 294 967 295字节	二进制形式的极大文本数据
LONGTEXT	0-4 294 967 295字节	极大文本数据
CHAR和VARCHAR类型类似，但它们保存和检索的方式不同。它们的最大长度和是否尾部空格被保留等方面也不同。在存储或检索过程中不进行大小写转换。
BINARY和VARBINARY类类似于CHAR和VARCHAR，不同的是它们包含二进制字符串而不要非二进制字符串。也就是说，它们包含字节字符串而不是字符字符串。这说明它们没有字符集，并且排序和比较基于列值字节的数值值。
BLOB是一个二进制大对象，可以容纳可变数量的数据。有4种BLOB类型：TINYBLOB、BLOB、MEDIUMBLOB和LONGBLOB。它们只是可容纳值的最大长度不同。
有4种TEXT类型：TINYTEXT、TEXT、MEDIUMTEXT和LONGTEXT。这些对应4种BLOB类型，有相同的最大长度和存储需求。
```
error_reporting 等调试函数使用。（答案参考这篇文章：PHP扩展之错误处理和日志记录）
```
PHP错误处理和日志记录

PHP提供了方便，灵活的手段，报告和记录错误，警告，通知由PHP生成在编译时，运行时，由于一些用户操作。发展报告灵敏度的控制权，无论这些信息是如何显示到浏览器的信息是否被记录到一个文件或系统日志（syslog中在Linux，Windows的事件日志）。未来15指令控制这种行为。

在一些公司里面，有专门的日志收集系统。日志收集系统会在背后默默的帮你收集错误、警告、提示。
也有些公司没有专门的日志收集系统，通过文件来服务器当中的运行日志。
其中：PHP的错误，警告这些是必须要收信的。
那么问题来了——不让用户看到，设置好错误报告级别好，如何将错误收集到日志系统中呢？
这里有需要使用到php.ini的相关配置项。这两个配置项为：
参数	配置项	说明
log_errors	on/off	是否开启日志记录
log_errors_max_len	整型，默认1024	单行错误最大记录长度
error_log	syslog或者指定路径	错误日志记录在什么地方
说明：
    1.在表格中的log_errors和log_errors_max_len非常好理解。
    2.而error_log 指定将错误存在什么路径上。配置项中的syslog可能有点不太好理解。syslog是指系统来记录。windows系统在电脑的日志收集器里面。linux默认在：/etc/syslog.conf
[扩展] 了解知识点。若Linux系统启动或修改了日志收集。可能存储在第三方专用的日志收集服务器中。
此外，PHP还为我们专门准备了一个自定义的错误日志函数：
bool error_log ( string $错误消息 [, int $错误消息类型 = 0 [, string $存储目标]] )
这个函数可以把错误信息发送到web服务器的错误日志，或者到一个文件里。
常用的错误消息类型：
错误消息类型	说明
0	发送至默认的error_log指定位置
1	发送到指定的邮件位置
3	发送至指定的文件位置

示例：
<?php

//无法连接到数据库服务器，直接记录到php.ini 中的error_log指定位置
error_log("无法连接到数据库服务器服务器");

//可以发送邮件，但是php.ini必须配置过邮件系统
error_log('可以用邮件报告错误，让运维人员半夜起床干活',1 ,'pig@php.cn');

//记录在指定的位置
error_log("我是一个错误哟", 3, "d:/test/my-errors.log");

?>
注意：
error_log 中发送邮件可能对初学者不熟，您可以不用掌握些块知识。

```
您是否用过版本控制软件? 如果有您用的版本控制软件的名字是?（答案参考这两篇文章：Linux中svn安装、配置和使用和版本控制工具 git 使用入门指南）
```
svn git
```
posix和perl标准的正则表达式区别。
```
posix和perl标准的正则表达式区别
1、正则表达式（Regular Expression，缩写为regexp，regex或regxp)，又称正规表达式、正规表示式或常规表达式或正规化表示法或正规表示法，是指一个用来描述或者匹配一系列符合某个句法规则的字符串的单个字符串 。在很多文本编辑器或其他工具里，正则表达式通常被用来检索和/或替换那些符合某个模式的文本内容 。许多程序设计语言都支持利用正则表达式进行字符串操作。例如，在Perl中就内建了一个功能强大的在正则表达式引擎。正则表达式这个概念最初是由Unix中的工具软件（例如sed和grep）普及开的。

2、PHP同时使用两套正则表达式规则，一套是由电气和电子工程师协会（IEEE）制定的POSIX Extended 1003.2兼容正则（事实上php对此标准的支持并不完善），另一套来自PCRE（Perl Compatible Regular Expression）库提供PERL兼容正则，这是个开放源代码的软件，作者为Philip Hazel。
使用POSIX兼容规则的函数有：
ereg_replace()
ereg()
eregi()
eregi_replace()
split()
spliti()
sql_regcase()
mb_ereg_match()
mb_ereg_replace()
mb_ereg_search_getpos()
mb_ereg_search_getregs()
mb_ereg_search_init()
mb_ereg_search_pos()
mb_ereg_search_regs()
mb_ereg_search_setpos()
mb_ereg_search()
mb_ereg()
mb_eregi_replace()
mb_eregi()
mb_regex_encoding()
mb_regex_set_options()
mb_split()
 
使用PERL兼容规则的函数有：
preg_grep()
preg_replace_callback()
preg_match_all()
preg_match()
preg_quote()
preg_split()
preg_replace()

3、定界符：
POSIX兼容正则没有定界符，函数的相应参数会被认为是正则。
PERL兼容正则可以使用任何不是字母、数字或反斜线（\）的字符作为定界符，如果作为定界符的字符必须被用在表达式本身中，则需要用反斜线转义。也可以使用()，{}，[]和 <>作为定界符。

4、修正符：
POSIX兼容正则没有修正符。
PERL兼容正则中可能使用的修正符（修正符中的空格和换行被忽略，其它字符会导致错误）：
i (PCRE_CASELESS)：
匹配时忽略大小写。
m（PCRE_MULTILINE）：
当设定了此修正符，行起始(^)和行结束($)除了匹配整个字符串开头和结束外，还分别匹配其中的换行符(\n)的之后和之前。
s（PCRE_DOTALL）：
如果设定了此修正符，模式中的圆点元字符（.）匹配所有的字符，包括换行符。没有此设定的话，则不包括换行符。
x（PCRE_EXTENDED）：
如果设定了此修正符，模式中的空白字符除了被转义的或在字符类中的以外完全被忽略。
e：
如果设定了此修正符，preg_replace()在替换字符串中对逆向引用作正常的替换，将其作为 PHP代码求值，并用其结果来替换所搜索的字符串。 只有 preg_replace()使用此修正符，其它 PCRE函数将忽略之。
A（PCRE_ANCHORED）：
如果设定了此修正符，模式被强制为“anchored”，即强制仅从目标字符串的开头开始匹配。
D（PCRE_DOLLAR_ENDONLY）：
如果设定了此修正符，模式中的行结束($)仅匹配目标字符串的结尾。没有此选项时，如果最后一个字符是换行符的话，也会被匹配在里面。如果设定了m 修正符则忽略此选项。
S：
当一个模式将被使用若干次时，为加速匹配，值得先对其进行分析。如果设定了此修正符则会进行额外的分析。目前，分析一个模式仅对没有单一固定起始字符的 non-anchored模式有用。
U（PCRE_UNGREEDY）：
使“?”的默认匹配成为贪婪状态的。
X（PCRE_EXTRA）：
模式中的任何反斜线后面跟上一个没有特殊意义的字母导致一个错误，从而保留此组合以备将来扩充。默认情况下，一个反斜线后面跟一个没有特殊意义的字母被当成该字母本身。
u（PCRE_UTF8）：
模式字符串被当成UTF-8。

5、逻辑区隔：
POSIX兼容正则和PERL兼容正则的逻辑区隔符号作用和使用方法完全一致：
[]：包含任选一操作的相关信息。
{}：包含匹配次数的相关信息。
()：包含一个逻辑区间的相关信息，可被用来进行引用操作。
|：表示“或”，[ab]和a|b是等价的。
元字符与“[]”相关：
有两组不同的元字符：一种是模式中除了方括号内都能被识别的，还有一种是在方括号“[]”内被识别的。
POSIX兼容正则和PERL兼容正则“[]之外”“一致”的元字符：
\ 有数种用途的通用转义符
^ 匹配字符串的开头
$ 匹配字符串的结尾
? 匹配0或者1
* 匹配 0个或多个前面指定类型的字符
+ 匹配 1个或多个前面指定类型的字符
 
POSIX兼容正则和PERL兼容正则“[]之外”“不一致”的元字符：
. PERL兼容正则匹配除了换行符外的任意一个字符
. POSIX兼容正则匹配任意一个字符
 
POSIX兼容正则和PERL兼容正则“[]之内”“一致”的元字符：
\ 有数种用途的通用转义符
^ 取反字符，但仅当其为第一个字符时有效
- 指定字符ASCII范围，仔细研究ASCII码，你会发现[W-c]等价于[WXYZ\\^_`abc]
 
POSIX兼容正则和PERL兼容正则“[]之内”“不一致”的元字符：
- POSIX兼容正则中[a-c-e]的指定会抛出错误。
- PERL兼容正则中[a-c-e]的指定等价于[a-e]。
 
匹配次数与“{}”相关：
POSIX兼容正则和PERL兼容正则在匹配次数方面完全一致：
{2}：表示匹配前面的字符2次
{2,}：表示匹配前面的字符2次或多次，默认都是贪婪（尽可能多）的匹配
{2,4}：表示匹配前面的字符2次或4次
 
逻辑区间与“()”相关：
使用()包含起来的区域是一个逻辑区间，逻辑区间的主要作用是体现出一些字符出现的逻辑次序，另一个用处就是可以用来引用（可以将此区间内的值引用给一个变量）。后一个作用比较奇特：
[php] view plain copy print?
<?php  
$str = "http://www.163.com/";  
// POSIX兼容正则：  
echo ereg_replace("(.+)","<a href = \\1 >\\1</a>",$str);  
// PERL兼容正则：  
echo preg_replace("/(.+)/","<a href = $1 >$1</a>",$str);  
// 显示两个链接  
?>  
在引用的时候，括号是可以嵌套的，逻辑次序是按照“(”出现的次序来标定的。

6、类型匹配：
POSIX兼容正则：
[:upper:]：匹配所有的大写字母
[:lower:]：匹配所有的小写字母
[:alpha:]：匹配所有的字母
[:alnum:]：匹配所有的字母和数字
[:digit:]：匹配所有的数字
[:xdigit:]：匹配所有的十六进制字符，等价于[0-9A-Fa-f]
[:punct:]：匹配所有的标点符号，等价于[.,"'?!;:]
[:blank:]：匹配空格和TAB，等价于[ \t]
[:space:]：匹配所有的空白字符，等价于[ \t\n\r\f\v]
[:cntrl:]：匹配所有ASCII 0到31之间的控制符。
[:graph:]：匹配所有的可打印字符，等价于：[^ \t\n\r\f\v]
[:print:]：匹配所有的可打印字符和空格，等价于：[^\t\n\r\f\v]
[.c.]：功能不明
[=c=]：功能不明
[:<:]：匹配单词的开始
[:>:]：匹配单词的结尾

7、PERL兼容正则（这里可以看出PERL正则的强大）：
\a alarm，即 BEL字符（’0）
\cx "control-x"，其中 x 是任意字符
\e escape（’0B）
\f 换页符 formfeed（’0C）
\n 换行符 newline（’0A）
\r 回车符 carriage return（’0D）
\t 制表符 tab（’0）
\xhh 十六进制代码为 hh 的字符
\ddd 八进制代码为 ddd的字符，或 backreference
\d 任一十进制数字
\D 任一非十进制数的字符
\s 任一空白字符
\S 任一非空白字符
\w 任一数字、字母或下划线的字符
\W 任一非数字、字母或下划线的字符
\b 字分界线
\B 非字分界线
\A 目标的开头（独立于多行模式）
\Z 目标的结尾或位于结尾的换行符前（独立于多行模式）
\z 目标的结尾（独立于多行模式）
\G 目标中的第一个匹配位置
```
safe_mode 打开后哪些地方受限。（参考这篇文章：PHP特点之安全模式2——被安全模式限制或屏蔽的函数）
```
被安全模式限制或屏蔽的函数 ¶

以下安全模式列表可能不完整或不正确。

安全模式限制函数
函数名	限制
dbmopen()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。
dbase_open()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。
filepro()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。
filepro_rowcount()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。
filepro_retrieve()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。
ifx_*	sql_safe_mode 限制, (!= safe mode)
ingres_*	sql_safe_mode 限制, (!= safe mode)
mysql_*	sql_safe_mode 限制, (!= safe mode)
pg_loimport()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。
posix_mkfifo()	检查被操作的目录是否与被执行的脚本有相同的 UID（所有者）。
putenv()	遵循 ini 设置的 safe_mode_protected_env_vars 和 safe_mode_allowed_env_vars 选项。请参考 putenv() 函数的有关文档。
move_uploaded_file()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。
chdir()	检查被操作的目录是否与被执行的脚本有相同的 UID（所有者）。
dl()	当 PHP 运行在 安全模式 时，不能使用此函数。
backtick operator	当 PHP 运行在 安全模式 时，不能使用此函数。
shell_exec()（在功能上和 backticks 函数相同）	当 PHP 运行在 安全模式 时，不能使用此函数。
exec()	只能在 safe_mode_exec_dir 设置的目录下进行执行操作。基于某些原因，目前不能在可执行对象的路径中使用 ..。escapeshellcmd() 将被作用于此函数的参数上。
system()	只能在 safe_mode_exec_dir 设置的目录下进行执行操作。基于某些原因，目前不能在可执行对象的路径中使用 ..。escapeshellcmd() 将被作用于此函数的参数上。
passthru()	只能在 safe_mode_exec_dir 设置的目录下进行执行操作。基于某些原因，目前不能在可执行对象的路径中使用 ..。escapeshellcmd() 将被作用于此函数的参数上。
popen()	只能在 safe_mode_exec_dir 设置的目录下进行执行操作。基于某些原因，目前不能在可执行对象的路径中使用 ..。escapeshellcmd() 将被作用于此函数的参数上。
fopen()	检查被操作的目录是否与被执行的脚本有相同的 UID（所有者）。
mkdir()	检查被操作的目录是否与被执行的脚本有相同的 UID（所有者）。
rmdir()	检查被操作的目录是否与被执行的脚本有相同的 UID（所有者）。
rename()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。 检查被操作的目录是否与被执行的脚本有相同的 UID（所有者）。
unlink()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。 检查被操作的目录是否与被执行的脚本有相同的 UID（所有者）。
copy()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。 检查被操作的目录是否与被执行的脚本有相同的 UID（所有者）。 (on source and target)
chgrp()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。
chown()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。
chmod()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。 另外，不能设置 SUID、SGID 和 sticky bits
touch()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。 检查被操作的目录是否与被执行的脚本有相同的 UID（所有者）。
symlink()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。 检查被操作的目录是否与被执行的脚本有相同的 UID（所有者）。 （注意：仅测试 target）
link()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。 检查被操作的目录是否与被执行的脚本有相同的 UID（所有者）。 （注意：仅测试 target）
apache_request_headers()	在安全模式下，以“authorization”（区分大小写）开头的标头将不会被返回。
header()	在安全模式下，如果设置了 WWW-Authenticate，当前脚本的 uid 将被添加到该标头的 realm 部分。
PHP_AUTH 变量	在安全模式下，变量 PHP_AUTH_USER、PHP_AUTH_PW 和 PHP_AUTH_TYPE 在 $_SERVER 中不可用。但无论如何，您仍然可以使用 REMOTE_USER 来获取用户名称（USER）。（注意：仅 PHP 4.3.0 以后有效）
highlight_file(), show_source()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。 检查被操作的目录是否与被执行的脚本有相同的 UID（所有者）。 （注意，仅在 4.2.1 版本后有效）
parse_ini_file()	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。 检查被操作的目录是否与被执行的脚本有相同的 UID（所有者）。 （注意，仅在 4.2.1 版本后有效）
set_time_limit()	在安全模式下不起作用。
max_execution_time	在安全模式下不起作用。
mail()	在安全模式下，第五个参数被屏蔽。（注意，仅自 PHP 4.2.3 起受影响）
session_start()	The owner of a script must be the same as owner of a session.save_path directory if the default files session.save_handler is used.
All filesystem and stream functions.	检查被操作的文件或目录是否与被执行的脚本有相同的 UID（所有者）。 检查被操作的目录是否与被执行的脚本有相同的 UID（所有者）。 (see the safe_mode_include_dir php.ini option.
add a note add a note

```
写代码来解决多进程/线程同时读写一个文件的问题。
```
首先PHP是支持进程的而不支持多线程（这个先搞清楚了），如果是对于文件操作，其实你只需要给文件加锁就能解决，不需要其它操作，PHP的flock已经帮你搞定了。
用flock在写文件前先锁上，等写完后解锁，这样就实现了多线程同时读写一个文件避免冲突。大概就是下面这个流程

[php] view plain copy
/*  
*flock(file,lock,block)  
*file 必需，规定要锁定或释放的已打开的文件  
*lock 必需。规定要使用哪种锁定类型。  
*block 可选。若设置为 1 或 true，则当进行锁定时阻挡其他进程。  
*lock  
*LOCK_SH 要取得共享锁定（读取的程序）  
*LOCK_EX 要取得独占锁定（写入的程序）  
*LOCK_UN 要释放锁定（无论共享或独占）  
*LOCK_NB 如果不希望 flock() 在锁定时堵塞  
/*  
  
  
if (flock($file,LOCK_EX))  
{  
fwrite($file,'write more words');  
flock($file,LOCK_UN);  
}  
else  
{  
//处理错误逻辑  
}  
fclose($file);  
)  
但这一般是有问题的，让我们看一个更好的分享:
在这里提供4种高并发读写文件的方案，各有优点，可以根据自己的情况解决php并发读写文件冲突的问题
对于日IP不高或者说并发数不是很大的应用，一般不用考虑这些！用一般的文件操作方法完全没有问题。但如果并发高，在我们对文件进行读写操作时，很有可能多个进程对进一文件进行操作，如果这时不对文件的访问进行相应的独占，就容易造成数据丢失。
例如：一个在线聊天室（这里假定把聊天内容写入文件），在同一时刻，用户A和用户B都要操作数据保存文件，首先是A打开了文件，然后更新里面的数据，但这里B也正好也打开了同一个文件，也准备更新里面的数据。当A把写好的文件保存时，这里其实B已经打开了文件。但当B再把文件保存回去时，这里已经造成了数据的丢失，因为这里B用户完全不知道它所打开的文件在它对其进行更改时，A用户也更改了这个文件，所以最后B用户保存更改时，用户A的更新就被会丢失。
对于这样的问题，一般的解决方案时当一进程对文件进行操作时，首先对其它进行加锁，意味着这里只有该进程有权对文件进行读取，其它进程如果现在读，是完全没有问题，但如果这时有进程试图想对其进行更新，会遭到操作拒绝，先前对文件进行加锁的进程这时如果对文件的更新操作完毕，这就释放独占的标识，这时文件又恢复到了可更改的状态。接下来同理，如果那个进程在操作文件时，文件没有加锁，这时，它就可以放心大胆的对文件进行锁定，独自享用。
一般的方案会是：
复制代码代码如下:

$fp=fopen('/tmp/lock.txt','w+');
if (flock($fp,LOCK_EX)){
    fwrite($fp,"Write something here\n");
    flock($fp,LOCK_UN);
}else{
    echo 'Couldn\'t lock the file !';
}
fclose($fp);

但在PHP中，flock似乎工作的不是那么好！在多并发情况下，似乎是经常独占资源，不即时释放，或者是根本不释放，造成死锁，从而使服务器的cpu占用很高，甚至有时候会让服务器彻底死掉。好像在很多linux/unix系统中，都会有这样的情况发生。所以使用flock之前，一定要慎重考虑。
那么就没有解决方案了吗？其实也不是这样的。如果flock()我们使用得当，完全可能解决死锁的问题。当然如果不考虑使用flock()函数，也同样会有很好的解决方案来解决我们的问题。经过我个人的搜集和总结，大致归纳了解决方案有如下几种。
方案一：对文件进行加锁时，设置一个超时时间。大致实现如下：
复制代码代码如下:

if($fp=fopen($fileName,'a')){
 $startTime=microtime();
 do{
  $canWrite=flock($fp,LOCK_EX);
  if(!$canWrite){
   usleep(round(rand(0,100)*1000));
  }
 }while((!$canWrite)&&((microtime()-$startTime)<1000));
 if($canWrite){
  fwrite($fp,$dataToSave);
 }
 fclose($fp);
}

超时设置为1ms，如果这里时间内没有获得锁，就反复获得，直接获得到对文件操作权为止，当然。如果超时限制已到，就必需马上退出，让出锁让其它进程来进行操作。
方案二：不使用flock函数，借用临时文件来解决读写冲突的问题。大致原理如下：
（1）将需要更新的文件考虑一份到我们的临时文件目录，将文件最后修改时间保存到一个变量，并为这个临时文件取一个随机的，不容易重复的文件名。
（2）当对这个临时文件进行更新后，再检测原文件的最后更新时间和先前所保存的时间是否一致。
（3）如果最后一次修改时间一致，就将所修改的临时文件重命名到原文件，为了确保文件状态同步更新，所以需要清除一下文件状态。
（4）但是，如果最后一次修改时间和先前所保存的一致，这说明在这期间，原文件已经被修改过，这时，需要把临时文件删除，然后返回false,说明文件这时有其它进程在进行操作。
实现代码如下：
复制代码代码如下:

$dir_fileopen='tmp';
function randomid(){
    return time().substr(md5(microtime()),0,rand(5,12));
}
function cfopen($filename,$mode){
    global $dir_fileopen;
    clearstatcache();
    do{
  $id=md5(randomid(rand(),TRUE));
        $tempfilename=$dir_fileopen.'/'.$id.md5($filename);
    } while(file_exists($tempfilename));
    if(file_exists($filename)){
        $newfile=false;
        copy($filename,$tempfilename);
    }else{
        $newfile=true;
    }
    $fp=fopen($tempfilename,$mode);
    return $fp?array($fp,$filename,$id,@filemtime($filename)):false;
}
function cfwrite($fp,$string){
 return fwrite($fp[0],$string);
}
function cfclose($fp,$debug='off'){
    global $dir_fileopen;
    $success=fclose($fp[0]);
    clearstatcache();
    $tempfilename=$dir_fileopen.'/'.$fp[2].md5($fp[1]);
    if((@filemtime($fp[1])==$fp[3])||($fp[4]==true&&!file_exists($fp[1]))||$fp[5]==true){
        rename($tempfilename,$fp[1]);
    }else{
        unlink($tempfilename);
  //说明有其它进程 在操作目标文件，当前进程被拒绝
        $success=false;
    }
    return $success;
}
$fp=cfopen('lock.txt','a+');
cfwrite($fp,"welcome to beijing.\n");
fclose($fp,'on');

对于上面的代码所使用的函数，需要说明一下：
（1）rename();重命名一个文件或一个目录，该函数其实更像linux里的mv。更新文件或者目录的路径或名字很方便。但当我在window测试上面代码时，如果新文件名已经存在，会给出一个notice,说当前文件已经存在。但在linux下工作的很好。
（2）clearstatcache();清除文件的状态.php将缓存所有文件属性信息，以提供更高的性能，但有时，多进程在对文件进行删除或者更新操作时，php没来得及更新缓存里的文件属性，容易导致访问到最后更新时间不是真实的数据。所以这里需要使用该函数对已保存的缓存进行清除。
方案三：对操作的文件进行随机读写，以降低并发的可能性。
在对用户访问日志进行记录时，这种方案似乎被采用的比较多。先前需要定义一个随机空间，空间越大，并发的的可能性就越小，这里假设随机读写空间为[1-500],那么我们的日志文件的分布就为log1~到log500不等。每一次用户访问，都将数据随机写到log1~log500之间的任一文件。在同一时刻，有2个进程进行记录日志，A进程可能是更新的log32文件，而B进程呢？则此时更新的可能就为log399.要知道，如果要让B进程也操作log32,概率基本上为1/500,差不多约等于零。在需要对访问日志进行分析时，这里我们只需要先将这些日志合并，再进行分析即可。使用这种方案来记录日志的一个好处时，进程操作排队的可能性比较小，可以使进程很迅速的完成每一次操作。
方案四：将所有要操作的进程放入一个队列中。然后专门放一个服务完成文件操作。队列中的每一个排除的进程相当于第一个具体的操作，所以第一次我们的服务只需要从队列中取得相当于具体操作事项就可以了，如果这里还有大量的文件操作进程，没关系，排到我们的队列后面即可，只要愿意排，队列的多长都没关系。
对于以前几种方案，各有各的好处！大致可能归纳为两类：
（1）需要排队（影响慢）比如方案一、二、四
（2）不需要排队。（影响快）方案三
在设计缓存系统时，一般我们不会采用方案三。因为方案三的分析程序和写入程序是不同步的，在写的时间，完全不考虑到时候分析的难度，只管写的行了。试想一下，如我们在更新一个缓存时，如果也采用随机文件读写法，那么在读缓存时似乎会增加很多流程。但采取方案一、二就完全不一样，虽然写的时间需要等待（当获取锁不成功时，会反复获取），但读文件是很方便的。添加缓存的目的就是要减少数据读取瓶颈，从而提高系统性能。
```
写一段上传文件的代码。（参考这篇文章：PHP特点之文件上传1——使用POST方法上传单文件和多文件）
```
在PHP程序开发中，文件上传是一个使用非常普遍的功能，也是PHP程序员的必备技能之一。值得高兴的是，在PHP中实现文件上传功能要比在Java、C#等语言中简单得多。下面我们结合具体的代码实例来详细介绍如何通过PHP实现文件上传和多文件上传功能。
要使用PHP实现文件上传功能，我们先来编写两个php文件：index.php和upload.php。其中，index.php页面用于提交文件上传的表单请求，upload.php页面用于接收上传的文件并进行相应处理。
首先，我们来编写一个简单的index.php文件，由于其中涉及的主要是html代码，比较简单，因此不再赘述，index.php页面的详细代码如下：
?
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
<?php
//设置编码为UTF-8，以避免中文乱码
header('Content-Type:text/html;charset=utf-8');
?>
<!DOCTYPE html>
<html>
<head>
  <title>文件上传表单页面</title>
</head>
<body>
<form action="upload.php" method="post" enctype="multipart/form-data">
文件1：<input name="upload_file1" type="file" /><br/>
文件2：<input name="upload_file2" type="file" /><br/>
<input type="submit" value="上传" />
</form>
</body>
</html>
值得注意的是，由于在HTTP协议设计之初，它并不支持文件上传功能，form表单的encrypt属性的默认值为application/x-www-form-urlencoded，它只能用于提交一般的表单请求。如果提交的表单中包含需要上传的文件，我们需要将enctype的属性值改为multipart/form-data才能实现文件上传功能。此外，method的属性值必须为post。
接着，我们继续编写upload.php文件的代码。
?
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
<?php
//设置编码为UTF-8，以避免中文乱码
header('Content-Type:text/html;charset=utf-8');
$first_file = $_FILES['upload_file1']; //获取文件1的信息
$second_file = $_FILES['upload_file2']; //获取文件2的信息$upload_dir = 'D:/upload/';  //保存上传文件的目录//处理上传的文件1
if ($first_file['error'] == UPLOAD_ERR_OK){
  //上传文件1在服务器上的临时存放路径
  $temp_name = $first_file['tmp_name'];
  //上传文件1在客户端计算机上的真实名称
  $file_name = $first_file['name'];
  //移动临时文件夹中的文件1到存放上传文件的目录，并重命名为真实名称
  move_uploaded_file($temp_name, $upload_dir.$file_name);
  echo '[文件1]上传成功!<br/>';
}else{
  echo '[文件1]上传失败!<br/>';
}
 
//处理上传的文件2
if ($second_file['error'] == UPLOAD_ERR_OK){
  //上传文件2在服务器上的临时存放路径
  $temp_name = $second_file['tmp_name'];
  //上传文件2在客户端计算机上的真实名称
  $file_name = $second_file['name'];
  //移动临时文件夹中的文件2到存放上传文件的目录，并重命名为真实名称
  move_uploaded_file($temp_name, $upload_dir.$file_name);
  echo '[文件2]上传成功!<br/>';
}else {
  echo '[文件2]上传失败!<br/>';
}
?>
在PHP中，当浏览器客户端提交过来的表单请求中包含上传的文件时，PHP会将上传的文件先暂时存放在临时目录中(在Windows操作系统中，默认的临时目录一般为C:/Windows/Temp)，然后将上传文件的相关信息存放在超全局变量$_FILES中。因此，我们只需要通过$_FILES数组获取上传的文件信息，然后对其进行相应的处理操作即可。下面，我们来看看通过浏览器上传A.gif和B.gif两个图片文件时，使用print_r()函数输出超全局变量$_FILES的详细信息：
?
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
Array ( [upload_file1] => Array ( 
 [name] => A.gif (客户端上传时的真实文件名称)
 [type] => image/gif (文件的类型)
 [tmp_name] => C:\Windows\Temp\php9803.tmp (文件上传到PHP服务器后临时存放的路径)
 [error] => 0 (错误信息，0表示没有错误)
 [size] => 87123 (文件大小，单位为字节)
 )
    [upload_file2] => Array (
 [name] => B.gif
 [type] => image/gif
 [tmp_name] => C:\Windows\Temp\php9813.tmp
 [error] => 0
 [size] => 93111
 )
)
在上面的例子中，我们上传的两个文件参数名分别为upload_file1和upload_file2。现在，我们让表单中的多个文件以相同的参数名upload_file，将刚才上传的两个文件以参数数组形式再次提交上传。此时，我们需要将index.php页面中的两个file文件域修改为如下html代码：
文件1：<input name="upload_file[]" type="file" /><br/>
文件2：<input name="upload_file[]" type="file" /><br/>
此外，我们还需要对upload.php页面进行相应的修改：
?
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
<?php
//设置编码为UTF-8，以避免中文乱码
header('Content-Type:text/html;charset=utf-8');
$fileArray = $_FILES['upload_file'];//获取多个文件的信息，注意：这里的键名不包含[]
 
$upload_dir = 'D:/upload/'; //保存上传文件的目录
foreach ( $fileArray['error'] as $key => $error) {
  if ( $error == UPLOAD_ERR_OK ) { //PHP常量UPLOAD_ERR_OK=0，表示上传没有出错
    $temp_name = $fileArray['tmp_name'][$key];
    $file_name = $fileArray['name'][$key];
    move_uploaded_file($temp_name, $upload_dir.$file_name);
    echo '上传[文件'.$key.']成功!<br/>';
  }else {
    echo '上传[文件'.$key.']失败!<br/>';
  }
}
?>
同样的，我们使用print_r()函数来查看上面例子中的超全局变量$_FILES的详细信息：
?
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
Array ( 
 [upload_file] => Array ( 
 [name] => Array ( 
  [0] => A.gif
  [1] => B.gif  
  ) 
 [type] => Array ( 
  [0] => image/gif
  [1] => image/gif  
  ) 
 [tmp_name] => Array (
  [0] => C:\Windows\Temp\php87B9.tmp
  [1] => C:\Windows\Temp\php87BA.tmp
  ) 
 [error] => Array ( 
  [0] => 0
  [1] => 0  
  ) 
 [size] => Array ( 
  [0] => 87123
  [1] => 93111  
  )
 )
)
备注1：在PHP的默认配置下，上传的文件大小超出一定的范围将会出错，请参文末提到的如何修改PHP上传文件的大小限制问题的解决方法。
备注2：上述处理文件上传的PHP代码只是一个简单的入门示例，并不能直接作为正式代码使用，因为还有许多需要额外注意的安全因素没有考虑，例如：文件的类型、文件的大小以及上传文件的名称重复等。
备注3：如果上传的文件名称中包含中文，可能引起文件名称乱码问题。此时，需要使用函数iconv()来转换文件名称的编码。
前面我们了解了如何使用PHP实现文件上传和多文件上传。不过，在PHP的默认配置情况下，当上传的文件大小超出一定的限制时，我们将得到如下的错误提示信息：
?
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
Warning: POST Content-Length of 625523488 bytes exceeds the limit of 8388608 bytes in Unknown on line 0
上述错误信息的大致意思是，我们使用POST请求提交的数据大小超过了服务器的最大限制数(8388608字节=8MB)。
 
出现上述错误的原因是，在PHP的配置文件php.ini中，默认存在如下配置信息(在php.ini中，行首的分号";"表示当前行是注释，不会生效)：
 
;脚本解析输入数据(类似 POST 和 GET)允许的最大时间，单位是秒。 它从接收所有数据到开始执行脚本进行测量的。 
max_input_time = 60
 
;允许客户端单个POST请求发送的最大数据
post_max_size = 8M
 
;是否开启文件上传功能
file_uploads = On
 
;文件上传的临时存放目录(如果不指定，使用系统默认的临时目录)
;upload_tmp_dir =
 
;允许单个请求上传的最大文件大小
upload_max_filesize = 2M
 
;允许单个POST请求同时上传的最大文件数量
max_file_uploads = 20
从上面的配置信息中我们可以看出，PHP的默认配置信息就是导致PHP文件上传时提示文件大小超出限制的「罪魁祸首」。笔者已经在上述配置信息中给出了各个指令选项对应的中文注释信息，大家可以根据自己的实际需求情况对php.ini配置文件进行相应的修改。
```
Mysql 的存储引擎,myisam和innodb的区别。（参考这篇文章：再谈MySQL存储引擎的选择:InnoDB还是MyISAM）
```
两种类型最主要的差别就是Innodb 支持事务处理与外键和行级锁.而MyISAM不支持.所以MyISAM往往就容易被人认为只适合在小项目中使用。

我作为使用MySQL的用户角度出发，Innodb和MyISAM都是比较喜欢的，但是从我目前运维的数据库平台要达到需求：99.9%的稳定性，方便的扩展性和高可用性来说的话，MyISAM绝对是我的首选。

原因如下：

1、首先我目前平台上承载的大部分项目是读多写少的项目，而MyISAM的读性能是比Innodb强不少的。

2、MyISAM的索引和数据是分开的，并且索引是有压缩的，内存使用率就对应提高了不少。能加载更多索引，而Innodb是索引和数据是紧密捆绑的，没有使用压缩从而会造成Innodb比MyISAM体积庞大不小。

3、从平台角度来说，经常隔1，2个月就会发生应用开发人员不小心update一个表where写的范围不对，导致这个表没法正常用了，这个时候MyISAM的优越性就体现出来了，随便从当天拷贝的压缩包取出对应表的文件，随便放到一个数据库目录下，然后dump成sql再导回到主库，并把对应的binlog补上。如果是Innodb，恐怕不可能有这么快速度，别和我说让Innodb定期用导出xxx.sql机制备份，因为我平台上最小的一个数据库实例的数据量基本都是几十G大小。

4、从我接触的应用逻辑来说，select count(*) 和order by 是最频繁的，大概能占了整个sql总语句的60%以上的操作，而这种操作Innodb其实也是会锁表的，很多人以为Innodb是行级锁，那个只是where对它主键是有效，非主键的都会锁全表的。

5、还有就是经常有很多应用部门需要我给他们定期某些表的数据，MyISAM的话很方便，只要发给他们对应那表的frm.MYD,MYI的文件，让他们自己在对应版本的数据库启动就行，而Innodb就需要导出xxx.sql了，因为光给别人文件，受字典数据文件的影响，对方是无法使用的。

6、如果和MyISAM比insert写操作的话，Innodb还达不到MyISAM的写性能，如果是针对基于索引的update操作，虽然MyISAM可能会逊色Innodb,但是那么高并发的写，从库能否追的上也是一个问题，还不如通过多实例分库分表架构来解决。

7、如果是用MyISAM的话，merge引擎可以大大加快应用部门的开发速度，他们只要对这个merge表做一些select count(*)操作，非常适合大项目总量约几亿的rows某一类型(如日志，调查统计)的业务表。

当然Innodb也不是绝对不用，用事务的项目如模拟炒股项目，我就是用Innodb的，活跃用户20多万时候，也是很轻松应付了，因此我个人也是很喜欢Innodb的，只是如果从数据库平台应用出发，我还是会首选MyISAM。

另外，可能有人会说你MyISAM无法抗太多写操作，但是我可以通过架构来弥补，说个我现有用的数据库平台容量：主从数据总量在几百T以上，每天十多亿 pv的动态页面，还有几个大项目是通过数据接口方式调用未算进pv总数，(其中包括一个大项目因为初期memcached没部署,导致单台数据库每天处理 9千万的查询)。而我的整体数据库服务器平均负载都在0.5-1左右。
```
2. web 架构，安全,项目经验

介绍xdebug,apc,eAccelerator,Xcache,Zend opt的使用经验。（答案参考这两篇文章：PHP调试利器XDebug的安装与使用——XDebug的简单介绍和可选PHP缓存 APC 浅析——安装、配置及工作原理）
```
PHP调试跟踪之XDebug使用总结：
Xdebug是一个开源的php程序调试工具，可以使用它来调试、跟踪及分析程序运行状态。当然，Xdebug需要结合PHP的编辑工具来打断点、跟踪、调试及分析，比较常用的PHP的Xdebug调试环境：Vim +Xdebug。
 
·     安装配置
·     调试环境
·     跟踪分析
·     注意事项
·     遇到问题
 
一、安装配置
1、安装
Xdebug的安装是作为PHP的拓展而存在的，所以可参考PHP拓展文章：
http://blog.csdn.net/why_2012_gogo/article/details/51120645
 
2、配置
php.ini:
[xdebug]
;基本调试配置
xdebug.auto_trace = on
xdebug.collect_params = on
xdebug.collect_return = on
xdebug.profiler_enable = on
xdebug.profiler_output_dir ="/php/ext/xdebug_profilers"
xdebug.trace_output_dir = "/tmp/ext/xdebug_traces"
;远程调试设置
xdebug.remote_enable = on
xdebug.remote_host = localhost
xdebug.remote_port = 9010
xdebug.remote_autostart = on
 
zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20131226/xdebug.so
 
NOTE:
上面罗列的是最为常用的配置选项，至于其他配置选项及对应的含义，请参考：
https://xdebug.org/docs/all_settings#auto_trace
 
二、调试环境
Vim + Xdebug:
1、下载
http://www.vim.org/scripts/script.php?script_id=1929
2、配置
$ cd  ~
$ sudo mkdir ~/.vim
将上面下载的xdebug的plugin中文件复制到.vim下：
$ sudo cp –r /php/ext/plugin  .
在用户主目录下创建.vimrc文件：
$ sudo touch ／usr/share/vim/vimrc  ~/.vimrc
$ sudo vim ~/.vimrc
为.vimrc添加以下内容：
let g:debuggerPort = 9010（该端口必须与xdebug.remote_port相同）
let g:debuggerMaxDepth = 5（代表数组调试深度配置）
 
NOTE:
vimrc文件是vim的主要配置文件，它包含两个版本：全局版本和用户版本，我们建议修改用户版本的vimrc配置文件，这两种版本的路径可在vim普通模式下查看，如下：
 
全局版本路径查看：
$ sudo vim
$ :echo $VIM
路径地址：/usr/share/vim
 
用户版本路径查看：
$ sudo vim
$ :echo $HOME
路径地址：$HOME（pwd ~）
 
注意：
g:debuggerPort的端口号，必须与xdebug.remote_port相同；
g:debuggerMaxDepth代表的是脚本调试的最大深度层次；
 
最后，修改完php.ini、.vimrc配置后，记得重启php-fpm。
 
3、调试
A、准备一个php文件
<?php
 $value = '马上使用XDebug调试程序，你准备好了吗';
 echo $value;
?>
将上面的文件放入到你的Web根目录下，我的访问地址是：
http://localhost/xdebug.php 测试下是否正常显示。
 
B、使用vim打开php文件
使用vim普通模式打开php文件，移动鼠标箭头到欲调试的那行，输入：
$:Bp
截图如下：

然后，按下F5(Mac:Fn+F5)，开始监听调试事件，这时在编辑窗底部提示5秒内访问要调试的php文件，例如：
http://localhost/xdebug.php? XDEBUG_SESSION_START=1
 
截图如下：

对于调试中的操作在下面附加上：
类型
功能
说明
<Command Mode>
 
 
:Bp
 toggle breakpoint
断点标记
:Up
 stack up
 
:DN
 stack down
 
<Normal Mode>
 
 
,e
 eval
 
<Function Keys>
 
 
F1
 resize
调整窗口大小
F2
 step into
调试步进入
F3
 step over
调试步进入下一标记
F4
 step out
调试步出当前标记
F5
 run
调试运行
F6
 quit debugging
退出调试模式
F11
 get all context
获得所有变量内容
F12
 get property of cursor
获得当前光标变量
 
 
三、跟踪分析
1、代码覆盖分析
Xdebug 2.2开始支持对代码覆盖的分析，也就是通过对代码的覆盖分析，我们可以了解到在IDE访问期间，有哪些代码行数被执行了，有助于对核心代码和单元测试有针对性的了解和分析，最终提高代码的质量。
A、涉及的配置
xdebug.coverage_enable=1
//该配置默认为1，也就是默认开启，如果设置为0，代码的覆盖分析就不会进行。
 
B、涉及的函数
boolean xdebug_code_coverage_started()
//该函数返回布尔值，用来判断代码覆盖分析功能是否开启，未开启则返回false。
 
void xdebug_start_code_coverage( [int options] )
//该函数没有任何返回，它的作用是开始搜集分析结果集数据，数据是以二维数组形势//存在，一维参数为分析的文件名字，二维参数为对应的分析行数；另外，在分析文件
//的每行代码时，都会产生一个结果码，如下：
//1：代表代码已经执行；
//-1：代表代码未被执行，对应函数参数XDEBUG_CC_UNUSED传入；
//-2：代表没有可执行的代码存在，对应XDEBUG_CC_DEAD_CODE和XDEBUG_CC_UNUSED
 
NOTE:
XDEBUG_CC_UNUSED：用来计算分析时包含搜集未被执行的代码；
XDEBUG_CC_DEAD_CODE：用来计算分析时代码行是否被执行；
 
形式如下：
xdebug_start_code_coverage(XDEBUG_CC_UNUSED|XDEBUG_CC_DEAD_CODE);
 
array xdebug_get_code_coverage()
//该函数返回数组值，用来搜集和返回代码覆盖分析的结果集信息。
 
void xdebug_stop_code_coverage( [int cleanup=true] )
//该函数不返回任何值，用来停止覆盖分析，如果传入参数为true，那么就会停止分析并清空内存中的分析结果集，否者传入false，反之，还可使用//xdebug_start_code_coverage找回该内存信息。
 
C、示例的验证
Php代码：
<?php
echo '覆盖分析进行中...</br>';
 
// 构建封装对象
class XdebugCoverageAnalysisModel {
      private $_coverage_info;
      private $_status;
 
      function __construct() {
          $this->_coverage_info = xdebug_get_code_coverage();
          $this->_status =xdebug_code_coverage_started();
        }
 
      // 获取分析结果
      public functiongetCodeCoverageResult() {
           returnjson_encode(xdebug_get_code_coverage());
      }
 
      // 开启覆盖分析
      public functionxdebugStartCodeCoverage() {
            xdebug_start_code_coverage( -1 | -2 );
        }
 
      // 分析是否执行
      public functionxdebugCodeStarted() {
           return xdebug_code_coverage_started();
      }
}
 
// 初始化
$apiModel = new XdebugCoverageAnalysisModel();
 
echo '开启覆盖分析...</br>';
$apiModel->xdebugStartCodeCoverage();
 
// 定义一个测试函数
function coverageSample($a,$b) {
   echo '函数结果：'.($a * $b).'</br>';
}
 
echo '判断是否开启...</br>';
$status = $apiModel->xdebugCodeStarted();
if($status=='1') {
  echo '开启覆盖分析已完成</br>';
} else {
  echo '开启覆盖分析失败了</br>';
}
 
echo '测试函数开启...</br>';
coverageSample(10,10);
 
echo '获取分析结果...</br>';
$result = $apiModel->getCodeCoverageResult();
echo $result.'</br>';
 
echo '关闭分析开关...</br>';
xdebug_stop_code_coverage();
 
$status = $apiModel->xdebugCodeStarted();
if($status=='1') {
  echo '覆盖分析已经完成</br>';
} else {
  echo '覆盖分析已经关闭！</br>';
}
 
unset($result);
unset($apiModel);
 
?>
 
浏览器结果：

 
2、PHP脚本分析
Xdebug的PHP脚本分析功能比较实用，它可以帮助我们分析代码的瓶颈和影响性能缓慢的问题，为优化代码提供可行性的参考。
 
A、涉及的配置
xdebug.profiler_enable
//该配置默认为0，为开启，设置为非0之后，即开启profiler功能
xdebug.profiler_output_dir
//该配置为上面开启之后，存放生成分析文件的位置，需要保证位置可写入，默认/tmp
xdebug.profiler_enable_trigger
//如果开启该选项，则在每次请求中如果GET/POST或cookie中包含//XDEBUG_PROFILE变量名，则才会生成性能报告文件(前提是必须关闭
//xdebug.profiler_enable选项，否则该选项不起作用)。
xdebug.profiler_output_name
//可以使用该配置修改生成的分析文件，默认cachegrind.out.%p
 
NOTE:
建议使用xdebug.profiler_enable_trigger替代xdebug.profiler_enable。
 
B、涉及的函数
string xdebug_get_profiler_filename()
//返回类型为字符串，用来返回分析的文件名字
 
C、示例的验证
当我们开启分析开关之后，当有脚本运行就会在指定的位置生成格式为cachegrind.out.xxx的分析文件：

该文件的内容不是很直观，所以需要使用可视化的工具来查看和分析，而Xdebug本身就支持使用第三方的可视化profiler文件的内容。在Linux下，可以使用KCacheGrind，而在Windows平台，可以使用QCacheGrind，当然还有一些在线的由爱好者开发的工具，例如：WebGrind，具体怎样使用这些工具，可以参考：
https://xdebug.org/docs/profiler
下面罗列下，WebGrind的效果：

 
WebGrind可以在这里下载：
https://github.com/jokkedk/webgrind
 
四、注意事项
1、避免生产环境开启profiler和trace，只需开启远程调试；
2、尽量使用xdebug.profiler_enable_trigger替代xdebug.profiler_enable；
3、如果使用webgrind分析profiler，建议不要放入生产环境，因为其没有安全限制，任何人都可以访问；
4、Xdebug的功能虽然强大，但是要均衡性能开销；
 
五、遇到问题
问题：Error("DbgProtocol instance has no attribute 'stop'",)
产生该问题的原因大致如下：
A、配置文件配置不正确；
B、.vimrc和php.ini中的port不相同；
C、.vimrc和php.ini中的port与现有的port冲突；
解决：
对照上面的几条仔细查看配置即可。
 
NOTE:
有博客说因为未在URL后添加XDEBUG_SESSION_START=1，其实不然
```

使用mod_rewrite,在服务器上没有/archivers/567.html这个物理文件时，重定向到index.php?id=567 ,请先打开mod_rewrite。
MySQL数据库作发布系统的存储，一天五万条以上的增量，预计运维三年,怎么优化？
```
优化应该不仅仅是数据库方面

使用高性能的服务器

多使用缓存

页面服务器、数据库服务器、图片服务器、上传下载服务器分离

数据库集群，表分割（水平分割和垂直分割）和表散列

负载均衡

重视每个代码开发细节，特别是大循环，多请求和SQL语句复杂的时候



1.代码要干净

2.数据库要分表，可以1个月数据存储一张表：

3.读写分离：写入用一台服务器，然后同步到另外的服务器进行读出。

4.如果有图片，建议有专门图片服务器，使用独立域名访问

5.网站前台和后台如果有条件尽量分开
```
写出一种排序算法（原理），并说出优化它的方法。
```
前提：分别用冒泡排序法，快速排序法，选择排序法，插入排序法将下面数组中的值按照从小到大的顺序进行排序。 
$arr(1,43,54,62,21,66,32,78,36,76,39);
 

1. 冒泡排序
思路分析：在要排序的一组数中，对当前还未排好的序列，从前往后对相邻的两个数依次进行比较和调整，让较大的数往下沉，较小的往上冒。即，每当两相邻的数比较后发现它们的排序与排序要求相反时，就将它们互换。

代码实现：
$arr=array(1,43,54,62,21,66,32,78,36,76,39);  
function bubbleSort($arr)
{  
  $len=count($arr);
  //该层循环控制 需要冒泡的轮数
  for($i=1;$i<$len;$i++)
  { //该层循环用来控制每轮 冒出一个数 需要比较的次数
    for($k=0;$k<$len-$i;$k++)
    {
       if($arr[$k]>$arr[$k+1])
        {
            $tmp=$arr[$k+1];
            $arr[$k+1]=$arr[$k];
            $arr[$k]=$tmp;
        }
    }
  }
  return $arr;
}

 

2. 选择排序 
思路分析：在要排序的一组数中，选出最小的一个数与第一个位置的数交换。然后在剩下的数当中再找最小的与第二个位置的数交换，如此循环到倒数第二个数和最后一个数比较为止。

代码实现：
function selectSort($arr) {
//双重循环完成，外层控制轮数，内层控制比较次数
 $len=count($arr);
    for($i=0; $i<$len-1; $i++) {
        //先假设最小的值的位置
        $p = $i;
        
        for($j=$i+1; $j<$len; $j++) {
            //$arr[$p] 是当前已知的最小值
            if($arr[$p] > $arr[$j]) {
            //比较，发现更小的,记录下最小值的位置；并且在下次比较时采用已知的最小值进行比较。
                $p = $j;
            }
        }
        //已经确定了当前的最小值的位置，保存到$p中。如果发现最小值的位置与当前假设的位置$i不同，则位置互换即可。
        if($p != $i) {
            $tmp = $arr[$p];
            $arr[$p] = $arr[$i];
            $arr[$i] = $tmp;
        }
    }
    //返回最终结果
    return $arr;
}

 

3.插入排序
思路分析：在要排序的一组数中，假设前面的数已经是排好顺序的，现在要把第n个数插到前面的有序数中，使得这n个数也是排好顺序的。如此反复循环，直到全部排好顺序。

代码实现：
function insertSort($arr) {
    $len=count($arr); 
    for($i=1, $i<$len; $i++) {
        $tmp = $arr[$i];
        //内层循环控制，比较并插入
        for($j=$i-1;$j>=0;$j--) {
            if($tmp < $arr[$j]) {
                //发现插入的元素要小，交换位置，将后边的元素与前面的元素互换
                $arr[$j+1] = $arr[$j];
                $arr[$j] = $tmp;
            } else {
                //如果碰到不需要移动的元素，由于是已经排序好是数组，则前面的就不需要再次比较了。
                break;
            }
        }
    }
    return $arr;
}

 

4.快速排序  
思路分析：选择一个基准元素，通常选择第一个元素或者最后一个元素。通过一趟扫描，将待排序列分成两部分，一部分比基准元素小，一部分大于等于基准元素。此时基准元素在其排好序后的正确位置，然后再用同样的方法递归地排序划分的两部分。

代码实现：
function quickSort($arr) {
    //先判断是否需要继续进行
    $length = count($arr);
    if($length <= 1) {
        return $arr;
    }
    //选择第一个元素作为基准
    $base_num = $arr[0];
    //遍历除了标尺外的所有元素，按照大小关系放入两个数组内
    //初始化两个数组
    $left_array = array();  //小于基准的
    $right_array = array();  //大于基准的
    for($i=1; $i<$length; $i++) {
        if($base_num > $arr[$i]) {
            //放入左边数组
            $left_array[] = $arr[$i];
        } else {
            //放入右边
            $right_array[] = $arr[$i];
        }
    }
    //再分别对左边和右边的数组进行相同的排序处理方式递归调用这个函数
    $left_array = quick_sort($left_array);
    $right_array = quick_sort($right_array);
    //合并
    return array_merge($left_array, array($base_num), $right_array);
}


```
请简单阐述您最得意的开发之作。
对于大流量的网站,您采用什么样的方法来解决各页面访问量统计问题。
您是否用过模板引擎? 如果有您用的模板引擎的名字是？
请介绍Session的原理,大型网站中Session方面应注意什么？
```
http://www.cnblogs.com/tangyanbo/p/4442242.html

原理

       在第三，四篇文章中讲到了会话保持的问题，而且还遗留了一个问题，就是会话保持存在单点故障，
当时的方案是cookie插入后缀，即haproxy指负责分发请求，应用服务自行保持用户会话，如果应
用服务器宕机，则session会丢失。
现在来温习下解决方案
方案1：session复制
原理
就是将1台服务器的session复制到其它所有的服务器上，这样无论访问哪台服务器，都会得到用户 的session
优点
不存在单点故障问题
缺点
当服务器的数量比较大时，session同步将会变得相当耗时
 方案2：session粘滞
原理
就是用户请求一个服务器之后，同一个会话的其它请求，都会被分配到这台服务器，session粘滞的 功能由负载均衡中间件完成
优点
解决了session复制的性能问题
缺点
由于用户的会话被保存到单一的服务器，就容易出现单点故障
      
 
 
 
 
方案3：session服务器
原理
部署一个专门的服务，保存用户session，同时在web服务器本地也保存一份，当本地没有或者失效时， 去访问session服务器，当然session服务器就成了单点，当用户量大的时候也容易宕机，这时可以做一 个session服务器集群，做主备同步备份，这样就达到了较好的效果，具体实现可以用redies，memcached 等缓存中间件。
优点
解决了单点故障和性能问题
缺点
实现复杂
      
 
 
 
       
      
 
redis保存session方案
      上篇文章讲到的就是session粘滞的方案，既然前2种方案都有各自的缺点，那么就采用第三中方案     
可以用redis做session缓存，保存用户session，做成主备模式，采用同步备份或者异步备份。
同步备份：在主机宕机时，备机接管之后session数据不丢失。
异步备份：在主机宕机时，备机接管主机，但是如果有一部分session还没来得及同步到备机，session将丢失。
可以根据实际情况来决定采用同步备份还是异步备份。
系统架构图如下：
image
如果用户量比较大，单服务器访问和存储session将会成为瓶颈，可以考虑用session服务器集群，架构图如下：
image
redis集群特点
1）将数据分散到集群中的多个节点，每个节点存储的数据量就会变少，这样存储和访问
的效率会得到提升。
2）每个节点都有主备，如果节点的主存储挂了，备份存储会接管主存储，提高可用性。
Redis+Tomcat实现

session流程
1.客户端首次请求服务端
2.服务端产生session并set cookie响应给客户端
3.客户端再次请求服务端，会带上cookie
4.服务端根据cookie找到对应的session
实现思路
如果我们要编写程序实现这个方案，需要解决以下问题：
1.session的安全性，即不容易被仿造。
2.session的唯一性，如果用tomcat产生session的策略，多台tomcat会产生的session会存在重复的可能。
3.session的有效期维护，session会有个有效期，用户在这个时间内不访问系统，session将会失效，如果
用户一直访问，则要自动延长session有效期。
4.在集群session服务器中，要考虑负载均衡，这也是需要编写客户端代码的，在分布式session缓存中，
需要根据sessionId哈希分布，那么就和服务器个数进行了耦合，在添加和移除服务器的时候，将出现数
据不一致的问题 。
5.如何实现才能让应用程序改动最小，或者是不改动。
我们可以选择自己写程序来实现以上功能，不过在这里我使用一个现成的框架，即tomcat-redis-session-manager
有时间并感兴趣的朋友，可以在这个基础上自行实现一个，这样更适合自己的项目。
服务器部署分布：
ha主机 192.168.1.227:80
ha备机 192.168.1.246:80
keepalived 主机 192.168.1.227
keepalived备机 192.168.1.246
web1 http://192.168.1.226:8888/login
web2 http://192.168.1.246:8888/login
redis主 192.168.1.245 6380
redis备
安装redis
主机和备机都安装redis
wget http://download.redis.io/releases/redis-2.8.5.tar.gz
解压：
tar xzf redis-2.8.5.tar.gz
cd redis-2.8.5
make
启动
src/redis-server redis.conf --port 6380 &
客户端登录
src/redis-cli -p 6380
备机配置复制：
redis.conf文件中
添加
slaveof 192.168.1.245 6380
Tomcat配置
tomcat版本：7.0.61
相关jar包：
注意这里的jar包最好是按下面的版本，否则会出现jar包冲突的问题。
tomcat-redis-session-manager-1.1.jar
commons-pool-1.6.jar
jedis-2.1.0.jar
下载tomcat redis session manager
https://github.com/jcoleman/tomcat-redis-session-manager/downloads
下载 apache common pool
http://commons.apache.org/proper/commons-pool/download_pool.cgi
下载版本：tomcat-redis-session-manager-1.1
redis的jar包可以从maven中央仓库下载
将以上3个jar包放入tomcat/lib目录中
在tomcat context.xml中加入如下内容
复制代码
<!-- host: optional: defaults to "localhost" -->
<!-- port: defaults to "6379" -->
<!-- database: optional: defaults to "0" -->
<!-- maxInactiveInterval: optional: defaults to "60" (in seconds) -->
<Valve className="com.radiadesign.catalina.session.RedisSessionHandlerValve" />
<Manager className="com.radiadesign.catalina.session.RedisSessionManager"
    host="192.168.1.245" port="6380" database="0" maxInactiveInterval="60" />
复制代码
启动tomcat，浏览器请求tomcat
http://192.168.1.226:8888/login/
  image
登录redis客户端，查看session
image
session已经被保存到redis
下面，我们进行一项测试
测试流程：
1.先访问虚拟ip1.99的应用，得到sessionId
image
web服务器是226
2.然后将对应的tomcat停掉
3.刷新该应用，若sessionId未变，则表示redis保存session成功。
image
我们发现web服务器变成了246，但是sessionId未发生变化
该方案将session集中保存在了redis服务器，并做了主备容灾，从一定程度上提高了系统的高可用，由于
redis是内存存储，访问效率较高，在性能上也是比较好的，但是本例中session不是分布式存储，因此当用户量
非常大，并发访问量非常高的时候，session服务器会成为性能瓶颈。
```

测试php性能和mysql数据库性能的工具,和找出瓶颈的方法。

正则提出一个网页中的所有链接。
介绍一下常见的SSO(单点登陆)方案(比如dedecms整合discuz的passport)的原理。
您写过的PHP框架的特点，主要解决什么问题，与其他框架的不同点。
大型的论坛/新闻文章系统/SNS网站在性能优化上有什么区别?
相册类应用:要求在浏览器中能同时选中并上传多个文件，图片要求能剪裁，压缩包在服务器端解压。能上传单个达50M的文件。上传过程中有进度条显示。每个图片能生成四种大小缩略图，视频文件要转成flv供flash播放。叙述要涉及的各类开源软件和简单用途。
一群猴子排成一圈，按1，2，…，n依次编号。然后从第1只开始数，数到第m只,把它踢出圈，从它后面再开始数，再数到第m只，在把它踢出去…，如此不停的进行下去，直到最后只剩下一只猴子为止，那只猴子就叫做大王。要求编程模拟此过程，输入m、n, 输出最后那个大王的编号。用程序模拟该过程。
3. unix/linux 基本使用

linux下查看当前系统负载信息的一些方法。（答案参考这两篇文章：Ubuntu指令大全系列一——linux指令和Ubuntu查看系统的信息命令大全）
vim的基本快捷键。（答案参考这两篇文章：Ubuntu指令大全系列二——VIM编辑器的使用和简明 Vim 练级攻略）
ssh 安全增强方法;密码方式和rsa key 方式的配置。
rpm/apt/yum/ports 装包，查询，删除的基本命令。（答案参考这篇文章：Ubuntu指令大全系列五——软件安装）
Makefile的基本格式，gcc 编译，连接的命令,-O0 和-O3区别。（答案参考这篇文章：Linux编译安装中configure、make和make install各自的作用）
gdb,strace,valgrind的基本使用。

4. 前端,HTML,JS

css盒模型。
```
CSS 盒子模型(Box Model)
所有HTML元素可以看作盒子，在CSS中，"box model"这一术语是用来设计和布局时使用。
CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。
盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
下面的图片说明了盒子模型(Box Model)：

CSS box-model
不同部分的说明：
Margin(外边距) - 清除边框外的区域，外边距是透明的。
Border(边框) - 围绕在内边距和内容外的边框。
Padding(内边距) - 清除内容周围的区域，内边距是透明的。
Content(内容) - 盒子的内容，显示文本和图像。
为了在所有浏览器中的元素的宽度和高度设置正确的话，你需要知道的盒模型是如何工作的。
元素的宽度和高度
Remark重要: 当您指定一个CSS元素的宽度和高度属性时，你只是设置内容区域的宽度和高度。要知道，完全大小的元素，你还必须添加填充，边框和边距。.
下面的例子中的元素的总宽度为300px：
实例
div {
    width: 300px;
    border: 25px solid green;
    padding: 25px;
    margin: 25px;
}

尝试一下 »
让我们自己算算：
300px (宽)
+ 50px (左 + 右填充)
+ 50px (左 + 右边框)
+ 50px (左 + 右边距)
= 450px
试想一下，你只有250像素的空间。让我们设置总宽度为250像素的元素:
实例
div {
    width: 220px;
    padding: 10px;
    border: 5px solid gray;
    margin: 0; 
}

尝试一下 »
最终元素的总宽度计算公式是这样的：
总元素的宽度=宽度+左填充+右填充+左边框+右边框+左边距+右边距
元素的总高度最终计算公式是这样的：
总元素的高度=高度+顶部填充+底部填充+上边框+下边框+上边距+下边距
浏览器的兼容性问题
一旦为页面设置了恰当的 DTD，大多数浏览器都会按照上面的图示来呈现内容。然而 IE 5 和 6 的呈现却是不正确的。根据 W3C 的规范，元素内容占据的空间是由 width 属性设置的，而内容周围的 padding 和 border 值是另外计算的。不幸的是，IE5.X 和 6 在怪异模式中使用自己的非标准模型。这些浏览器的 width 属性不是内容的宽度，而是内容、内边距和边框的宽度的总和。
虽然有方法解决这个问题。但是目前最好的解决方案是回避这个问题。也就是，不要给元素添加具有指定宽度的内边距，而是尝试将内边距或外边距添加到元素的父元素和子元素。
IE8 及更早IE版本不支持 填充的宽度和边框的宽度属性设。
解决IE8及更早版本不兼容问题可以在HTML页面声明 <!DOCTYPE html>即可。
```
javascript中的prototype。
```
用过JavaScript的同学们肯定都对prototype如雷贯耳，但是这究竟是个什么东西却让初学者莫衷一是，只知道函数都会有一个prototype属性，可以为其添加函数供实例访问，其它的就不清楚了，最近看了一些 JavaScript高级程序设计，终于揭开了其神秘面纱。

每个函数都有一个prototype属性，这个属性是指向一个对象的引用，这个对象称为原型对象，原型对象包含函数实例共享的方法和属性，也就是说将函数用作构造函数调用（使用new操作符调用）的时候，新创建的对象会从原型对象上继承属性和方法。

私有变量、函数

在具体说prototype前说几个相关的东东，可以更好的理解prototype的设计意图。之前写的一篇JavaScript 命名空间博客提到过JavaScript的函数作用域，在函数内定义的变量和函数如果不对外提供接口，那么外部将无法访问到，也就是变为私有变量和私有函数。

复制代码
function Obj(){
                var a=0; //私有变量
                var fn=function(){ //私有函数
                    
                }
            }
复制代码
这样在函数对象Obj外部无法访问变量a和函数fn，它们就变成私有的，只能在Obj内部使用，即使是函数Obj的实例仍然无法访问这些变量和函数

var o=new Obj();
            console.log(o.a); //undefined
            console.log(o.fn); //undefined
静态变量、函数

当定义一个函数后通过 “.”为其添加的属性和函数，通过对象本身仍然可以访问得到，但是其实例却访问不到，这样的变量和函数分别被称为静态变量和静态函数，用过Java、C#的同学很好理解静态的含义。

复制代码
function Obj(){
                
            }
            
            Obj.a=0; //静态变量
            
            Obj.fn=function(){ //静态函数
                    
            }
            
            console.log(Obj.a); //0
            console.log(typeof Obj.fn); //function
            
            var o=new Obj();
            console.log(o.a); //undefined
            console.log(typeof o.fn); //undefined
复制代码
实例变量、函数

在面向对象编程中除了一些库函数我们还是希望在对象定义的时候同时定义一些属性和方法，实例化后可以访问，JavaScript也能做到这样

复制代码
function Obj(){
                this.a=[]; //实例变量
                this.fn=function(){ //实例方法
                    
                }
            }
            
            console.log(typeof Obj.a); //undefined
            console.log(typeof Obj.fn); //undefined
            
            var o=new Obj();
            console.log(typeof o.a); //object
            console.log(typeof o.fn); //function
复制代码
这样可以达到上述目的，然而

复制代码
function Obj(){
                this.a=[]; //实例变量
                this.fn=function(){ //实例方法
                    
                }
            }
            
            var o1=new Obj();
            o1.a.push(1);
            o1.fn={};
            console.log(o1.a); //[1]
            console.log(typeof o1.fn); //object
            var o2=new Obj();
            console.log(o2.a); //[]
            console.log(typeof o2.fn); //function
复制代码
上面的代码运行结果完全符合预期，但同时也说明一个问题，在o1中修改了a和fn，而在o2中没有改变，由于数组和函数都是对象，是引用类型，这就说明o1中的属性和方法与o2中的属性与方法虽然同名但却不是一个引用，而是对Obj对象定义的属性和方法的一个复制。

这个对属性来说没有什么问题，但是对于方法来说问题就很大了，因为方法都是在做完全一样的功能，但是却又两份复制，如果一个函数对象有上千和实例方法，那么它的每个实例都要保持一份上千个方法的复制，这显然是不科学的，这可肿么办呢，prototype应运而生。

prototype

无论什么时候，只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个prototype属性，默认情况下prototype属性会默认获得一个constructor(构造函数)属性，这个属性是一个指向prototype属性所在函数的指针，有些绕了啊，写代码、上图！

function Person(){
                
            }
image

根据上图可以看出Person对象会自动获得prototyp属性，而prototype也是一个对象，会自动获得一个constructor属性，该属性正是指向Person对象。

当调用构造函数创建一个实例的时候，实例内部将包含一个内部指针（很多浏览器这个指针名字为__proto__）指向构造函数的prototype，这个连接存在于实例和构造函数的prototype之间，而不是实例与构造函数之间。

复制代码
function Person(name){
                this.name=name;
            }
            
            Person.prototype.printName=function(){
                alert(this.name);
            }
            
            var person1=new Person('Byron');
            var person2=new Person('Frank');
复制代码
 

image

Person的实例person1中包含了name属性，同时自动生成一个__proto__属性，该属性指向Person的prototype，可以访问到prototype内定义的printName方法，大概就是这个样子的

image

写段程序测试一下看看prototype内属性、方法是能够共享

复制代码
function Person(name){
                this.name=name;
            }
            
            Person.prototype.share=[];
            
            Person.prototype.printName=function(){
                alert(this.name);
            }
            
            var person1=new Person('Byron');
            var person2=new Person('Frank');
            
            person1.share.push(1);
            person2.share.push(2);
            console.log(person2.share); //[1,2]
复制代码
果不其然！实际上当代码读取某个对象的某个属性的时候，都会执行一遍搜索，目标是具有给定名字的属性，搜索首先从对象实例开始，如果在实例中找到该属性则返回，如果没有则查找prototype，如果还是没有找到则继续递归prototype的prototype对象，直到找到为止，如果递归到object仍然没有则返回错误。同样道理如果在实例中定义如prototype同名的属性或函数，则会覆盖prototype的属性或函数。

复制代码
function Person(name){
                this.name=name;
            }
            
            Person.prototype.share=[];

            var person=new Person('Byron');
            person.share=0;
            
            console.log(person.share); //0而不是prototype中的[]
复制代码
构造简单对象

当然prototype不是专门为解决上面问题而定义的，但是却解决了上面问题。了解了这些知识就可以构建一个科学些的、复用率高的对象，如果希望实例对象的属性或函数则定义到prototype中，如果希望每个实例单独拥有的属性或方法则定义到this中，可以通过构造函数传递实例化参数。

复制代码
function Person(name){
                this.name=name;
            }
            
            Person.prototype.share=[];
            
            Person.prototype.printName=function(){
                alert(this.name);
            }

```
javascript中this对象的作用域。
```
大家在使用Javascript的时候经常被this这个家伙搞得晕头转向的。对大多数有OOP开发经验的开发人员来说this是当前作用域中引用普通元素的标识符，但是在Javascript中它却显得古灵精怪的，因为它不是固定不变的，而是随着它的执行环境的改变而改变。在Javascript中this总是指向调用它所在方法的对象。
举一个简单的例子：
复制代码 代码如下:

function test(){
alert(this);
}
var obj=function(){
var name='testObj';
}
obj.objTest=test;
test();
obj.objTest();
把这段代码放到HTML中运行这个页面，你会看到首先提示一个警告[object window]，然后第二个警告。
复制代码 代码如下:

var obj=function(){
var name='testObj';
}
我们先定义了一个test()方法，并在方法内部调用alert()方法将this显示出来，然后定义了一个obj函数对象，并给它加了一个私有的字段name，同时给它加了一个静态的方法objTest()，而这个函数则直接指向test()函数。
分别调用test()和obj.objTest()方法，第一次警告框提示的是Window对象，而第二次提示的是我们定义的obj这个函数的代码。这说明了test函数在两次执行的时候this的值是不同的！
这就说明了当调用函数的对象不同的时候，其内部的this关键字指代的对象是不同的。这里需要值得注意的是Javascript是基于对象的语言，当我们的变量或者函数定义在<script></script>标签的根下的时候其实相当于给window对象加了相应的属性或方法，所以当我们利用function test(){}代码定义一个函数的时候，其实相当于给window对象添加了一个新的函数，即window.test()函数。
我们可以做一个实验：
复制代码 代码如下:

function test(){
alert(this);
}
alert(test===window.test);
警告框提示的将是true，这说明当我们在调用test()这个函数时相当于调用的是window.test()。所以当我们调用test()函数的时候调用这个函数的对象其实是window对象，this指代的是window对象，所以我们在alert(this)的时候弹出的警告窗口内容是[object Window]。我们将obj.objTest=test相当于把obj.objTest()指向test(),所以当我们调用obj.objTest()函数时相当于在obj调用了test()这个函数，所以现在this指代的是obj对象，提示的就是obj这个Function也就是我们看到的代码。
说到这应该也解释的差不多了，可能上面的例子太抽象，想象不出来它能在什么情况下用到，那我们现在就假设一个需求，做一个贴近实用一点的例子。
假设我们现在页面中的所有超链接在点击之后颜色要改为红色，用Javascript实现。大体的思路应该是获取页面中所有的<a>标签，然后遍历所有的<a>标签，给每一个注册一个click事件，事件触发后我们将它的color值设为red。
示例代码如下：
复制代码 代码如下:

//改变颜色
function changeColor(){
this.style.color='#f00';
}
//初始化，给所有 a 标签注册事件
function init(){
var customLinks=document.getElementsByTagName('a');
for(i in customLinks){
//你也可以使用事件侦听器方式来注册事件
//由于要兼容IE，FF等浏览器可能需要更多代码，您可以自行编写
customLinks[i].onclick=changeColor;
}
}
window.onload=init;
将这段代码添加到HTML文档中，并在文档中添加一些超链接，当超链接点击后颜色会变成红色，这里我们定义的changeColor()函数中this关键字在点击超链接触发函数的时候它指代的是当前这个超链接。而如果你直接调用changeColor()函数浏览器会报错，提示Error: ‘this.style' is null or not an object或者undefined之类的错误。
不知道说到这能不能让正在看文章的你对Javascript中的this关键字有了一些自己的了解呢？或者你已经不耐烦了？（:P）
其实要想真正对这个问题有更深入的理解那么必须对Javascript的作用域和作用域链有深入的理解。
作用域，顾名思义就是指某一属性或方法具有访问权限的代码空间，简单的说也就是这个变量或方法它在代码中的的适用范围。在大多数的OOP中主要有public，private，protect三种作用域，对着三种作用域在这里就不详细解释了，如果有OOP的经验应该都有深入的了解。在这里我要说的是这三种作用域类型对Javascript来说几乎是毫无意义的，因为Javascript中只有一种公共作用域，在Javascript中作用域是在函数中进行维护的。举个例子：
复制代码 代码如下:

var test1='globle variable';
function example(){
var test2='example variable';
alert(test1);
alert(test2);
}
example();
alert(test1);
alert(test2);
根据我们前面解释的，这里的test1变量相当于window的一个属性，所以它会在整个window作用域内起作用，而test2则在example()函数的内部声明，所以它的作用域也就维持在example()方法的内部，如果在函数的外部调用test2浏览器会提示出错。而在example()内部调用test1则没问题。
根据这个我们再举一个例子：
复制代码 代码如下:

var test='globle variable';
function example(){
var test='example variable';
}
example();
alert(test);
这个例子运行会是什么结果呢？对，警告框会提示“globle variable”，因为example()函数内部的test变量其作用域只维持在内部，不会影响外部的test变量。如果我们将example()内部test变量的var关键字去掉呢？你可以自己试试。
说到这就有牵扯出另外一个概念，那就是作用域链的概念。作用域链就是可以确定变量值的路径。由上面一个例子可以看出，var关键字是用来维护作用域链的，如果变量使用了var关键字声明那么他就可以看作为作用域链的终点。同样函数的形参的定义也会起到类似的作用。
说到这你对this这个精灵古怪的家伙有了比较清晰的认识了吧？根据它简单的一个诠释，this总是指向调用它所在函数的对象，根据作用域和作用域链，我们会很清晰的确定this的真面目。临末尾再来一个开始那个例子的简单变化：
复制代码 代码如下:

function test(){
alert(this);
}
var obj=function(){
var name='testObj';
}
obj.objTest=test;
obj.objTest2=function(){
test();
}
test();
obj.objTest();
obj.objTest2();
你猜会提示什么内容呢？你可以运行一下试试(:P);
既然this是根据调用其所在函数的对象的改变而改变的，那我们可不可以强制改变它的调用对象呢？答案是肯定的，以后的文章会介绍一下这部分内容，以及Javascript中不同类型的数据成员的实现方式，闭包等概念。
```
IE和firefox事件冒泡的不同。
什么是怪异模式,标准模式，近标准模式。
DTD的定义。
IE/firefox常用hack。
firefox,IE下的前端js/css调试工具。