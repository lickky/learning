#使用Shell提高工作效率

本文介绍一些常见或隐晦的Shell功能和命令来帮助我们提高工作效率。

## 测试环境

目前比较常用的Shell是bash,所以下文中说的Shell就是指bash。

OS:     MacOSX 10.7

Shell：GNU bash, version 4.2.45o

## 1.数据批量处理

### 1.1大括号展开表达式: 

在Shell中我们可以使用大括号"{}"表达式生成一些有规律的数据，比如：

    输出1到5

        $ echo {1..5}GNU bash, version 4.2.45
        1 2 3 4 5
    
    拼接字符串

        $ echo {1,5,10}.txt
        1.txt 5.txt 10.txt

    组合拼接字符串
        echo {1,2}button{_cn,_en}.txt
        1button_cn.txt 1button_en.txt 2button_cn.txt 2button_en.txt

将生成的字符串作为命令行工具的输入参数则可以完成很多复杂的文件操作，
下面以批量改文件名为例：

    新建两个文件分别为：1button_cn.txt 2button_cn.txt

        $ touch {1,2}button{_cn}.txt

    通过mv命令重名文件名，把_cn换成_en

        $ echo {1,2}button{_cn,_en}.txt | xargs -n 2 mv

最后一条命令中，xargs命令用来拆分输入参数列表的工具，-n 2 表示一次读取2个参数传给后面的mv命令执行，所以实际上，这条命令等效于以下两条命令：

        $ mv 1button_cn.txt 1button_en.txt
        $ mv 2button_cn.txt 2button_en.txt



## 2.文件的定位与查找

处理文件是日常操作频率最高的工作，Shell本身提供多种功能方便你定位文件，查找文件。

### 2.1多使用tab键自动补全快速定位文件

在Shell中敲击tab键，这时Shell会尝试补全你输入的内容。根据输入内容的不同Shell会智能的判断要补充的内容然后补全输入，比如：

在什么也不输入的情况下连续敲击两下tab键，Shell会提示将列出所有系统的可执行文件名：
    
    不输入任何东西，连续2下敲击tab键
    $
    Display all 2350 possibilities? (y or n)

如果输入非路径的字符，Shell会认为你要输入命令而尝试查找文件名与之相识的系统的可执行文件补全。

如果输入路径符号如./，那么shell会认为你在输入路径，然后根据已输入的路径进行匹配查找。

通过自动补全，我们可以快速的定位到我们想要找的文件，尤其是在当前目录下有很多文件名相似的文件的情况下定位文件。

### 2.2使用通配符查找

在shell中可以使用统配符进行模糊查找，比如：

    删除当前目录下所有png文件
    $ rm *.png

    删除当前目录下所有_hv.png形式的文件
    $ rm *_hv.png

使用通配符可以减少我们大量的重复劳动，提高工作效率。

<p><strong>shell常见通配符：</strong></p>
<table border="0" cellspacing="1" cellpadding="4" width="550" bgcolor="#666666">
<tbody>
<tr>
<td width="154" bgcolor="#eeeeee"><strong>字符</strong></td>
<td width="145" bgcolor="#eeeeee"><strong>含义</strong></td>
<td width="223" bgcolor="#eeeeee"><strong>实例</strong></td>
</tr>
<tr>
<td bgcolor="#ffffff">* </td>
<td bgcolor="#ffffff">匹配 0 或多个字符 </td>
<td bgcolor="#ffffff">a*b&nbsp; a与b之间可以有任意长度的任意字符, 也可以一个也没有, 如aabcb, axyzb, a012b, ab。 </td>
</tr>
<tr>
<td bgcolor="#ffffff">? </td>
<td bgcolor="#ffffff">匹配任意一个字符</td>
<td bgcolor="#ffffff">a?b&nbsp; a与b之间必须也只能有一个字符, 可以是任意字符, 如aab, abb, acb, a0b。 </td>
</tr>
<tr>
<td bgcolor="#ffffff">[list]&nbsp; </td>
<td bgcolor="#ffffff">匹配 list 中的任意单一字符 </td>
<td bgcolor="#ffffff">a[xyz]b&nbsp;&nbsp; a与b之间必须也只能有一个字符, 但只能是 x 或 y 或 z, 如: axb, ayb, azb。 </td>
</tr>
<tr>
<td bgcolor="#ffffff">[!list]&nbsp; </td>
<td bgcolor="#ffffff">匹配 除list 中的任意单一字符 </td>
<td bgcolor="#ffffff">a[!0-9]b&nbsp; a与b之间必须也只能有一个字符, 但不能是阿拉伯数字, 如axb, aab, a-b。 </td>
</tr>
<tr>
<td bgcolor="#ffffff">[c1-c2] </td>
<td bgcolor="#ffffff">匹配 c1-c2 中的任意单一字符 如：[0-9] [a-z]</td>
<td bgcolor="#ffffff">a[0-9]b&nbsp; 0与9之间必须也只能有一个字符 如a0b, a1b... a9b。 </td>
</tr>
<tr>
<td bgcolor="#ffffff">{string1,string2,...} </td>
<td bgcolor="#ffffff">匹配 sring1 或 string2 (或更多)其一字符串 </td>
<td bgcolor="#ffffff">a{abc,xyz,123}b&nbsp;&nbsp;&nbsp; a与b之间只能是abc或xyz或123这三个字符串之一。 </td>
</tr>
</tbody>
</table>


### 2.3find 命令 

        查找plist文件中包含某个abc字符串:
        find . -name "*.plist" -exec grep 'abc' {} \; -print
