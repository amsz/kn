# Shell

* [bash manual](https://www.gnu.org/software/bash/manual/bash.html)
* [getops 用法](https://gist.github.com/amsz/11dd6cb0755c4e06f19a990e547b4550)

## 语法

#### 字符串

* `${var:=DEFAULT}` 如果var没有被声明或为空, 以$DEFAULT为值
 * `${#str}`字符串长度
 * `${str}`从$position开始获取子串
 * `${str:position:length}`从$position开始获取长度为$length的子串
* `${str#RE}` 从变量 `$string` 的开头, 删除最短匹配`$RE` 的子串
* `${str##RE}` 从变量 `$string` 的开头, 删除最长匹配`$RE` 的子串
* `${str%RE}` 从变量 `$string` 的结尾, 删除最短匹配`$RE` 的子串
* `${str%%RE}` 从变量 `$string` 的结尾, 删除最长匹配 `$RE` 的子串
* `expr index $string '123'` 子串位子
* `${str/apple/APPLE}` 替换第一次出现的apple
* `${str//apple/APPLE}` 替换所有apple 
* ${str/#apple/APPLE}  如果字符串str以apple开头，则用APPLE替换它

#### if 语句

判断的语法可以查看 `man test`，`test expr` 等价于 `[ expr ]`

```shell
if [ -f $1 ]; then
if [ "$a" = "$b" ]; then
if [[ $# -eq 0 || ! -f $FILE ]]; then
  ...
fi
```

#### for 语句

```shell
for i in 80   22   25   110   8000   23   20   21   3306
for i in {1..10}
for i in /etc/profile.d/*.sh
do
  ...
done

```

#### while 语句

```shell
find . -iname "*.jpg" -type f | while read img ; do
  ...
done
```


#### $ 开头的参数

- `$$` 当前bash的进程号
- `$!`  获取上条命令执行的进程pid
- `$#` 获取参数个数
- `$@` 获取参数列表 （$* 也是获取参数列表）
- `$?` 获取程序返回值（成功为0，错误为其他）
- `${#array[@]}` 获取数组长度（元素个数），array为数组名
- `${array[*]}`  获取数组元素列表
- `${#str}`  获取字符串长度
- `${name:?error message}` 检查一个变量是否存在

#### ! 开头的快捷键

- 上一条命令 `!!` ，等价于 `!-1`
- 执行第 n 个命令 ` !n`
- 执行倒数第 n 个命令 ` !-n` 
- 上一个命令的最后一个参数 `!$` 或者 `Esc + .`
- 上一个命令的第一个参数 `!^`
- 上一个命令的第n-m 参数 `!:n-m`
- 使用 !foo 执行 foo 开头的命令
- 使用  !?foo 执行包含 foo 的命令

#### 小技巧

* telnet 后无法退出 -   `Ctrl-]` -> `q`
* 目录里最大的10个文件 du -s * | sort -n | tail
* 替换上一条命令里的部分字符串 `^old^new`
* 在远程机器上运行一段脚本 `ssh user@server bash  < /mypath/script.sh`
* 比较一个远程文件和一个本地文件 `ssh user@server cat /path/to/remotefile | diff /path/to/localfile –`
* 后台运行一段不终止的程序 `screen -dmS my_name  ping xx.com`，进入 `screen -r my_name` ，退出 `Ctrl-a + d`
* 快速备份一个文件 `cp filename{,.bak}`

- ​
