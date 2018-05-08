# 第一章 PHP入门
#### 1.PHP所需环境

```
安装 wampServer 
安装包以及安装步骤：链接：https://pan.baidu.com/s/17M0QNNmRyzFN9NWRSW41IA 密码：sxvm
```
#### 2.PHP标记

```
以 <?php  开始 ，以  ?>  结束  后缀名 .php
```
#### 3.PHP注释 和 空格

```
//   单行注释   
/* */  多行注释
空格：间隔字符（换行（回车）、空格、制表符（Tab））
```

#### 4.PHP语句

```
echo : 可以输出一个或多个字符，用逗号分隔，结束处必须有分号,可以使用点（.）操作符链接
	例：
	<?php
		$x=1; //定义变量
		$y=1;
		$z=$x+$y;
		echo $x,$y,$z;
		echo $x . '+' . $y . '=' . $z;  // 1+1=2
	?>
print：只允许输出一个字符串，返回值总为1，可以使用点（.）操作符链接
	例：
	<?php
		$x=1;
		$y=1;
		$z=$x+$y;
		// print $x,$y,$z; //报错 只能单个输出
		print $x . '+' . $y . '=' . $z;  // 1+1=2
	?>
heredoc语法：<<<thenEnd(是一种在命令shell和程序语言定义一个字符串的方式)
	* 结束处必须后接分号，否则编译不通过
	* thenEnd 可以用任意其它字符代替，只需保证结束标识与开始标识一致
	* 结束标识必须顶格独占一行（即必须从行首开始，前后不能衔接任何空白和字符）
	* 当内容需要内嵌引号时不需要加转译字符
	例：
	<?php
		echo <<<thenEnd
			</br>line 1
	        </br>line ‘2’
	        </br>line 3
	thenEnd;
	?>
```

#### 5.PHP标识符命名规则

```
 * 标识符可以是任何长度，而且可以由任何字母、数字、下划线组成
 * 标识符不能以数字开始
 * 在PHP中，标识符是区分大小写的。（函数名不区分大小写）
 * 变量名称可以与函数名称相同。这一点容易造成混淆，虽然是允许的，但应该尽量避免使用。此外不能创建一个与已有函数同名的函数
```
#### 5.PHP调用date()函数

```
初步了解，详解后续....
	调用格式：date('Y/m/d  h:i:sa  , jS F Y')   // 2018/05/08 03:27:35pm, 8th May 2018
		参数详解：
			Y/m/d - 年/月/日 （Y.m.d   Y-m-d）
			h - 带有首位零的 12 小时小时格式  H - 24小时格式
            i - 带有首位零的分钟
            s - 带有首位零的秒（00 -59）
            a - 小写的午前和午后（am 或 pm）
            j - 该月的日期，不需要在前面补零，而s表示顺序后缀(th)
            F - 月份的全称
            Y - 年
    例：
    <?php
        // date_default_timezone_set('PRC');
        echo "<p>order processed at ";
        echo date('H:i, jS F Y');
        echo "</p>";
        echo "<h2>点链接符</h2>";
        echo 'hello '.'world';
        echo "今天是 " . date("Y/m/d") . "<br>"; //2018/05/08
        echo "今天是 " . date("Y.m.d") . "<br>"; //2018.05.08
        echo "今天是 " . date("Y-m-d") . "<br>"; //2018-05-08
        echo "今天是 " . date("l") . "<br>"; // 星期二 Tuesday 
        echo "现在时间是 " . date("h:i:sa , jS") . "<br>";
    ?>
    bug：获取系统时间不正确(php默认时区为UTF)
	    方法一：修改php.ini文件
		    wampServer单击 - PHP -php.ini
		    查找date.timezone 时区，修改为date.timezone = PRC  北京时区
		方法二：懒人法 使用date_default_timezone_set() 方法设置
			date_default_timezone_set('PRC');
```
#### 6.PHP访问表单变量

```
  在PHP脚本中，你可以访问每一表单域，因为每个表单域都有一个PHP变量通过名称与其关联。你可以很容易的识别PHP的变量名称，因为它们都是以$符号开始的（漏掉这个$符是一个常见的变成错误）
  可以通过$_POST[''] 访问post方式的变量域  $_GET[''] get 方式   $_REQUEST['']  get 和post 方式
  bug: php中文乱码 （设置头信息）
	  header("Content-type: text/html; charset=utf-8"); 
```
form.html  代码

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <form action="form.php" method="POST">
        <p>姓名：<input type="text" name="trueName" value=""></p>
        <p>年龄：<input type="number" name="age"></p>
        <p>电话：<input type="tel" name="phone" value=""></p>
        <p><input type="submit" value="提交信息"></p>
    </form>
</body>
</html>
```

form.php 代码

```
<?php
    header("Content-type: text/html; charset=utf-8"); 
    $trueName=$_REQUEST['trueName'];
    $age=$_REQUEST['age'];
    $phone=$_REQUEST['phone'];
    echo '姓名：' . $trueName . '<br/> 年龄：' . $age . '<br/> 电话：' . $phone . '<br/>';
    echo '姓名：' . htmlspecialchars($trueName) . '<br/>年龄：' . htmlspecialchars($age) . '<br/>电话：' . htmlspecialchars($phone);  //推荐使用 htmlspecialchars
    // 单引号和双引号的区别   单引号将里面内容不做修改发送给浏览器   而双引号可以解析里面的变量
    // 变量和字面量   双引号称为计算   单引号称为字面量
    // 字符串类型：具有双引号、具有单引号、heredoc语法（<<<）
?>    
```