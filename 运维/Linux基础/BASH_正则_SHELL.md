# BASH

## BASH中组合键

- `ctrl + alt + F1~F6` tty切换
- `ctrl + u/k` 左右删除
- `ctrl + a/e` 左右跳转
- `ctrl + c` 终止
- `ctrl + d` 退出
- `ctrl + s` 暂停输出
- `ctrl + z` 暂停命令
- `ctrl + q` 恢复
- `ctrl + m` 回车

## BASH中特殊符号

- 正则相关
  - `*` 任意字符
  - `?` 至少一个字符
  - `[abc]` a,b,c中任一个
  - `[0-9]` 任意一个数字
  - `[^a]` 不是a的字符
- 特殊含义
  - `#` 注释
  - `|` 管道
  - `~` 家
  - `\` 转义符
  - `;` 连续
  - `$` 变量引用,前置
  - `&` 后台运行,后置
  - `!` 逻辑非
  - `/` 路径分隔符
  - `>` `>>` `<` `<<` 重定向
  - `'` `"` 引号,双引号保留原意
  - `$()` 指令调用,**等价于两个反引号**
  - `{}` 命令块,PATH等价于${PATH}

## BASH中环境

### login和non-login

- `login` 进入bash,需要密码
- `non-login` x-window进入bash,或者bash再次进入bash

### 系统文件

- `/etc/man_db.conf` 记录man-pages的路径
- `/etc/profile` 需要login-shell来读取
  - `/etc/profile.d/*.sh` centos7的新规范
  - `/etc/locale.conf` 由/etc/profile.d/lang.sh来读取,关联指令locale
  - `/usr/share/bash-completion/completions/*` 由/etc/profile.d/bash_completion.sh读取,涉及tab的自动补全
- `.bash_profile` centos7中的命名,在其他版本中也可能是: .bash_login 或者 .profile
- `.bashrc` 
  - 使用不同的UID,来规范umask和PS1
  - 读取/etc/profile.d中的配置
  - 副本的位置在/etc/skel/.bashrc
- `.bash_history` 指令的历史记录
- `.bash_logout` 用户退出的配置
- 优先级(由低到高)
  - `/etc/profile` -> `/etc/profile.d` -> `.bash_profile` 

### 指令source

用来重新读取指定的配置文件,而不是重新登陆加载读取.

### 登陆信息

- `/etc/issue` 进站信息
- `/etc/motd` 登陆信息

## BASH中变量

### 约束

1. `=` 连接
2. 不能有空格
3. 不能以数字开头
4. 引号和转义符
5. 指令的调用: 两个反引号  或者  `$()` 
6. 扩容 `PATH=$PATH:增加内容`
7. `export var` 环境变量
8. `unset var` 删除

### 读取和声明

- 键盘读取
  - `read var` 读取键盘输入的变量,并命名为var
- 变量声明
  - `declare` 或者 `typeset`
    - `declare -i var` 整数
    - `decalre -a var;var[1]=1;` 数组,下标从1开始;调用方式是`${var[1]}` 
    - `declare -x var=123` 声明后,同时调用export指令
    - `declare -r var=123` 只读;不可修改,不可unset
  - 数组的直接声明方式
    - `arr[1]=1` `arr[2]=2` `arr[3]=3` 

### 删除和替换

- 删除和替换
  - `${变量#关键字}` 左边删除最短的关键字
  - `${变量##关键字}` 左边删除最长
  - `${变量%关键字}` 右边删除最短
  - `${变量%%关键字}` 右边删除最长
  - `${变量/旧/新}` 替换第一个
  - `${变量//旧/新}` 替换所有
- 特殊语法
  - `var=${str-expr}` 
    - str不存在,var为expr
    - str存在,var为expr
    - str为空,var为空
  - `var=${str:-expr}`
    - str不存在,var为expr
    - str存在,var为expr
    - str为空,var为expr
  - 此外还有`+` `:+` `=` `:=` `?` `:?` 

### 环境变量

- `env` 系统变量
  - `HOME` `SHELL` `HISTSIZE` `MAIL` `PATH` `LANG` `RANDOM`
  - 相关的指令`locale` 和 `locale -a` 
- `set` 系统+用户变量
  - 直接使用该指令,可查看
  - 另外改指令可设置tty,仅作了解

## 重定向和管道

- 重定向
  - `<` `<<` 输入
  - `>` `>>` 输出
  - `2>` `2>>` 错误输出
  - `&>` 全部输出
- 管道
  - `|` 输入->输出->输入
  - `cmd1 && cmd2` 正确执行的话,继续
  - `cmd1 || cmd2` 错误执行的话,继续

## 文本处理指令扩展

### cut文本分割

- `echo $PATH|cut -d ':' -f 5` 以:分割,取第5个
- `echo $PATH|cut -c 12-` 取12至结束的字符

### grep文本过滤

	- `grep -in pass file` 过滤pass关键字;忽略大小写,显示行号
	- `grep -iv pass file` 反向过滤
	- `grep -i -A3 pass file`  添加显示匹配后的后3行
	- `grep -i -B3 pass file` 添加显示匹配后的前3行

### sort排序

### wc统计

### uniq去重

### tee双向重定向

- `cat /etc/profile |tee 1.txt` 输入到文件1.txt,并在屏幕中输出

### 字符转换

- `tr` 
  - `echo $PATH|tr -d 'bin'` 删除bin单词
  - `echo $PATH|tr -s 'bin' 'abc'` 用abc替代bin
- `col` 
  - `cat /etc/profile |col -x` tab对等转换为空格
- `expand` 
  - `expand file` tab转换为空格

### 字符拼接

- `join` 
  - `join -t ':' /etc/passwd /etc/shadow` 对等的行,加入符号:.然后拼接在一起
  - 加入参数`-i` 表示忽略大小写
- `paste` 
  - `paste /etc/passwd /etc/shadow` 对等的行,加入tab.然后拼接在一起

### split分割

### xargs批量输入转换参数

## 杂项

- shell查看
  - `cat /etc/shells` 合法的shell
  - `type pwd` shell的类型
- 其他
  - `echo PATH` 等价 `echo ${PATH}` 
  - `ll /lib/modules/$(uname -r)/kernel` 等价反引号的格式
  - `ulimit -a` 查看多用户的限制
- 别名和历史
  - `alias` 和 `unalias` 别名
  - `history` `$HISTSIZE` 指令历史
- 符号`-`

  - 符号`-` 的用途.可替代`stdin` `stdout`
  - `tar cvf - /home | tar xvf - -C /root` 这就可以省略压缩包的名称了

# 正则

## 特殊符号

- `[:alnum:]` 字母和数字*
- `[:alpha:]` 字母*
- `[:blank:]` 空格或tab
- `[:cntrl:]` CR,LF,Tab,Del等控制键
- `[:digit:]` 数字*
- `[:graph:]` 除空格和tab的其他所有键
- `[:lower:]` 小写字母*
- `[:upper:]` 大写字母*
- `[:print:]` 任何可打印的字符
- `[:punct:]` 标点符号
- `[:space:]` 会产生空格的字符或键位
- `[:xdigit:]` 16进制数字

## 基础正则

- 特定字符`''` `""`   
  - `grep -i 'path' 1.txt` 
- 集合字符`[abc]` `[0-9]` `[^a]bc` `[[:lower:]]`
  - `grep -i [abc] 1.txt`    `[]` 集合
  - `grep -i [0-9] 1.txt`     `[-]` 范围
  - `grep -i [0-9] 1.txt`    `[^]` 逻辑非  
  - `grep [[:lower:]] 1.txt`   `[:lower:]` 小写字母
- 开头结尾 `^$`
  - `grep '^path' 1.txt`   `^` 表示开头
  - `grep 'd$' 1.txt`   `$` 表示结尾
  - `grep 'd$' 1.txt`   `^$` 表示空白行
- 通配符`.` `*`
  - `grep 'pa.h' 1.txt`   `.` 表示任意一个字符
  - `grep 'p*h' 1.txt`   `*` 表示任意个字符
- 连续和转义符 `{}` `\`
  - `grep 'o\{2\}' 1.txt`   `{}` 出现的次数

## 扩展正则

- `+` 至少1个
  - `grep -E 'o+' 1.txt` 
- `?` 0个或1个
  - `egrep 'o?' 1.txt` 
- `|` 逻辑或
  - `grep -E 'pa|oo' 1.txt`  
- `()` 群组
  - `egrep '(pa|oo)' 1.txt` 
- `()+` 多个群组
  - `egrep '(pa|oo)+' 1.txt` 

## 高级文本处理工具

### sed行处理

- 参数
  - `-n` 取消静默模式
  - `-i` 直接编辑
  - `-e` 脚本

- 替换`s`
  - `cat 1.txt |sed -n 's/path/sed/p'` **行**首次匹配替换,并打印
  - `cat 1.txt |sed -n 's/path/sed/gp'` 全局匹配替换,并打印
  - `cat 1.txt |sed -n '8s/path/sed/p'` 第8行首次匹配替换,并打印
  - `cat 1.txt |sed -n '1,8s/path/sed/gp'` 第1到8行全局匹配替换,并打印
- 添加`a`
  - `cat 1.txt |sed -n '/abc/a sed'` 在首次匹配处的下行添加
  - `cat 1.txt |sed -n '/abc/ag sed'` 在全局匹配处的下行添加
- 删除`d`
  - `cat 1.txt |sed -e '/path/d'` 删除匹配的行
  - `cat 1.txt |sed -e '4,$d'` 从第4行删除至末尾
- 直接修改
  - `sed -i 's/path/sed/g' 1.txt` **重要,慎用**
- 正则
  - `cat 1.txt |sed -n 's/^abc/sed/gp'`   `^` 不以开头
  - `cat 1.txt |sed -n 's/abc/sed/gp;s/bcd/sed/gp'`   `;` 多次匹配
  - `cat 1.txt |sed -n 's/abc/&sed/gp'`   `&` 追加
  - `cat 1.txt |sed -n '/a/,/b/s/abc/sed/gp'`   `,` 范围,  在a和b之间匹配替换
  - `cat 1.txt |sed -n '{n;s/abc/sed/gp}'`   `{n;}` 匹配的下行替换
  - `cat 1.txt |sed -n 's/abc/sed/gp;2q'`   `;2q` 匹配到第2行停止
  - `cat 1.txt |sed -n 's/(a|b)/sed/gp'`   `(|)` 群组

- 获取上级路径
  - `pwd|sed 's@\(/.*/\)[^/]\{1,\}/\?@\1@'`
  - `pwd|sed -r 's@(/.*/)[^/]+/?@\1@'`

### awk栏处理

|          |                           |
| -------- | ------------------------- |
| `NF`     | 一行,栏位的总数           |
| `NR`     | 当前行                    |
| `FS`     | 栏位的分隔符号,默认是空格 |
| `BEGIN`  | 开始                      |
| `print`  | 打印                      |
| `><=!等` | 支持逻辑运算符            |
| `$1`     | 第1个栏位                 |

- `{print $1 "\t"}` 打印第1个栏位
  - `last|tail -n 5|awk '{print $1 "\t"}'`   
- `{print $1 "\t" NF "\t" NR}` 打印栏位的总数和行号
  - `last|tail -n 5|awk '{print $1 "\t" NF "\t" NR}'` 
- `'BEGIN {FS=":"} $3<10 {print $1 "\t"}'` 更改默认分栏符号
  - `cat /etc/passwd|awk 'BEGIN {FS=":"} $3<10 {print $1 "\t"}'` 

### printf格式化打印

`printf '%s\t \n' $(last|tail -n 5)`    

格式化打印

- `%s` 字符串
- `%i` 整数
- `%f` 浮点数
- `\t` 制表符
- 具体`cman printf` 

### 文件比对工具

- `diff -Bi file1 file2` 忽略大小写和空白行差异,比对文件

  - `N 目录比较中,其中目录未找到对应文件,视为空文件` `a 比较所有文件` `u 统一格式输出` `r 递归` 
  - `diff -Naur 1.txt 2.txt > diff.patch` 

- `cmp file1 file2` 列出file1中,比对file2中的不同

- `patch` 结合diff使用,更新或还原旧文件,手动安装`yum install patch`  

  ```sh
  diff -Naur 1.txt 2.txt >diff.patch
  patch -p0 <diff.patch #更新
  patch -R -p0 <diff.patch #还原
  # -R 还原
  # -pN 取消多少层目录,比较
  ```

### pr虚拟打印

- 打印带有标题,时间,页码的屏幕输出
- `pr 1.txt|less` 

# SHELL

## 规范

**首行必须指定shell的具体路径,如`#!/bin/bash`**

## 执行方式

- `sh` 或者 `./`  
  这种执行方式,是新建bash来执行的,相当于non-login.执行结束后,会返回到父bash.
- `source`  
  这种执行方式,是在父bash中执行的.也就是当前的bash中.

## test测试

- `test -e 1.txt && echo true` 测试文件是否存在
  - 文件的判断
    - `-e` 文件是否存在*
    - `-f` 文件是否存在,且是否是文本*
    - `-d` 文件是否存在,且是否是目录*
    - `-b` 文件是否存在,且是否为block设备
    - `-c` 文件是否存在,且是否为character设备
    - `-p` 是否存在,且是否是FIFO设备
    - `-S` 是否存在,且是否是socker设备
    - `-L` 是否存在,且是否是链接
  - 权限的判断
    - `-r` 是否存在,是否有读取权限
    - `-w` 是否存在,是否有写入权限
    - `-x` 是否存在,是否有执行权限
    - `-u` 是否存在,且是否有SUID权限
    - `-g` 是否存在,且是否有SGID权限
    - `-k` 是否存在,是否有SBIT权限
    - `-s` 是否存在,且不为空白的
  - 文本比较
    - `-nt` 是否是最新的
    - `-ot` 是否是旧的
    - `-ef` 是否指向同一个inode,即硬链接
  - 整数的比较
    - `-eq` 相等
    - `-ne` 不相等
    - `-gt` 大于
    - `-lt` 小于
    - `-ge` 大于等于
    - `-le` 小于等于
  - 字符串的判断
    - `-z` 是否为空
    - `==`
    - `!=` 
  - 多条件判断
    - `-a` 逻辑与
    - `-o` 逻辑或
    - `!` 逻辑非
- `[ -e 1.txt ];echo $?` 等价于test的形式
  - 符号`[]` 两边必须要有空格

## shell中预设变量

- `$#` 代表参数的总个数
- `$@` `$*` 代表所有参数的总数
- `shift` 参数的偏移量,声明一次,可以理解为把第一个参数给删除掉.

```sh
#!/bin/bash
echo $#
echo $@
shift
echo $#
echo $@
# 输出结果如下
# 4
# 1 2 3 4
# 3
# 2 3 4
```

## 条件分支

### if语句

```sh
#!/bin/bash
if true;then
  echo 123
fi
```

```sh
#!/bin/bash

if false;then
  echo 123;
else
  echo 456;
fi
```

```sh
#!/bin/bash
num=3;
if [ $num -gt 10 ];then
  echo 1;
elif [ $num -gt 20 ];then
  echo 2;
elif [ $num -gt 30 ];then
  echo 3;
else
  echo 4;
fi
```

### case语句

```sh
#!/bin/bash
case $1 in
  1)
  echo 1;
  ;;
  2)
  echo 2;
  ;;
  3)
  echo 3;
  ;;
  *)
  echo 4;
  ;;
esac
```

## 循环分支

### while语句

```sh
#!/bin/bash
declare -i num=10;
while [[ $num -gt 1 ]]
do
  echo $num;
  num=$num-1;
done
# while循环语句,表示条件不满足的时候,才会终止循环.
```

### util语句

```sh
#!/bin/bash
declare -i num=10;
until [[ $num -gt 20 ]]
do
  echo $num;
  num=$num+1;
done
# util循环语句,表示条件成立的时候,就终止循环.
```

### for语句

```sh
#!/bin/bash
for var in 1 2 3 4 5 6 7 8 9 10
do
  echo $var
done
```

```sh
#!/bin/bash
for ((i=1;i<10;i=i+1))
do
  echo $i
done
```

## 函数function

```sh
#!/bin/bash
function func(){
  echo 'function exec';
}
func;
```

## 脚本的跟踪

`sh -x test.sh` 参数x表示把使用到的脚本,输出到屏幕上

