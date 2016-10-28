## cksum 
指定文件交由cksum进行演算，它会回报计算结果，供用户核对文件是否正确无误（被修改过）
magapp14a:/shared/opt/pos/mxg/mxg_irdfx_dev9$ cksum mx    // 若不指定任何文件名，则cksum指令会从标准输入设备读取数据
2444285914      88118112        mx


## ## awk 连续输出几列 ;比如我有个文件 想输出10 到20列。
awk '{for(i=10;i<=33;i++)printf $i";"FS;print""}'  file


awk：用于一行中分成数个“字段”来处理。适合处理 	小型数据。
运行模式：awk '条件类型1{动作1} 条件类型2{动作2} ...' filename

# last | awk '{print $1 "\t" $3}' <== 查看登录者的数据，只显示登录名和ip地址，并以[tab]隔开

awk 的内置变量
变量名称	代表的含义

NF	每一行（$0）拥有的字段总数

NR	当前 awk 所处理的是 “第几行” 数据

FS	当前分隔符，默认空格键

例:显示文本文件myfile中第七行到第十五行中以字符%分隔的第一字段，第三字段和第七字段： 

awk -F % 'NR==7,NR==15 {printf $1 $3 $7}' 

awk 的逻辑运算符
运算单元	代表含义
>	大于
<	小于
>=	大于或等于
<=	小于或等于
==	等于
!=	不等于
范例：
cat /etc/passwd | awk '{FS=":"} $3 < 10 {print $1 "\t" $3}' <== 文件/etc/passwd是以":"分隔的，查看第三栏小于10的数据，并且只显示帐号与第三栏

以上是我对awk的总结，希望对你有帮助，是我写的哦，不是复制的。

## awk 'BEGIN {FS="|"} {f4=$4;f_4=f4+128;print $1"|"$2"|"substr($3,1,12)"|"$f_4"|"$5"|"$6"|"$7"|"$8"|"$9"|"$10"|"$11"|"$12"|"$13"|"$14 > "fxcrdiv_var_fxsp.csv.aud.v1"}' fxcrdiv_var_fxsp.csv.aud
awk 'BEGIN {FS="|"} {f4=$4;f_4=f4+128;print $1"|"$2"|"substr($3,1,12)"|"f_4"|"$5"|"$6"|"$7"|"$8"|"$9"|"$10"|"$11"|"$12"|"$13"|"$14 > "fxcrdiv_var_fxsp.csv.aud.v1"}' fxcrdiv_var_fxsp.csv.aud

awk的语法： 
=============

与其它UNIX命令一样，awk拥有自己的语法： 

awk [ -F re] [parameter...] ['prog'] [-f progfile][in_file...] 

参数说明： 

-F re:允许awk更改其字段分隔符。 

parameter: 该参数帮助为不同的变量赋值。 

'prog': awk的程序语句段。这个语句段必须用单拓号('')括起来，以防被shell解释。这个程序语句段的标准形式为： 

'pattern {action}' 其中pattern参数可以是egrep正则表达式中的任何一个，它可以使用语法/re/再加上一些样式匹配技巧构成。
与sed类似，你也可以使用","分开两样式以选择某个范围。关于匹配的细节，你可以参考附录，如果仍不懂的话，
找本UNIX书学学grep和sed（本人是在学习sed时掌握匹配技术的）。
 action参数总是被大括号包围，它由一系统awk语句组成，各语句之间用";"分隔。awk解释它们，并在pattern给定的样式匹配的记录上执行其操作。与shell类似，你也可以使用“#”作为注释符，它使“#”到行尾的内容成为注释，在解释执行时，它们将被忽略。你可以省略pattern和action之一，但不能两者同时省略，当省略pattern时没有样式匹配，表示对所有行（记录）均执行操作，省略action时执行缺省的操作――在标准输出上显示。 

-f progfile:允许awk调用并执行progfile指定有程序文件。progfile是一个文本文件，他必须符合awk的语法。 

in_file:awk的输入文件，awk允许对多个输入文件进行处理。值得注意的是awk不修改输入文件。如果未指定输入文件，awk将接受标准输入，并将结果显示在标准输出上。awk支持输入输出重定向。 

awk的记录、字段与内置变量：
===========================
awk的记录、字段与内置变量： 

前面说过，awk处理的工作与数据库的处理方式有相同之处，其相同处之一就是awk支持对记录和字段的处理，其中对字段的处理是grep和sed不能实现的，这也是awk优于二者的原因之一。
在awk中，缺省的情况下总是将文本文件中的一行视为一个记录，而将一行中的某一部分作为记录中的一个字段。为了操作这些不同的字段，awk借用shell的方法，
用$1,$2,$3...这样的方式来顺序地表示行（记录）中的不同字段。特殊地，awk用$0表示整个行（记录）。不同的字段之间是用称作分隔符的字符分隔开的。
系统默认的分隔符是空格。awk允许在命令行中用-F re的形式来改变这个分隔符。事实上，awk用一个内置的变量FS来记忆这个分隔符。
awk中有好几个这样的内置变量，例如，记录分隔符变量FS、当前工作的记录数NR等等，本文后面的附表列出了全部的内置变量。这些内置的变量可以在awk程序中引用或修改，例如，你可以利用NR变量在模式匹配中指定工作范围，也可以通过修改记录分隔符RS让一个特殊字符而不是换行符作为记录的分隔符。 

例:显示文本文件myfile中第七行到第十五行中以字符%分隔的第一字段，第三字段和第七字段： 
---------------
awk -F % 'NR==7,NR==15 {printf $1 $3 $7}' 

awk的内置函数 
=============

awk之所以成为一种优秀的程序设计语言的原因之一是它吸收了某些优秀的程序设计语言（例如C）语言的许多优点。这些优点之一就是内置函数的使用，
awk定义并支持了一系列的内置函数，由于这些函数的使用，使得awk提供的功能更为完善和强大，
例如，awk使用了一系列的字符串处理内置函数(这些函数看起来与C语言的字符串处理函数相似，其使用方式与C语言中的函数也相差无几)，
正是由于这些内置函数的使用，使awk处理字符串的功能更加强大。本文后面的附录中列有一般的awk所提供的内置函数，这些内置函数也许与你的awk版本有些出入，
因此，在使用之前，最好参考一下你的系统中的联机帮助。 作为内置函数的一个例子，我们将在这里介绍awk的printf函数，
这个函数使得awk与c语言的输出相一致。实际上，awk中有许多引用形式都是从C语言借用过来的。如果你熟悉C语言，
你也许会记得其中的printf函数，它提供的强大格式输出功能曾经带我们许多的方便。幸运的是，我们在awk中又和它重逢了。
awk中printf几乎与C语言中一模一样，如果你熟悉C语言的话，你完全可以照C语言的模式使用awk中的printf。
因此在这里，我们只给出一个例子，如果你不熟悉的话，请随便找一本C语言的入门书翻翻。 

例:显示文件myfile中的行号和第3字段： 
---------------------
$awk '{printf"%03d%s",NR,$1}' myfile 
在命令行使用awk 

按照顺序，我们应当讲解awk程序设计的内容了，但在讲解之前，我们将用一些例子来对前面的知识进行回顾，
这些例子都是在命令行中使用的，由此我们可以知道在命令行中使用awk是多么的方便。这样做的原因一方面是为下面的内容作铺垫，
另一方面是介绍一些解决简单问题的方法，我们完全没有必要用复杂的方法来解决简单的问题----既然awk提供了较为简单的方法的话。

例：显示文本文件mydoc匹配（含有）字符串"sun"的所有行。 
---------------------
$awk '/sun/{print}' mydoc 

由于显示整个记录（全行）是awk的缺省动作，因此可以省略action项。 

$awk '/sun/' mydoc 

例：下面是一个较为复杂的匹配的示例： 
---------------------
$awk '/[Ss]un/,/[Mm]oon/ {print}' myfile 

它将显示第一个匹配Sun或sun的行与第一个匹配Moon或moon的行之间的行，并显示到标准输出上。 

例：下面的示例显示了内置变量和内置函数length（）的使用： 
---------------------
$awk 'length($0)>80 {print NR}' myfile 

该命令行将显示文本myfile中所有超过80个字符的行号，在这里，用$0表示整个记录（行），同时，内置变量NR不使用标志符'$'。


例：作为一个较为实际的例子，我们假设要对UNIX中的用户进行安全性检查，方法是考察/etc下的passwd文件，
检查其中的passwd字段（第二字段）是否为"*"，如不为"*"，则表示该用户没有设置密码，显示出这些用户名（第一字段）。
我们可以用如下语句实现：
--------------
#awk -F: '$2=="" {printf("%s no password!",$1'} /etc/passwd 
在这个示例中，passwd文件的字段分隔符是“：”，因此，必须用-F：来更改默认的字段分隔符，这个示例中也涉及到了内置函数printf的使用。 

awk的变量 
=============
如同其它程序设计语言一样，awk允许在程序语言中设置变量，事实上，提供变量的功能是程序设计语言的其本要求，
不提供变量的程序设计语言本人还从未见过。 awk提供两种变量，一种是awk内置的变量，这前面我们已经讲过，需要着重指出的是，
与后面提到的其它变量不同的是，在awk程序中引用内置变量不需要使用标志符"$"（回忆一下前面讲过的NR的使用）。
awk提供的另一种变量是自定义变量。awk允许用户在awk程序语句中定义并调用自已的变量。
当然这种变量不能与内置变量及其它awk保留字相同，在awk中引用自定义变量必须在它前面加上标志符"$"。
与C语言不同的是，awk中不需要对变量进行初始化，awk根据其在awk中第一次出现的形式和上下文确定其具体的数据类型。
当变量类型不确定时，awk默认其为字符串类型。这里有一个技巧：如果你要让你的awk程序知道你所使用的变量的明确类型，
你应当在在程序中给它赋初值。在后面的实例中，我们将用到这一技巧。

运算与判断： 
============
作为一种程序设计语言所应具有的特点之一，awk支持多种运算，这些运算与C语言提供的几本相同：如+、-、*、/、%等等，
同时，awk也支持C语言中类似++、--、+=、-=、=+、=-之类的功能，这给熟悉C语言的使用者编写awk程序带来了极大的方便。
作为对运算功能的一种扩展，awk还提供了一系列内置的运算函数（如log、sqr、cos、sin等等）和一些用于对字符串进行
操作（运算）的函数（如length、substr等等）。这些函数的引用大大的提高了awk的运算功能。作为对条件转移指令的一部分，
关系判断是每种程序设计语言都具备的功能，awk也不例外。awk中允许进行多种测试，如常用的
==（等于）、！=（不等于）、>（大于）、<（小于）、>=（大于等于）、>=（小于等于）等等，同时，作为样式匹配，
还提供了~（匹配于）和！~（不匹配于）判断。 作为对测试的一种扩充，awk也支持用逻辑运算符:!(非)、&&（与）、||（或）和括号（）
进行多重判断，这大大增强了awk的功能。本文的附录中列出了awk所允许的运算、判断以及操作符的优先级。 

awk的流程控制 
==============
流程控制语句是任何程序设计语言都不能缺少的部分。任何好的语言都有一些执行流程控制的语句。
awk提供的完备的流程控制语句类似于C语言，这给我们编程带来了极大的方便。
1、BEGIN和END: 
在awk中两个特别的表达式，BEGIN和END，这两者都可用于pattern中（参考前面的awk语法），提供BEGIN和END的作用是给程序赋予
初始状态和在程序结束之后执行一些扫尾的工作。任何在BEGIN之后列出的操作（在{}内）将在awk开始扫描输入之前执行，
而END之后列出的操作将在扫描完全部的输入之后执行。因此，通常使用BEGIN来显示变量和预置（初始化）变量，使用END来输出最终结果。  

$awk 
>'BEGIN { FS=":";print "统计销售金额";total=0} 
>{print $3;total=total+$3;} 
>END {printf "销售金额总计：%.2f",total}' sx 
（注：>是shell提供的第二提示符，如要在shell程序awk语句和awk语言中换行，则需在行尾加反斜杠） 
在这里，BEGIN预置了内部变量FS（字段分隔符）和自定义变量total,同时在扫描之前显示出输出行头。而END则在扫描完成后打印出总合计。 
2、流程控制语句 
awk提供了完备的流程控制语句，其用法与C语言类似。下面我们一一加以说明： 

2.1、if...else语句: 

格式： 
if(表达式) 
语句1 
else 
语句2 

格式中"语句1"可以是多个语句，如果你为了方便awk判断也方便你自已阅读，你最好将多个语句用{}括起来。awk分枝结构允许嵌套，其格式为： 

if(表达式1） 
{if(表达式2） 
语句1 
else 
语句2 
} 
语句3 
else {if(表达式3) 
语句4 
else 
语句5 
} 
语句6 

当然实际操作过程中你可能不会用到如此复杂的分枝结构，这里只是为了给出其样式罢了。 

2.2、while语句 

格式为: 

while(表达式) 
语句 

2.3、do-while语句 

格式为: 

do 
{ 
语句 
}while(条件判断语句） 

2.4、for语句 

格式为： 

for(初始表达式;终止条件;步长表达式) 
{语句} 

在awk的 while、do-while和for语句中允许使用break,continue语句来控制流程走向，也允许使用exit这样的语句来退出。break中断当前正在执行的循环并跳到循环外执行下一条语句。continue从当前位置跳到循环开始处执行。对于exit的执行有两种情况：当exit语句不在END中时，任何操作中的exit命令表现得如同到了文件尾，所有模式或操作执行将停止，END模式中的操作被执行。而出现在END中的exit将导致程序终止。 



awk中的自定义函数 
=======================

定义和调用用户自己的函数是几乎每个高级语言都具有的功能，awk也不例外，但原始的awk并不提供函数功能，只有在nawk或较新的awk版本
中才可以增加函数。 函数的使用包含两部分：函数的定义与函数调用。其中函数定义又包括要执行的代码（函数本身）和从主程序代码传递到该函数的临时调用。 

awk函数的定义方法如下： 

function 函数名(参数表){ 
函数体 
} 

在gawk中允许将function省略为func，但其它版本的awk不允许。函数名必须是一个合法的标志符，参数表中可以不提供参数
（但在调用函数时函数名后的一对括号仍然是不可缺少的），也可以提供一个或多个参数。与C语言相似，awk的参数也是通过值来传递的。

在awk中调用函数比较简单，其方法与C语言相似，但awk比C语言更为灵活，它不执行参数有效性检查。换句话说，在你调用函数时，可以列出比函数预计（函数定义中规定）的多或少的参数，多余的参数会被awk所忽略，而不足的参数，awk将它们置为缺省值0或空字符串，具体置为何值，将取决于参数的使用方式。 

awk函数有两种返回方式：隐式返回和显式返回。当awk执行到函数的结尾时，它自动地返回到调用程序，这是函数是隐式返回的。如果需要在结束之前退出函数，可以明确地使用返回语句提前退出。方法是在函数中使用形如：return 返回值 格式的语句。 

例：下面的例子演示了函数的使用。在这个示例中，定义了一个名为print_header的函数，该函数调用了两个参数FileName和PageNum，
FileName参数传给函数当前使用的文件名，PageNum参数是当前页的页号。这个函数的功能是打印（显示）出当前文件的文件名，
和当前页的页号。完成这个功能后，这个函数将返回下一页的页号。 

nawk 
>'BEGIN{pageno=1;file=FILENAME 
>pageno=print_header(file，pageno)；#调用函数print_header 
>printf("当前页页号是：%d",pageno); 
>} 

>#定义函数print_header 
>function print_header(FileName,PageNum){ 
>printf("%s %d",FileName,PageNum); 
>PageNum++;return PageNUm; 
>} 
>}' myfile 

执行这个程序将显示如下内容： 

myfile 1 
当前页页号是：2 

awk高级输入输出 
================

1.读取下一条记录： 

awk的next语句导致awk读取下一个记录并完成模式匹配，然后立即执行相应的操作。通常它用匹配的模式执行操作中的代码。
next导致这个记录的任何额外匹配模式被忽略。

2.简单地读取一条记录 

awk的 getline语句用于简单地读取一条记录。如果用户有一个数据记录类似两个物理记录，那么getline将尤其有用。它完成一般字段的分离(设置字段变量$0 FNR NF NR)。如果成功则返回1，失败则返回0（到达文件尾）。如果需简单地读取一个文件，则可以编写以下代码： 

例：示例getline的使用 

{while(getline==1) 
{ 
#process the inputted fields 
} 
} 

也可以使getline保存输入数据在一个字段中，而不是通过使用getline variable的形式处理一般字段。当使用这种方式时，
NF被置成0，FNR和NR被增值。 用户也可以使用getline<"filename"方式从一个给定的文件中输入数据，而不是从命令行所列内容输入数据。
此时，getline将完成一般字段分离（设置字段变量$0和NF)。如果文件不存在，返回-1,成功，返回1,返回0表示失败。
用户可以从给定文件中读取数据到一个变量中，也可以用stdin(标准输入设备）或一个包含这个文件名的变量代替filename。值得注意的是当使用这种方式时不修改FNR和NR。 

另一种使用getline语句的方法是从UNIX命令接受输入，例如下面的例子: 
例：示例从UNIX命令接受输入 

{while("who -u"|getline) 
{ 
#process each line from the who command 
} 
} 

当然，也可以使用如下形式: 

"command" | getline variable 

3.关闭文件: 

awk中允许在程序中关闭一个输入或输出文件，方法是使用awk的close语句。 

close("filename") 

filename可以是getline打开的文件（也可以是stdin,包含文件名的变量或者getline使用的确切命令）。或一个输出文件（可以是stdout，包含文件名的变量或使用管道的确切命令）。 
 
4.输出到一个文件: 

awk中允许用如下方式将结果输出到一个文件： 

printf("hello word!")>"datafile" 
或 
printf("hello word!")>>"datafile" 

5.输出到一个命令 

awk中允许用如下方式将结果输出到一个命令： 

printf("hello word!")|"sort-t','" 

awk与shell script混合编程 
===========================
因为awk可以作为一个shell命令使用，因此awk能与shell批处理程序很好的融合在一起，这给实现awk与shell程序的混合编程提供了可能。
实现混合编程的关键是awk与shell script之间的对话，换言之，就是awk与shell script之间的信息交流:awk从shell script中获取所需
的信息（通常是变量的值）、在awk中执行shell命令行、shell script将命令执行的结果送给awk处理以及shell script读取awk的执行结果等等。 

1.awk读取Shell script程序变量 
 

 “‘$var'”

这种写法大家无需改变用‘括起awk程序的习惯,是老外常用的写法.如:

var=”test”
awk ‘BEGIN{print “‘$var'”}’

这种写法其实际是双括号变为单括号的常量,传递给了awk.

如果var中含空格,为了shell不把空格作为分格符,便应该如下使用:

var=”this is a test”
awk ‘BEGIN{print “‘”$var”‘”}’

2.将shell命令的执行结果送给awk处理 

作为信息传送的一种方法，我们可以将一条shell命令的结果通过管道线（|）传递给awk处理： 

例：示例awk处理shell命令的执行结果 

$who -u | awk '{printf("%s正在执行%s",$2,$1)}' 

该命令将打印出注册终端正在执行的程序名。 

3.shell script程序读awk的执行结果 

为了实现shell script程序读取awk执行的结果，我们可以采取一些特殊的方法，例如我们可以用变量名=`awk语句`的形式将awk执行的结果存放入一个shell script变量。当然也可以用管道线的方法将awk执行结果传递给shell script程序处理。 

例：作为传送消息的机制之一，UNIX提供了一个向其所有用户传送消息的命令wall（意思是write to all写给所有用户），该命令允许向所有工作中的用户（终端）发送消息。为此，我们可以通过一段shell批处理程序wall.shell来模拟这一程序（事实上比较老的版本中wall就是一段shell批处理程序： 

$cat wall.shell 
: 
# @(#) wall.shell:发送消息给每个已注册终端 
# 
cat >/tmp/$$ 
#用户录入消息文本 who -u | awk '{print $2}' | while read tty 
do 
cat /tmp/$$>$tty 
done 

在这个程序里，awk接受who -u命令的执行结果，该命令打印出所有已注册终端的信息，其中第二个字段是已注册终端的设备名，因此用awk命令析出该设备名，然后用while read tty语句循环读出这些文件名到变量（shell script变量）tty中，作为信息传送的终结地址。 

4.在awk中执行shell命令行----嵌入函数system() 

system()是一个不适合字符或数字类型的嵌入函数，该函数的功能是处理作为参数传递给它的字符串。system对这个参数的处理就是将其作为命令处理，也就是说将其当作命令行一样加以执行。这使得用户在自己的awk程序需要时可以灵活地执行命令或脚本。 

例：下面的程序将使用system嵌入函数打印用户编制好的报表文件，这个文件存放在名为myreport.txt的文件中。为简约起见，我们只列出了其END部分： 

. 
. 
. 
END {close("myreport.txt");system("lp myreport.txt");} 

在这个示例中，我们首先使用close语句关闭了文件myreport.txt文件，然后使用system嵌入函数将myreport.txt送入打印机打印。 

写到这里，我不得不跟朋友们说再见了，实在地说，这些内容仍然是awk的初步知识，电脑永远是前进的科学，awk也不例外，本篇所能做的只是在你前行的漫漫长途中铺平一段小小开端，剩下的路还得靠你自己去走。老实说，如果本文真能给你前行的路上带来些许的方便，那本人就知足了！ 

如对本篇有任何疑问，请E-mail To:Chizlong@yeah.net或到主页http://chizling.yeah.net中留言。 


附录： 

1.awk的常规表达式元字符 

换码序列 
^ 在字符串的开头开始匹配 
$ 在字符串的结尾开始匹配 
. 与任何单个字符串匹配 
[ABC] 与[]内的任一字符匹配 
[A-Ca-c] 与A-C及a-c范围内的字符匹配（按字母表顺序） 
[^ABC] 与除[]内的所有字符以外的任一字符匹配 
Desk|Chair 与Desk和Chair中的任一个匹配 
[ABC][DEF] 关联。与A、B、C中的任一字符匹配，且其后要跟D、E、F中的任一个字符。 
* 与A、B或C中任一个出现0次或多次的字符相匹配 
+ 与A、B或C中任何一个出现1次或多次的字符相匹配 
？ 与一个空串或A、B或C在任何一个字符相匹配 
（Blue|Black）berry 合并常规表达式，与Blueberry或Blackberry相匹配 

2.awk算术运算符 

运算符 用途 
------------------ 
x^y x的y次幂 
x**y 同上 
x%y 计算x/y的余数（求模） 
x+y x加y 
x-y x减y 
x*y x乘y 
x/y x除y 
-y 负y(y的开关符号);也称一目减 
++y y加1后使用y(前置加） 
y++ 使用y值后加1（后缀加） 
--y y减1后使用y(前置减） 
y-- 使用后y减1(后缀减） 
x=y 将y的值赋给x 
x+=y 将x+y的值赋给x 
x-=y 将x-y的值赋给x 
x*=y 将x*y的值赋给x 
x/=y 将x/y的值赋给x x%=y 将x%y的值赋给x 
x^=y 将x^y的值赋给x 
x**=y 将x**y的值赋给x 

3.awk允许的测试： 

操作符 含义 

x==y x等于y 
x!=y x不等于y 
x>y x大于y 
x>=y x大于或等于y 
x<y x小于y 
x<=y x小于或等于y? 
x~re x匹配正则表达式re? 
x!~re x不匹配正则表达式re? 

4.awk的操作符(按优先级升序排列) 

= 、+=、 -=、 *= 、/= 、 %= 
|| 
&& 
> >= < <= == != ~ !~ 
xy (字符串连结，'x'y'变成"xy"） 
+ - 
* / % 
++ -- 

5.awk内置变量（预定义变量） 

说明：表中v项表示第一个支持变量的工具（下同）：A=awk，N=nawk,P=POSIX awk,G=gawk 

V 变量 含义 缺省值 
-------------------------------------------------------- 
N ARGC 命令行参数个数 
G ARGIND 当前被处理文件的ARGV标志符 
N ARGV 命令行参数数组 
G CONVFMT 数字转换格式 %.6g 
P ENVIRON UNIX环境变量 
N ERRNO UNIX系统错误消息 
G FIELDWIDTHS 输入字段宽度的空白分隔字符串 
A FILENAME 当前输入文件的名字 
P FNR 当前记录数 
A FS 输入字段分隔符 空格 
G IGNORECASE 控制大小写敏感0（大小写敏感） 
A NF 当前记录中的字段个数 
A NR 已经读出的记录数 
A OFMT 数字的输出格式 %.6g 
A OFS 输出字段分隔符 空格 
A ORS 输出的记录分隔符 新行 
A RS 输入的记录他隔符 新行 
N RSTART 被匹配函数匹配的字符串首 
N RLENGTH 被匹配函数匹配的字符串长度 
N SUBSEP 下标分隔符 "34" 

6.awk的内置函数 

V 函数 用途或返回值 
------------------------------------------------ 
N gsub(reg,string,target) 每次常规表达式reg匹配时替换target中的string 
N index(search,string) 返回string中search串的位置 
A length(string) 求串string中的字符个数 
N match(string,reg) 返回常规表达式reg匹配的string中的位置 
N printf(format,variable) 格式化输出，按format提供的格式输出变量variable。 
N split(string,store,delim) 根据分界符delim,分解string为store的数组元素 
N sprintf(format,variable) 返回一个包含基于format的格式化数据，variables是要放到串中的数据 
G strftime(format,timestamp) 返回一个基于format的日期或者时间串，timestmp是systime()函数返回的时间 
N sub(reg,string,target) 第一次当常规表达式reg匹配，替换target串中的字符串 
A substr(string,position,len) 返回一个以position开始len个字符的子串 
P totower(string) 返回string中对应的小写字符 
P toupper(string) 返回string中对应的大写字符 
A atan(x,y) x的余切(弧度) 
N cos(x) x的余弦(弧度) 
A exp(x) e的x幂 
A int(x) x的整数部分 
A log(x) x的自然对数值 
N rand() 0-1之间的随机数 
N sin(x) x的正弦(弧度) 
A sqrt(x) x的平方根 
A srand(x) 初始化随机数发生器。如果忽略x，则使用system() 
G system() 返回自1970年1月1日以来经过的时间（按秒计算） 

案例大全
=================

输出文件的前10行（模拟 head -n filename，其中n取值是10 ）

awk ' NR < 11 '  #不规范，不建议，浪费时间 
awk读取前11行（不包括第11行）。如前所述，这里省略了动作打印输出print。匹配模式是变量NR需要小于11，NR即为当前的行号。
这个写法很简单，但是有一个问题，在NR大于10的时候，awk其实还是对每行进行了判断，如果文件很大，比如说有上万行，浪费的时间
是无法忽略的。所以，更好的写法是

awk '1; NR = 10 { exit }'   #我的理解是从第一行开始读数据，将第10行到数据读完后退出。 
第一句对当前行进行输出。第二句判断是不是已经到了第10行，如果是则退出。





输出文件的第一行（模拟 head -n filename  将n用1替代 ）

awk 'NR > 1 { exit }; 1' 
这个例子与前一个很相似，中心思想就是第二行就退出。





输出文件的最后两行（模拟 tail -n filename其中n取值为2 ）

awk '{ y=x "\n" $0; x=$0}; END { print y }'   #效率太低不推荐 
第一句总是把一个在当前行前面再加上变量x的内容赋值给y，然后用x记录当前行内容。这样的效果是y的内容始终是上一行加上当前行的
内容。在最后，输出y的内容。如果仔细看的话，不难发现这个写法是很不高效的，因为它不停的进行赋值和字符串连接，只为了找到最后
一行！所以，如果你想要输出文件的最后两行，'tail -2 filename'是最好的选择。



输出文件的最后一行（模拟 tail -n filename 其中n取值为1 ）

awk 'END { print }' 
句法方面没什么好说的，print省略参数即是等价于print $0。但是这个语句可能不能被非GNU awk的某些awk版本正常执行，如果为了兼容，下面的写法是最安全的：

awk '{ rec = $0 }; END { print rec }'   #这个写法更具有兼容性






 
输出只匹配某些模式的行（模拟 grep ）

awk '/regex/' 
似乎没什么好说的了。




输出不匹配某些模式的行（模拟 grep -v ）

awk '!/regex/' 
匹配模式前加“!”就是否定判断结果。





输出匹配模式的行的上一行，而非当前行

awk '/regex/ { print x }; { x = $0 }' 
变量x总是用来记录上一行的内容，如果模式匹配了当前行，则输出x的内容。





输出匹配模式的下一行

awk '/regex/ { getline; print }' 
这里使用了getline函数取得下一行的内容并输出。getline的作用是将$0的内容置为下一行的内容，并同时更新NR，NF，FNR变量。如果匹配的是最后一行，getline会出错，$0不会被更新，最后一行会被打印。





输出匹配AA  或  BB  或  CC的行

awk '/AA|BB|CC/' 
没什么好说的，正则表达式。如果有看不懂的朋友，请自行学习正则表达式。





输出长过65个字符的行

awk 'length > 64' 
length([str])返回字符串的长度，如果参数省略，即是以$0作为参数，括号也可以省略了。

输出短于65个字符的行

awk 'length < 65' 
和上例基本一样。





输出从匹配行到最后一样的内容

awk '/regex/,0' 
这里使用了“pattern1,pattern2”的形式来指定一个匹配的范围，其中pattern2这里为0，也就是false，所以一直会匹配到文件结束。





从第8行输出到第12行

awk 'NR==8,NR==12' 
同上例，这也是个范围匹配。





输出第52行

awk 'NR==52'  #不建议因为到了第52行awk还会继续往下读取下列的行。 
如果想要少执行些不必要的循环，就这样写：

awk 'NR==52 {print;exit}'    #建议用这个写法




 
输出两次正则表达式匹配之间的行

awk '/regex1/, /regex2/'




 
删除所有的空行

awk NF        #  NF为真即是非空行。

另外一种写法是用正则表达式：
awk '/./' 
这个很类似grep .的思路，但是不如awk NF好的，因为“.”也是可以匹配空格和TAB的。





打印出空行行号

awk '/^$/{print NR}' filename

同理：打印出含有字符good的行号

awk '/good/{print NR}' filename





打印报告头和结尾    #没有验证，我也没看懂

awk 'BEGIN {print "numAtnumBn------------"} {print $1"t"$2} END {print "ENDAt ENDBn-----------"}' test.txt


numA    numB
------------
1       2
3       4

ENDA     ENDB
-----------





=============================

<            小于
<=        小于等于
==        等于
!=      不等于
>            大于
>=        大于等于
~         匹配正则表达式
!~      不匹配正则表达式

b        退格键
f      走纸换页
n      新行
r      回车键
t      tab键            #特别注意这几个红色字体，常用到
ddd    八进制值
c        任意其他特殊字符



-------------------------------------[举例子]------------
源文件：
[root@BJIT tmp]# more test.txt
1       a
3       b
5       c
7       a
9       b
15      c

用if判断匹配
[root@BJIT tmp]#  awk '{if($1>5) print $1}' test.txt         #将第一列中数值大于5的数值输出
7
9
15
[root@BJIT tmp]#  awk '{if($2~/a/) print $1}' test.txt   #如果第二列和a匹配，则输出他们对应的第一列到数值
1
7




用==号匹配——==                        #一个=是赋值的含义，两个==才是相等表条件
[root@BJIT tmp]# awk '$1=="3" {print $0}' test.txt     #如果第一列等于3则输出其在$0当前所有行对应的全部
3       b




匹配正则
[root@BJIT tmp]# awk '$1 ~ ".5" {print $1}' test.txt      #不是很理解注意5前有个小数点
15
[root@BJIT tmp]# awk '{if($2~/c/)print $1}' test.txt      　#如果第2列有匹配的c则输出其对应在第一列的数值
5
15




关系匹配：
[root@BJIT tmp]# awk '$1 ~ "5|7" {print $0}' test.txt   #第一列中只要有5或者7匹配则输出其对应的全部
5       c
7       a
15      c




AND匹配——&&：
[root@BJIT tmp]# awk '$1=="5" && $2=="c" {print $0}' test.txt      #如果第一列有5且其在第二列中对应c则输出
5       c




或匹配——||：两边任意为真                      # 或不是一条竖线也可以吗，疑问？
[root@BJIT tmp]# awk '$2=="a" || $1=="15" {print $0}' test.txt
1        a
7        a
15      c




判断不匹配——！            # 感叹号在正则表达式中表示的是非，即否定的含义。
[root@BJIT tmp]# awk '$1!="3" {print $0}' test.txt    
1       a
5       c
7       a
9       b
15      c
[root@BJIT tmp]# awk '{if($2!~/a/) print $0}' test.txt
3       b
5       c
9       b
15      c






NR  和  NF
NR：记录已读的记录数
NF：浏览记录的域个数
[root@BJIT tmp]# awk '{print NR"\t"NF"\t"$0}' test.txt       # 注意这里到转义字符t表示tab占位符。
1       2       1       a
2       2       3       b
3       2       5       c
4       2       7       a
5       2       9       b
6       2       15      c


[root@BJIT tmp]# awk '{if(NR>0 && $1>7) print $0}' test.txt
9       b
15      c


[root@BJIT tmp]# awk '{if ($1==15)print $NF}' test.txt    $NF打印最后域  
c                      #   $NF表示的是变量NF的域，即她所对应的$0中最后一个数值。












域值比较（两种方法）
[root@BJIT tmp]# awk '{if($1<$2)print $0}' test.txt1       # 没有看懂
1       a
3       b
5       c
7       a
9       b
15      c
[root@BJIT tmp]# awk 'BEGIN {num=15}{if($1==num)print $0}' test.txt
15      c        #从数字15开始，如果第一列中变量的取值有等于15的就全部输出






修改数值域取值：
[root@BJIT tmp]# awk '{$1=$1-2;print $1}' test.txt     # 将第一列减去2之后在赋值给原来的数值
-1
1
3
5
7
13


[root@BJIT tmp]# awk '{if($1>2)($1="test"); print $0}' test.txt  

#  全部输出且如果第一列中有数值大于2，则用test替代。
1    a
test b
test c
test a
test b
test c


[root@BJIT tmp]# awk '{if($1==5){$1="test";print $1}}' test.txt
test





数值相加：
[root@BJIT tmp]# awk '{tot+=$1}; {print $1,$2} END{print tot}' test.txt

#   +=是一个正则表达式，再次即tot等于$1列中所有数字之和，因为$1表示的是列，而awk在执行的时候是逐行扫描读取的。 最后END输出这个tot的最终数值。
1  a
3  b
5  c
7  a
9  b
15 c
40



[root@BJIT tmp]#  awk '{tot+=$1}; {print $1,$2} END{print tot/100}' test.txt    

#  将相加的结果除以100    

1 a

3 b

5 c

7 a

9 b

15 c

0.4






替换字符串:                    (试验中替换成字母不成功)
[root@BJIT tmp]# awk 'gsub(/3/,123) {print $0}' test.txt
123     b

按照起始位置及长度返回字符串
[root@BJIT tmp]# more test2.txt
12345678
1234567
123456
12345
[root@BJIT tmp]# awk '{print substr($1,1,3)}' test2.txt
123
123
123
123
[root@BJIT tmp]# awk '$1==12345678 {print substr($1,1,5)}' test2.txt
12345
[root@BJIT tmp]# awk '$1==12345678 {print substr($1,3,5)}' test2.txt
34567
[root@BJIT tmp]# echo 12345678 | awk '{print substr ($1,3,6)}'
345678





字符
[root@BJIT tmp]# awk 'BEGIN {print "AtBnCtD"}'
A       B
C       D

删掉每行的最后一个字符

awk -F'|' '{print $1"|"$2"|"$3"|"$4}' filename

sed 's/.$//g' filename



源文件：

cat filename
1  2     3        4
1  2     3        4
1  2     3        4
1  2     3        4
将其中的空格都以tab键替换
awk '{print $1"t"$2"t"$3"t"$4}' filename
awk 'BEGIN {OFS="t"}{print $1,$2,$3,$4}'  filename


## Search 文件命令
我们经常在linux要查找某个文件，但不知道放在哪里了，可以使用下面的一些命令来搜索： 
       which  查看可执行文件的位置。
       whereis 查看文件的位置。 
       locate   配合数据库查看文件位置。
       find   实际搜寻硬盘查询文件名称。

which命令的作用是，在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。也就是说，使用which命令，就可以看到某个系统命令是否存在，以及执行的到底是哪一个位置的命令。 

1．命令格式：

which 可执行文件名称 

2．命令功能：

which指令会在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。

3．命令参数：

-n 　指定文件名长度，指定的长度必须大于或等于所有文件中最长的文件名。

-p 　与-n参数相同，但此处的包括了文件的路径。

-w 　指定输出时栏位的宽度。

-V 　显示版本信息

4．使用实例：

实例1：查找文件、显示命令路径

命令：

which lsmod

输出：

[root@localhost ~]# which pwd

/bin/pwd

[root@localhost ~]#  which adduser

/usr/sbin/adduser

[root@localhost ~]#

说明：

which 是根据使用者所配置的 PATH 变量内的目录去搜寻可运行档的！所以，不同的 PATH 配置内容所找到的命令当然不一样的！


实例2：用 which 去找出 which

命令：

  which which

输出：

[root@localhost ~]# which which

alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot  --show-tilde'

     /usr/bin/which

[root@localhost ~]#

说明：

竟然会有两个 which ，其中一个是 alias 这就是所谓的『命令别名』，意思是输入 which 会等於后面接的那串命令！

实例3：找出 cd 这个命令

命令：

 which cd

输出：

 

       说明：

cd 这个常用的命令竟然找不到啊！为什么呢？这是因为 cd 是bash 内建的命令！ 但是 which 默认是找 PATH 内所规范的目录，所以当然一定找不到的！

## Sed 用法
0. Sed简介   
sed 是一种在线编辑器，它一次处理一行内容。处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”（pattern space），接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。接着处理下一行，这样不断重复，直到文件末尾。文件内容并没有 改变，除非你使用重定向存储输出。Sed主要用来自动编辑一个或多个文件；简化对文件的反复操作；编写转换程序等。以下介绍的是Gnu版本的Sed 3.02。   

1. sed [options] 'command' in_file[s]
options 部分
-n 阻止输入行自动输出
-e 
-i
-f 脚本文件
-r 支持拓展正则

command 部分
'[地址1,地址2] [函数] [参数(标记)]'

定址的方法 1.数字 2.正则 
注意：只有这两种方法，不能用变量如sed -n '`echo 2`,$ p' new_fxsp.txt 会报错，Unrecognized command: `echo 2`,$ p

数字
  十进制数
1 单行   
1,3 范围 从第一行到第三行
2,+4 匹配行后若干行
4,~3   从第四行到下一个3的倍数行
2~3 第二行起每间隔三行的行
$ 尾行
1! 除了第一行以外的行


2. 定址   
可以通过定址来定位你所希望编辑的行，该地址用数字构成，用逗号分隔的两个行数表示以这两行为起止的行的范围（包括行数表示的那两行）。如1，3表示1，2，3行，美元符号($)表示最后一行。范围可以通过数据，正则表达式或者二者结合的方式确定 。   
  
3. Sed命令   
调用sed命令有两种形式：   
*   
sed [options] 'command' file(s)   
*   
sed [options] -f scriptfile file(s)   
a\   
在当前行后面加入一行文本。   
b lable   
分支到脚本中带有标记的地方，如果分支不存在则分支到脚本的末尾。   
c\   
用新的文本改变本行的文本。   
d   
从模板块（Pattern space）位置删除行。   
D   
删除模板块的第一行。   
i\   
在当前行上面插入文本。   
h   
拷贝模板块的内容到内存中的缓冲区。   
H   
追加模板块的内容到内存中的缓冲区   
g   
获得内存缓冲区的内容，并替代当前模板块中的文本。   
G   
获得内存缓冲区的内容，并追加到当前模板块文本的后面。   
l   
列表不能打印字符的清单。   
n   
读取下一个输入行，用下一个命令处理新的行而不是用第一个命令。   
N   
追加下一个输入行到模板块后面并在二者间嵌入一个新行，改变当前行号码。   
p   
打印模板块的行。   
P（大写）   
打印模板块的第一行。   
q   
退出Sed。   
r file   
从file中读行。   
t label   
if分支，从最后一行开始，条件一旦满足或者T，t命令，将导致分支到带有标号的命令处，或者到脚本的末尾。   
T label   
错误分支，从最后一行开始，一旦发生错误或者T，t命令，将导致分支到带有标号的命令处，或者到脚本的末尾。   
w file   
写并追加模板块到file末尾。   
W file   
写并追加模板块的第一行到file末尾。   
!   
表示后面的命令对所有没有被选定的行发生作用。   
s/re/string   
用string替换正则表达式re。   
=   
打印当前行号码。   
#   
把注释扩展到下一个换行符以前。   
以下的是替换标记   
*   
g表示行内全面替换。   
*   
p表示打印行。   
*   
w表示把行写入一个文件。   
*   
x表示互换模板块中的文本和缓冲区中的文本。   
*   
y表示把一个字符翻译为另外的字符（但是不用于正则表达式）   
  
4. 选项   
-e command, --expression=command   
允许多台编辑。   
-h, --help   
打印帮助，并显示bug列表的地址。   
-n, --quiet, --silent   
  
取消默认输出。   
-f, --filer=script-file   
引导sed脚本文件名。   
-V, --version   
打印版本和版权信息。   

-i, 使用 -i选项直接修改文件
  
5. 元字符集^   
锚定行的开始 如：/^sed/匹配所有以sed开头的行。    
$   
锚定行的结束 如：/sed$/匹配所有以sed结尾的行。    
.   
匹配一个非换行符的字符 如：/s.d/匹配s后接一个任意字符，然后是d。    
*   
匹配零或多个字符 如：/*sed/匹配所有模板是一个或多个空格后紧跟sed的行。   
[]  
匹配一个指定范围内的字符，如/[Ss]ed/匹配sed和Sed。   
[^]  
匹配一个不在指定范围内的字符，如：/[^A-RT-Z]ed/匹配不包含A-R和T-Z的一个字母开头，紧跟ed的行。   
\(..\)  
保存匹配的字符，如s/\(love\)able/\1rs，loveable被替换成lovers。   
&  
保存搜索字符用来替换其他字符，如s/love/**&**/，love这成**love**。    
\<   
锚定单词的开始，如:/\<love/匹配包含以love开头的单词的行。    
\>   
锚定单词的结束，如/love\>/匹配包含以love结尾的单词的行。    
x\{m\}   
重复字符x，m次，如：/0\{5\}/匹配包含5个o的行。    
x\{m,\}   
重复字符x,至少m次，如：/o\{5,\}/匹配至少有5个o的行。    
x\{m,n\}   
重复字符x，至少m次，不多于n次，如：/o\{5,10\}/匹配5--10个o的行。   
6. 实例   
删除：d命令   
*   
$ sed '2d' example-----删除example文件的第二行。   
*   
$ sed '2,$d' example-----删除example文件的第二行到末尾所有行。   
*   
$ sed '$d' example-----删除example文件的最后一行。   
*   
$ sed '/test/'d example-----删除example文件所有包含test的行。   
替换：s命令   
*   
$ sed 's/test/mytest/g' example-----在整行范围内把test替换为mytest。如果没有g标记，则只有每行第一个匹配的test被替换成mytest。   
*   
$ sed -n 's/^test/mytest/p' example-----(-n)选项和p标志一起使用表示只打印那些发生替换的行。也就是说，如果某一行开头的test被替换成mytest，就打印它。   
*   
$ sed 's/^192.168.0.1/&localhost/' example-----&符号表示替换换字符串中被找到的部份。所有以192.168.0.1开头的行都会被替换成它自已加 localhost，变成192.168.0.1localhost。   
*   
$ sed -n 's/\(love\)able/\1rs/p' example-----love被标记为1，所有loveable会被替换成lovers，而且替换的行会被打印出来。   
*   
$ sed 's#10#100#g' example-----不论什么字符，紧跟着s命令的都被认为是新的分隔符，所以，“#”在这里是分隔符，代替了默认的“/”分隔符。表示把所有10替换成100。   
选定行的范围：逗号   
*   
$ sed -n '/test/,/check/p' example-----所有在模板test和check所确定的范围内的行都被打印。   
*   
$ sed -n '5,/^test/p' example-----打印从第五行开始到第一个包含以test开始的行之间的所有行。   
*   
$ sed '/test/,/check/s/$/sed test/' example-----对于模板test和west之间的行，每行的末尾用字符串sed test替换。   
多点编辑：e命令   
*   
$ sed -e '1,5d' -e 's/test/check/' example-----(-e)选项允许在同一行里执行多条命令。如例子所示，第一条命令删除1至5行，第二条命令用check替换test。命令的执 行顺序对结果有影响。如果两个命令都是替换命令，那么第一个替换命令将影响第二个替换命令的结果。   
*   
$ sed --expression='s/test/check/' --expression='/love/d' example-----一个比-e更好的命令是--expression。它能给sed表达式赋值。   
从文件读入：r命令   
*   
$ sed '/test/r file' example-----file里的内容被读进来，显示在与test匹配的行后面，如果匹配多行，则file的内容将显示在所有匹配行的下面。   
写入文件：w命令   
*   
$ sed -n '/test/w file' example-----在example中所有包含test的行都被写入file里。   
追加命令：a命令   
*   
$ sed '/^test/a\\--->this is a example' example<-----'this is a example'被追加到以test开头的行后面，sed要求命令a后面有一个反斜杠。   
插入：i命令   
$ sed '/test/i\\   
new line   
-------------------------' example   
如果test被匹配，则把反斜杠后面的文本插入到匹配行的前面。   
下一个：n命令   
*   
$ sed '/test/{ n; s/aa/bb/; }' example-----如果test被匹配，则移动到匹配行的下一行，替换这一行的aa，变为bb，并打印该行，然后继续。   
变形：y命令   
*   
$ sed '1,10y/abcde/ABCDE/' example-----把1--10行内所有abcde转变为大写，注意，正则表达式元字符不能使用这个命令。   
退出：q命令   
*   
$ sed '10q' example-----打印完第10行后，退出sed。   
保持和获取：h命令和G命令   
*   
$ sed -e '/test/h' -e '$G example-----在sed处理文件的时候，每一行都被保存在一个叫模式空间的临时缓冲区中，除非行被删除或者输出被取消，否则所有被处理的行都将 打印在屏幕上。接着模式空间被清空，并存入新的一行等待处理。在这个例子里，匹配test的行被找到后，将存入模式空间，h命令将其复制并存入一个称为保 持缓存区的特殊缓冲区内。第二条语句的意思是，当到达最后一行后，G命令取出保持缓冲区的行，然后把它放回模式空间中，且追加到现在已经存在于模式空间中 的行的末尾。在这个例子中就是追加到最后一行。简单来说，任何包含test的行都被复制并追加到该文件的末尾。   
保持和互换：h命令和x命令   
*   
$ sed -e '/test/h' -e '/check/x' example -----互换模式空间和保持缓冲区的内容。也就是把包含test与check的行互换。   
7. 脚本   
Sed脚本是一个sed的命令清单，启动Sed时以-f选项引导脚本文件名。Sed对于脚本中输入的命令非常挑剔，在命令的末尾不能有任何空白或文本，如果在一行中有多个命令，要用分号分隔。以#开头的行为注释行，且不能跨行。

## ## 解析 shell 赋值语句 => PROFILE_VAR=${1:-"PROFILE_VARO"}
${varname:-word},如果变量$varname存在且非null,则返回其值;否则,返回"PROFILE_VARO"。用途:如果变量未定义,则返回默认值。

## tar -xzvf POS_VARUTILS_3.5.7.tar.gz

## 重定向输入 ( << EOF )
我们经常在shell脚本程序中用<<EOF重定向输入，将我们输入的命令字符串作为一个执行程序的输入，这样，我们就不需要在那个程序环境中手工输入命令，以便自动执行我们需要的功能，例如：


[plain] view plain copy 在CODE上查看代码片派生到我的代码片
sqlplus emssxjk/emssxjk <<EOF  
select count(*) from sncn_yxyj where create_date like sysdate;  
EOF  


其中的SQL语句相当于在sqlplus程序环境中输入的，这样输入的内容夹在两个EOF之间，可长可短，EOF也可以换成其他任意的字符，大小写不论，只要成对出现即可，例如：

[plain] view plain copy 在CODE上查看代码片派生到我的代码片
sqlplus emssxjk/emssxjk <<STD  
select count(*) from sncn_yxyj where create_date like sysdate;  
STD  

当然这个标志性字符不能用保留字，最常用的还是EOF。
需要注意的是，第一个EOF必须以重定向字符<<开始，第二个EOF必须顶格写，否则会报错。

## 命令执行控制（&& 和 ||）
shell 在执行某个命令的时候，会返回一个返回值，该返回值保存在 shell 变量 $? 中。当 $? == 0 时，表示执行成功；当 $? == 1 时，表示执行失败。
有时候，下一条命令依赖前一条命令是否执行成功。如：在成功地执行一条命令之后再执行另一条命令，或者在一条命令执行失败后再执行另一条命令等。shell 提供了 && 和 || 来实现命令执行控制的功能，shell 将根据 && 或 || 前面命令的返回值来控制其后面命令的执行。
1.命令之间使用 && 连接，实现逻辑与的功能。
2.只有在 && 左边的命令返回真（命令返回值 $? == 0），&& 右边的命令才会被执行。
3.只要有一个命令返回假（命令返回值 $? == 1），后面的命令就不会被执行。

1.命令之间使用 || 连接，实现逻辑或的功能。
2.只有在 || 左边的命令返回假（命令返回值 $? == 1），|| 右边的命令才会被执行。这和 c 语言中的逻辑或语法功能相同，即实现短路逻辑或操作。
3.只要有一个命令返回真（命令返回值 $? == 0），后面的命令就不会被执行。

## tail +3 $filename
将文件的前两行去掉，剩下的内容然后显示在终端 


## Linux挂载点（windows中的盘符如C:\）& Linux文件系统(windows中的文件或目录) & 文件
挂载点：实际上就是linux中的磁盘文件系统的入口目录，类似于windows中的用来访问不同分区（通常是磁盘的一部分，分区的目的是区分功能）的C:、D:、E:等盘符。其实winxp也支持将一个磁盘分区挂在一个文件夹下面，只是我们C:、D:这样的盘符操作用惯了，一般没有将分区挂到文件夹。

linux文件系统：所有硬件设备和磁盘分区的集合就是linux文件系统。linux、unix这类操作系统将系统中的一切都作为文件来管理。在windows中我们常见的硬件设备、磁盘分区等，在linux、unix中都被视作文件，对设备、分区的访问就是读写对应的文件。df -a 命令可以显示文件系统（所有设备、分区）列表。


Linux的分区，之所以让大家头疼，就是因为它并不是给每个分区，分配一个“字母盘符”，而是通过具体的“文件夹名字”来进行“挂载”从而进行功能的区分。其实，大家在潜意识里，明白这些挂载点的意思，就行了。别非想着Windows，分区就得有个盘符。。。。。。。^_^想用Linux，很多观念都必须要改变滴~
 


du -sh . 
当前目录下所有文件的大小总和

ctrl+v+m -> ^M(windows 换行符)
%s/^M//g


vi显示行号
：set nu

vi 模式字符串替换
%s/str1/str2/g  =〉%表示全局；s表示替换；g表示如果该行有不止一个，全部替换。

查找
find .|xargs grep "VARCON1"


测试key
 scp -v -P 22 -i /shared/home/posop/.ssh/rposop_rsa2 /shared/opt/pos/mxg/mxg_bodev8/datamart_extractions/sab_rec_trd_soff.txt RATESUSER1@uklvadsab60.uk:/shared/opt/SCB/RTA/recon/data/


将/shared/scratch/cc_develop/POS_TOOLS_GL_5.3.10.tar.gz解压在当前目录下。
tar -zxvf /shared/scratch/cc_develop/POS_TOOLS_GL_5.3.10.tar.gz

解压xxx.gz文件 
gunzip  xxx.gz (xxx.gz将消失)


dos2unix
可以把文本里的^M /r删除

split -l 1000000 fileName
指定行数将文件进行切割，遇到大文件无法用diff比较时，可先切割，再diff比较每个切割的文件

##比较两个folder下所有文件的内容是否相同。
diff -p -r folder1 folder2

## ^ M 
'cat -v' or 'vi'


## 
sp_helptext

## sed用法
1. Sed简介   
sed 是一种在线编辑器，它一次处理一行内容。处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”（pattern space），接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。接着处理下一行，这样不断重复，直到文件末尾。文件内容并没有 改变，除非你使用重定向存储输出。Sed主要用来自动编辑一个或多个文件；简化对文件的反复操作；编写转换程序等。以下介绍的是Gnu版本的Sed 3.02。   
2. 定址   
可以通过定址来定位你所希望编辑的行，该地址用数字构成，用逗号分隔的两个行数表示以这两行为起止的行的范围（包括行数表示的那两行）。如1，3表示1，2，3行，美元符号($)表示最后一行。范围可以通过数据，正则表达式或者二者结合的方式确定 。   
  
3. Sed命令   
调用sed命令有两种形式：   
*   
sed [options] 'command' file(s)   
*   
sed [options] -f scriptfile file(s)   
a\   
在当前行后面加入一行文本。   
b lable   
分支到脚本中带有标记的地方，如果分支不存在则分支到脚本的末尾。   
c\   
用新的文本改变本行的文本。   
d   
从模板块（Pattern space）位置删除行。   
D   
删除模板块的第一行。   
i\   
在当前行上面插入文本。   
h   
拷贝模板块的内容到内存中的缓冲区。   
H   
追加模板块的内容到内存中的缓冲区   
g   
获得内存缓冲区的内容，并替代当前模板块中的文本。   
G   
获得内存缓冲区的内容，并追加到当前模板块文本的后面。   
l   
列表不能打印字符的清单。   
n   
读取下一个输入行，用下一个命令处理新的行而不是用第一个命令。   
N   
追加下一个输入行到模板块后面并在二者间嵌入一个新行，改变当前行号码。   
p   
打印模板块的行。   
P（大写）   
打印模板块的第一行。   
q   
退出Sed。   
r file   
从file中读行。   
t label   
if分支，从最后一行开始，条件一旦满足或者T，t命令，将导致分支到带有标号的命令处，或者到脚本的末尾。   
T label   
错误分支，从最后一行开始，一旦发生错误或者T，t命令，将导致分支到带有标号的命令处，或者到脚本的末尾。   
w file   
写并追加模板块到file末尾。   
W file   
写并追加模板块的第一行到file末尾。   
!   
表示后面的命令对所有没有被选定的行发生作用。   
s/re/string   
用string替换正则表达式re。   
=   
打印当前行号码。   
#   
把注释扩展到下一个换行符以前。   
以下的是替换标记   
*   
g表示行内全面替换。   
*   
p表示打印行。   
*   
w表示把行写入一个文件。   
*   
x表示互换模板块中的文本和缓冲区中的文本。   
*   
y表示把一个字符翻译为另外的字符（但是不用于正则表达式）   
  
4. 选项   
-e command, --expression=command   
允许多台编辑。   
-h, --help   
打印帮助，并显示bug列表的地址。   
-n, --quiet, --silent   
  
取消默认输出。   
-f, --filer=script-file   
引导sed脚本文件名。   
-V, --version   
打印版本和版权信息。   
  
5. 元字符集^   
锚定行的开始 如：/^sed/匹配所有以sed开头的行。    
$   
锚定行的结束 如：/sed$/匹配所有以sed结尾的行。    
.   
匹配一个非换行符的字符 如：/s.d/匹配s后接一个任意字符，然后是d。    
*   
匹配零或多个字符 如：/*sed/匹配所有模板是一个或多个空格后紧跟sed的行。   
[]  
匹配一个指定范围内的字符，如/[Ss]ed/匹配sed和Sed。   
[^]  
匹配一个不在指定范围内的字符，如：/[^A-RT-Z]ed/匹配不包含A-R和T-Z的一个字母开头，紧跟ed的行。   
\(..\)  
保存匹配的字符，如s/\(love\)able/\1rs，loveable被替换成lovers。   
&  
保存搜索字符用来替换其他字符，如s/love/**&**/，love这成**love**。    
\<   
锚定单词的开始，如:/\<love/匹配包含以love开头的单词的行。    
\>   
锚定单词的结束，如/love\>/匹配包含以love结尾的单词的行。    
x\{m\}   
重复字符x，m次，如：/0\{5\}/匹配包含5个o的行。    
x\{m,\}   
重复字符x,至少m次，如：/o\{5,\}/匹配至少有5个o的行。    
x\{m,n\}   
重复字符x，至少m次，不多于n次，如：/o\{5,10\}/匹配5--10个o的行。   
6. 实例   
删除：d命令   
*   
$ sed '2d' example-----删除example文件的第二行。   
*   
$ sed '2,$d' example-----删除example文件的第二行到末尾所有行。   
*   
$ sed '$d' example-----删除example文件的最后一行。   
*   
$ sed '/test/'d example-----删除example文件所有包含test的行。   
替换：s命令   
*   
$ sed 's/test/mytest/g' example-----在整行范围内把test替换为mytest。如果没有g标记，则只有每行第一个匹配的test被替换成mytest。   
*   
$ sed -n 's/^test/mytest/p' example-----(-n)选项和p标志一起使用表示只打印那些发生替换的行。也就是说，如果某一行开头的test被替换成mytest，就打印它。   
*   
$ sed 's/^192.168.0.1/&localhost/' example-----&符号表示替换换字符串中被找到的部份。所有以192.168.0.1开头的行都会被替换成它自已加 localhost，变成192.168.0.1localhost。   
*   
$ sed -n 's/\(love\)able/\1rs/p' example-----love被标记为1，所有loveable会被替换成lovers，而且替换的行会被打印出来。   
*   
$ sed 's#10#100#g' example-----不论什么字符，紧跟着s命令的都被认为是新的分隔符，所以，“#”在这里是分隔符，代替了默认的“/”分隔符。表示把所有10替换成100。   
选定行的范围：逗号   
*   
$ sed -n '/test/,/check/p' example-----所有在模板test和check所确定的范围内的行都被打印。   
*   
$ sed -n '5,/^test/p' example-----打印从第五行开始到第一个包含以test开始的行之间的所有行。   
*   
$ sed '/test/,/check/s/$/sed test/' example-----对于模板test和west之间的行，每行的末尾用字符串sed test替换。   
多点编辑：e命令   
*   
$ sed -e '1,5d' -e 's/test/check/' example-----(-e)选项允许在同一行里执行多条命令。如例子所示，第一条命令删除1至5行，第二条命令用check替换test。命令的执 行顺序对结果有影响。如果两个命令都是替换命令，那么第一个替换命令将影响第二个替换命令的结果。   
*   
$ sed --expression='s/test/check/' --expression='/love/d' example-----一个比-e更好的命令是--expression。它能给sed表达式赋值。   
从文件读入：r命令   
*   
$ sed '/test/r file' example-----file里的内容被读进来，显示在与test匹配的行后面，如果匹配多行，则file的内容将显示在所有匹配行的下面。   
写入文件：w命令   
*   
$ sed -n '/test/w file' example-----在example中所有包含test的行都被写入file里。   
追加命令：a命令   
*   
$ sed '/^test/a\\--->this is a example' example<-----'this is a example'被追加到以test开头的行后面，sed要求命令a后面有一个反斜杠。   
插入：i命令   
$ sed '/test/i\\   
new line   
-------------------------' example   
如果test被匹配，则把反斜杠后面的文本插入到匹配行的前面。   
下一个：n命令   
*   
$ sed '/test/{ n; s/aa/bb/; }' example-----如果test被匹配，则移动到匹配行的下一行，替换这一行的aa，变为bb，并打印该行，然后继续。   
变形：y命令   
*   
$ sed '1,10y/abcde/ABCDE/' example-----把1--10行内所有abcde转变为大写，注意，正则表达式元字符不能使用这个命令。   
退出：q命令   
*   
$ sed '10q' example-----打印完第10行后，退出sed。   
保持和获取：h命令和G命令   
*   
$ sed -e '/test/h' -e '$G example-----在sed处理文件的时候，每一行都被保存在一个叫模式空间的临时缓冲区中，除非行被删除或者输出被取消，否则所有被处理的行都将 打印在屏幕上。接着模式空间被清空，并存入新的一行等待处理。在这个例子里，匹配test的行被找到后，将存入模式空间，h命令将其复制并存入一个称为保 持缓存区的特殊缓冲区内。第二条语句的意思是，当到达最后一行后，G命令取出保持缓冲区的行，然后把它放回模式空间中，且追加到现在已经存在于模式空间中 的行的末尾。在这个例子中就是追加到最后一行。简单来说，任何包含test的行都被复制并追加到该文件的末尾。   
保持和互换：h命令和x命令   
*   
$ sed -e '/test/h' -e '/check/x' example -----互换模式空间和保持缓冲区的内容。也就是把包含test与check的行互换。   
7. 脚本   
Sed脚本是一个sed的命令清单，启动Sed时以-f选项引导脚本文件名。Sed对于脚本中输入的命令非常挑剔，在命令的末尾不能有任何空白或文本，如果在一行中有多个命令，要用分号分隔。以#开头的行为注释行，且不能跨行。
------------------
UNIX 目录： 
dev　　里面一般都是一些设备文件，硬盘，USB什么的都在这里
usr全称是UNIX software resource，并不是大多数人想象的user，这里主要存放的是一些软件程序以及这些程序所需要使用的库，当然也会保存一些程序需要的资源文件
opt　　也是用来保存保存的程序文件的，但是保存的这些程序和usr中的一般有些不同，usr里面一般都是保存GNU的，或者是免费，开源的软件，而opt里面保存的都是一些版权比较严格的，或者是套件之类的，主要代表就是sun的java系列和rar系列的软件，当然，其实也有人把这些软件放到usr中，opt目录本来就是方便管理才弄的
etc 保存的是你所安装软件的配置文件，一般这里的配置文件都是全局有效的，即针对本台计算机上的所有用户，每个用户自己的个性化配置文件可以放在自己的home目录下

## 字符串截取

一、Linux shell 截取字符变量的前8位，有方法如下：


复制代码 代码如下:
1.expr substr “$a” 1 8
2.echo $a|awk ‘{print substr(,1,8)}'
3.echo $a|cut -c1-8
4.echo $
5.expr $a : ‘\(.\\).*'
6.echo $a|dd bs=1 count=8 2>/dev/null
二、按指定的字符串截取
1、第一种方法:
${varible##*string} 从左向右截取最后一个string后的字符串
${varible#*string}从左向右截取第一个string后的字符串
${varible%%string*}从右向左截取最后一个string后的字符串
${varible%string*}从右向左截取第一个string后的字符串
“*”只是一个通配符可以不要

例子：


复制代码 代码如下:$ MYVAR=foodforthought.jpg
$ echo ${MYVAR##*fo}
rthought.jpg
$ echo ${MYVAR#*fo}
odforthought.jpg
2、第二种方法：${varible:n1:n2}:截取变量varible从n1到n2之间的字符串。

可以根据特定字符偏移和长度，使用另一种形式的变量扩展，来选择特定子字符串。试着在 bash 中输入以下行：


复制代码 代码如下:$ EXCLAIM=cowabunga
$ echo ${EXCLAIM:0:3}
cow
$ echo ${EXCLAIM:3:7}
abunga
这种形式的字符串截断非常简便，只需用冒号分开来指定起始字符和子字符串长度。

三、按照指定要求分割：
比如获取后缀名


复制代码 代码如下:ls -al | cut -d “.” -f2
应用心得：


复制代码 代码如下:
$MYVAR="12|dadg"
echo ${MYVAR##*|}   #打印分隔符后的字符串
dafa
echo ${MYVAR%%|*} #打印分隔符前的字符串
12
