```
一、PHP核心技术
1.写出一个能创建多级目录的PHP函数（新浪网技术部）
2.写出smarty模板的特点（新浪网技术部）
3.打开php.ini中的safe_mode，会影响哪些函数？至少说出6个。（新浪）
4.抓取远程图片到本地,你会用什么函数?(51.com笔试题)
5.PHP的垃圾收集机制是怎样的（腾讯）
6.请写一段PHP代码，确保多个进程同时写入同一个文件成功（腾讯）
7.写一个函数，尽可能高效的，从一个标准url里取出文件的扩展名，例如:http://www.sina.com.cn/abc/de/fg.php?id=1需要取出php或.php（新浪）
8.写一个函数，能够遍历一个文件夹下的所有文件和子文件夹。（新浪）
9.简述论坛中无限分类的实现原理。（新浪）
10.写一个函数，算出两个文件的相对路径，如a='/a/b/c/d/e.php';b='/a/b/12/34/c.php';计算出b相对于a的相对路径应该是../../c/d（新浪）
11.mysql_fetch_row()和mysql_fetch_array()有什么分别？
12.有一个网页地址,比如PHP开发资源网主页:http://www.phpres.com/index.html,如何得到它的内容?
13.谈谈对mvc的认识
14.What does the GD library do?（Yahoo）
15.What function can you use to open a file for reading and writing?（Yahoo）
16.Smarty的原理
17.PHP如何实现页面跳转
18.PHP可以和sql server/oracle等数据库连接吗?
19.使用哪些工具进行版本控制？
20.写出一个正则表达式，过虑网页上的所有JS/VBS脚本（即把script标记及其内容都去掉）：
21.Given a line of text $string,how would you write a regular expression to strip all the HTML tags from it?（Yahoo）
22.请写一个函数验证电子邮件的格式是否正确（要求使用正则）（新浪）
相关题目：请用正则表达式写一个函数，验证电子邮件的格式是否正确。（鑫众人云）
23.请对POSIX风格和兼容Perl风格两种正则表达式的主要函数进行类比说明（腾讯）
24.请写出并说明如何在命令行下运行PHP脚本（写出两种方式）同时向PHP脚本传递参数？（腾讯）
25.使用正则表达式提取一段标识语言（html或xml）代码段中指定标签的指定属性值（需考虑属性值对不规则的情况，如大小写不敏感，属性名值与等号间有空格等）。此处假设需提取test标签的attr属性值，请自行构建包含该标签的串（腾讯）
26.What does the following code do?Explain what's going on there.date='08/26/2003';print ereg_replace(“([0-9]+)/([0-9]+)/([0-9]+)”,\\2/\\1/\\3,date);（Yahoo）
这是把一个日期从MM/DD/YYYY的格式转为DD/MM/YYYY格式。输出26/08/2003
27.What function would you use to redirect the browser to a new page?（Yahoo）
A.redir()B.header()C.location()D.redirect()答案：Bredir()这不是一个PHP函数，会引致执行错误。header()这个是正确答案，header()函数发送头信息，可以用来使浏览器转向到另一个页面，例如：header("Location:http://www.search-this.com/")。location()这不是一个PHP函数，会引致执行错误。redirect()这不是一个PHP函数，会引致执行错误。
28.When turned on____________will_________your script with different variables fromHTML forms and cookies.（腾讯）A.show_errors,enableB.show_errors,showC.register_globals,enhanceD.register_globals,inject答案：C
29.一个函数的参数不能是对变量的引用，除非在php.ini中把____设为on。allow_call_time_pass_reference是否启用在函数调用时强制参数被按照引用传递
30.在HTML语言中，页面头部的meta标记可以用来输出文件的编码格式，以下是一个标准的meta语句<META http-equiv='Content-Type'content='text/html;charset=gbk'>，请使用PHP语言写一个函数，把一个标准HTML页面的类似meta标记中的charset部分值改为big5。（新浪）
31.PHP中如何判断一个字符串是否是合法的日期模式：2007-03-13 13:13:13。要求代码不超过5行。（酷讯）
32.PHP中，如何获得一个数组的键值？（酷讯）
33.如果模板是用smarty模板。怎样用section语句来显示一个名为$data的组。比如：
34.哪个选项会匹配下边的这个正则表达式?(/.*xyz\d/)
A.*****xyz   B.*****xyz1  C.******xyz2  D.*xyz
35.以下哪个错误无法被标准的错误控制器获取？
A.E_WARNING B.E_USER_ERROR C.E_PARSE D.E_NOTICE
36.以下哪种错误类型无法被自定义的错误处理器捕捉到?（奇矩互动）
A.E_WARNING B.E_USER_ERROR C.E_PARSE D.E_NOTICE
37.(^\s)|(\s$)这个正则表达式作用是：__________；
38.编写函数取得上一月和本月的最后一天
39.在很多时候，我们可以通过apache的主配置文件来设置对test目录的访问权限控制，如http://IP/test请问如果需设置test下的一个子目录的访问控制权限，是否可以在主配置文件中修改，如果不可以应如何解决。
40.如果我的网站用的utf-8编码，为防止乱码出现，都需要注意哪些地方？
41.在url中用get传值的时候，若中文出现乱码，应该用哪个函数对中文进行编码？
42.写出两种对变量加密的函数？
43.如何把2009-9-2 10:30:25变成unix时间戳？
44.如何把一个GB2312格式的字符串装换成UTF-8格式？
45.如果需要原样输出用户输入的内容，在数据入库前，要用哪个函数处理？
46.写出五种以上你使用过的PHP的扩展的名称（提示：常用的PHP扩展）
47.了解MVC模式吗？请写出三种以上目前PHP流行的MVC框架名称（不区分大小写）
48.php中WEB上传文件的原理是什么，如何限制上传文件的大小？
49.简述UBB code的实现原理。(YG)
50.怎么把文件保存到指定目录？怎么避免上传文件重名问题？
51._____函数能返回脚本里的任意行中调用的函数的名称。该函数同时还经常被用在调试中，用来判断错误是如何发生的。（奇矩互动）
52.在Smarty模板语法中怎么能遍历数组a=array(array('id'=>0),array('id'=>1),array('id'=>2),……)id=>0-10000，怎么判断当id=>500,让输出的值在HTML里变为红色？（亿邮） 设数组分配过来的数组名为ids
53.在Smarty模板语法中如何获取当前时间，并且使用Y-m-d H:i:s的格式输出？（亿邮）
54.在Smarty模板语法中如何获取php的全局环境变量（亿邮）
55.在Smarty模板中如何用自定义函数（亿邮）
56.列举出你所知道的php系统函数库例如，数学函数库(亿邮)
57.假如让你来写一个函数实现Utf-8转gb2312，那么函数的名称应该怎么命名？(亿邮)
58.请描述如下URL重写规则的用意。（卓望）
<IfModulemod_rewrite.c>
RewriteEngineon
RewriteCond%{REQUEST_FILENAME}!-f
RewriteCond%{REQUEST_FILENAME}!-d
RewriteBase/
RewriteRule./index.php[L]
</IfModule>
59.Warning:Cannot modify header information-headers already sent by(output started at D:\src\init.php:7)in D:\src\init.php on line10通常什么情况下php会报该警告信息？(卓望)
60.写出你所知道的字符串和数组相关的php原生函数，并说明
61.写出你熟悉的shell命令
```