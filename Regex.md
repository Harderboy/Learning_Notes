# 正则表达式里面比较基础的一些语法  

## grep  

1. 搜寻特定字符串"the",注: n为显示行号  
`grep -n 'the' regular_express.txt`  

2. 反向搜寻特定字符串"the"  
`grep -vn 'the' regular_express.txt`  

3. 取得任意大小写"the"的这个字符串  
`grep -in 'the' regular_express.txt`  

4. 利用括号"[]"搜寻集合字符  

    * 搜索test或taste这两个单词时，发现他们有共同的't?st',所以可以这么搜寻  
    `grep -n 't[ae]st' regular_express.txt`  

    * 如果搜索有"oo"的字符时，则可以使用：  
    `grep -n 'oo' regular_express.txt`  

    * 如果搜索"oo"时不想搜到"oo"前面有"g"的话，我们可以利用反向选择`[^]`来达成:  
   `grep -n '[^g]oo'  regular_express.txt`

    * 如果搜索oo前面不想有小写字符，则：  
    `grep -n '[^a-z]oo' regular_express.txt`  

    * 如果我们要取得有数字的那行,则：  
    `grep -n '[0-9]' regular_express.txt`  

    * 注:大写英文/小写英文/数字 可以使用 "[a-z]/[A-Z]/[0-9]"等方式来书写，也可以写在一起，"[a-zA-Z0-9]"表示要求字符串是数字以及英文,但考虑到语系对编码顺序的影响，因此除了连续编码使用减号[-]外，也可以用"[:lower:]"代替"a-z"以及"[:digit:]"代替"0-9"使用     
    `grep -n '[^[:lower:]]oo' regular_express.txt`  
    `grep -n '[[:digit:]]' regular_express.txt`  

5. 根据行首搜索 
    * 显示行首为"the"的字符串  
    ` grep -n '^the' regular_express.txt`

    * 显示行首是小写字符  
    `grep -n '^[a-z]' regular_express.txt`  

6. 显示行尾为点"."的那一行  
`grep -n '\.$' regular_express.txt`  

7. 显示5-9行数据  
`cat -An regular_express.txt |head -n 10 |tail -n 6`  

8. 显示空白行  
`grep -n '^$' regular_express.txt`  

9. 找出g??d字符串，起头g结束d的四个字符串  
`grep -n 'g..d' regular_express.txt`  

10. "o*"代表空字符(就是有没有字符都可以)或者一个到N个o字符，所以  
`grep -n 'o*' regular_express.txt`  
就会把所有行全部打印出来  

11. "oo*"代表o+空字符或者一个到N个o字符,所以  
`grep -n 'oo*' regular_express.txt`  
就会把o,oo,ooo等的行全部打印出来  

12. "goo*g"代表gog,goog,gooog...等  
`grep -n 'goo*g' regular_express.txt`  

13. 找出含g...g字符串的行  
注:"."代表任意字符,".*"则就代表空字符或者一个到N个任意字符  
` grep -n 'g.*g' regular_express.txt`  

14. 找出含有数字的行  
`grep -n '[0-9][0-9]*' regular_express.txt`  
或  
`grep -n '[0-9]' regular_express.txt`  

15. 查找多个连续字符 
    * 找出含两个o的字符串  
    **注:{}因为在shell里有特殊意义，所以需要加跳脱符\来让其失去意义**  
    `grep -n 'o\{2\}'  regular_express.txt`  

    * 找出g后含2到5个o然后以g结尾的字符串  
    `grep -n 'go\{2,5\}g'  regular_express.txt`  
 
    * 找出g后含2以上的o然后以g结尾的字符串  
    `grep -n 'go\{2,\}g'  regular_express.txt`  

## grep总结：  
  
符号|代表的含义 
:----|:----
^word|表示带搜寻的字符串(word)在行首
word$|表示带搜寻的字符串(word)在行尾
.    |表示1个任意字符
\    |表示转义字符，在特殊字符前加\会将原本的特殊字符意义去除
*    |表示重复0到无穷多个前一个RE(正则表达式)字符
[list] |表示搜索含有list的字符串
[n1-n2]|表示搜索指定的字符串范围,例如[0-9] [a-z] [A-Z]等
[^list]|表示反向字符串的范围,例如[0-9]表示非数字字符，[A-Z]表示非大写字符范围
\\{n,m\\}|表示找出n到m个前一个RE字符 
\\{n\\} |表示n个以上的前一个RE字符  
  
----  
## egrep总结:

---
## sed  

---
## awk  


