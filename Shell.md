#### 1. Shell脚本执行
> 将命令行中的多个命令汇总放入脚本中一起执行
* 调用解释器执行，实际上是在当前Shell环境中启动一个子进程执行，以下两者略有差异。
```Shell
sh test.sh
# 下面的更推荐
bash test.sh
```

* 直接输入文件名运行。若脚本没有可执行权限，使用`chmod +x test.sh`
```Shell
# 也是新建一个shell执行脚本
./test.sh  
```

* `#!`指定解释器执行脚本
  * 执行命令时显示指定的解释器优先级高于`#!`
  * 默认是bash

#### 2. 基本语法
```Shell
# 定义变量，等号两边不要加空格
str="hello"

# 引用变量，加大括号更规范、更安全
$str
${str}

# 单引号内容为字面值常量，双引号遇到变量会自动转为变量的值
num=1
echo "${num}"   # 输出 1
echo '${num}'   # 输出 ${num}

# 默认所有变量都是全局作用域，即使变量定义在函数中也是如此
# 函数内的变量需要加local关键字，才不会污染全局作用域

# bash中的变量分为普通变量和环境变量。区别在于当前shell打开subshell时，环境变量会被subshell继承。有以下两种写法：
export num=1
declare -x num=1

# 特殊变量
$0          # 脚本的名字，可能是相对路径
$1...$10    # 用于表示参数，$1表示第一个参数
$#          # 表示参数的个数
$?          # 上一个命令的执行结果，0表示正常，非0表示出现错误
```

#### 3. 基础语法
```Shell
# 条件判断有[和[[两种写法
# 更建议
str="1"
if [[ $str = "1" ]]; then
    echo "equal"
fi
# 或者
if [ $str = "1"]; then 
    echo "equal"
fi

# 数字判断：单等号、双等号或`-eq`关键字
num=1
[[ $num = 1 ]] && echo "yes" || echo "not"
[[ $num == 1 ]] && echo "yes" || echo "not"
[[ $num -eq 1 ]] && echo "yes" || echo "not"
# 大于等于和小于等于必须用`-ge`和`-le`
# 字符串判断类似

# 字符串模式匹配
str="hello"
[[ $str == he* ]] && echo "yes" || echo "not"

# 文件判断
if [[ -e file ]]    # 判断是否存在，不限制类型
if [[ -f file ]]    # 必须是普通类型的文件，不能是文件夹
if [[ -d file ]]    # 必须是文件夹，不能是文件

# if语句
if [[ expression1 ]]; then
    echo "expression1"
elif [[ expression2 ]]; then 
    echo "expression2"
else    
    echo "else"
fi

# 循环
for f in `ls`; do
    echo $f
done
```

#### 4. 命令串联
> shell中的管道允许不同脚本、命令之间互相传递数据
```Shell
function before {
    echo 'output'
}

function after {
    read in
    echo "Read from pipeline: "${in}
}

before | after
# 输出结果为：
# Read from pipeline: output
```

> 重定向是管道的孪生兄弟，最简单的使用场景就是把原本输出到屏幕的内容重定向到文件中。*nix系统中有三种特殊的文件描述符，0表示标准输入，1表示标准输出，2表示错误输出，1和2一般表示屏幕。
```Shell
# 把标准输出和重定向到success文件，把错误信息输出到fail文件
ls exist.sh not_exist.sh 1>success 2>fail

# 2>&1 让错误输出使用标准输出的重定向方式
ls exist.sh not_exist.sh 1>success 2>&1
# 等于
ls exist.sh not_exist.sh 1>success 2>success

# 过滤输出：使用命令的功能，但不希望产生任何输出
command > /dev/null 2>&1
# 可简写为
command &> /dev/null
# 关闭某种输出
command >&-
```

> 函数返回值
```Shell
# 调用函数返回结果不是return的内容，而是echo的内容，return内容可通过`$?`特殊变量获取
function foo {
    echo 'output'
    return 1
}

a=`foo`
echo $?     # 输出1
echo $a     # 输出output
```

#### 5. 错误处理
> 可以参考阮一峰老师的bash脚本set命令教程
```Shell
set -u      # 遇到未定义变量时抛出错误，而不是忽略
echo $bar

set -e      # 强制报错时退出脚本
```

#### 6. 系统命令
> `grep` 详细参数可使用`man grep`查阅
* `grep 'content' file.txt` 搜索某个文件中的内容
* `echo 'something' | grep 'some'` 从标准输入中搜素内容

> `xargs` 将换行符、空格、制表符、EOF等符号作为分隔符

> `sed` 文本替换，文本匹配后处理

> `awk` 文本的结构化处理