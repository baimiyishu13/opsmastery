## 📒Doc

### 命令

bash：命令处理器（bash 解释器）



#### chmod

`chmod` 命令用于更改文件或目录的权限。Linux文件和目录权限分为三个类别：所有者（Owner）、群组（Group）和其他用户（Others），每个类别都有读（r）、写（w）和执行（x）权限。

语法：

```
chmod [选项] 权限 文件或目录
```

常见的选项和权限表示方式：

- `+`：添加权限。
- `-`：删除权限。
- `=`：设置权限。
- `r`：读权限。
- `w`：写权限。
- `x`：执行权限。

#### chown

`chown` 命令用于更改文件或目录的所有者和群组。

语法：

```
bashCopy code
chown [选项] 用户:组 文件或目录
```

示例：

1. **更改所有者和群组**：将文件 `file.txt` 的所有者更改为 `newowner`，群组更改为 `newgroup`：

   ```
   chown newowner:newgroup file.txt
   ```

2. **仅更改所有者**：将文件 `file.txt` 的所有者更改为 `newowner`，但群组保持不变：

   ```
   chown newowner file.txt
   ```

3. **仅更改群组**：将文件 `file.txt` 的群组更改为 `newgroup`，但所有者保持不变：

   ```
   chown :newgroup file.txt
   ```





#### crontab

Crontab 命令

+ `crontab -e` : 创建或编辑定时任务表
+ `crontab -l` : 列出任务表

 Cron 计划或 Cron 表达式

```
* * * * * command_to_execute
# 分 时 日 月 周
```

有一些特殊字符，如 *、/、- 和 ,，用于定义更复杂的定时任务

1. `*`：通配符，表示匹配任何值。例如，`* * * * *` 表示每分钟都运行一次任务。
2. `/`：用于定义间隔。例如，`*/15 * * * *` 表示每隔 15 分钟运行一次任务。
3. `-`：用于定义范围。例如，`0 9-17 * * *` 表示在每天的 9 点到 17 点之间每小时运行一次任务。
4. `,`：用于列出多个值。例如，`30 8,12,16 * * *` 表示在每天的早上 8 点、12 点和下午 4 点 30 分运行任务。

 Cron 表达式示例 在 Linux 中创建作业示例



---

grep：文本过滤工具

sed：文本编辑工具

awk：报告生成工具 （格式化文本）

#### find

查找命令语法 

`find` 是一个强大的命令行工具，用于在文件系统中搜索文件和目录。以下是 `find` 命令的一般语法：

```
find 起始目录 选项 表达式

find /name -name <file_name>
```

- `起始目录`：指定搜索的起始目录。可以是相对路径或绝对路径。
- `选项`：可选，用于设置 `find` 命令的不同选项。
- `表达式`：用于定义搜索条件和操作的表达式。

以下是一些常用的 `find` 命令选项和表达式：

**选项：**

- `-name`：按文件或目录的名称进行匹配。例如，`-name "*.txt"` 会匹配所有以 `.txt` 扩展名结尾的文件。
- `-iname` ：忽略文件名大小写
- `-type`：按文件类型进行匹配。可以是 `f`（普通文件）、`d`（目录）、`l`（符号链接）等。
- `-mtime`：按文件的修改时间进行匹配。例如，`-mtime -7` 匹配最近7天内修改过的文件。
- `-size`：按文件的大小进行匹配。例如，`-size +1M` 匹配大于1MB的文件。
- `-user`: 给定用户的文件
- `-inum`根据inode号查找文件
- ` -newer`: 匹配那些创建时间（或修改时间）比 `last.txt` 文件新的文件
-  `-newerct` 比较文件的创建时间（ctime）而不是修改时间
-  `-mindepth` 和 `-maxdepth` : 查找的层级深度

**表达式：**

- `-exec`：执行指定的命令来处理匹配的文件。例如，`-exec rm {} \;` 删除匹配的文件。
- `-print`：默认动作，将匹配的文件名打印到标准输出。
- `-and`、`-or`、`-not`：用于组合多个搜索条件。例如，`-name "*.txt" -and -mtime -7` 匹配最近7天内修改过的以 `.txt` 结尾的文件。

案例1：如何根据文件大小搜索文件？

```sh
➜  kube-prometheus-release-0.8 find manifests -size +40k
manifests/grafana-dashboardDefinitions.yaml
```

+ M for MB
+ K for KB
+ G for GB

案例2：如何仅查找给定路径中的文件或目录？

type:

+ f : file
+ d: dir
+ l : 链接
+ b：块设备

```sh
➜  kube-prometheus-release-0.8 find ./manifests -type d
./manifests
./manifests/setup

➜  kube-prometheus-release-0.8 find ./manifests -type f
./manifests/prometheus-adapter-deployment.yaml
./manifests/prometheus-clusterRole.yaml
```

案例3：如何根据文件名搜索文件？

```sh
➜  kube-prometheus-release-0.8 find . -name grafana-deployment.yaml
./manifests/grafana-deployment.yaml
```

案例4：如何在搜索时忽略文件名的大小写？

```sh
➜  kube-prometheus-release-0.8 find . -iname kubernetes-servicemonitorapiserver.yaml
./manifests/kubernetes-serviceMonitorApiserver.yaml
```



案例5：如何仅搜索给定用户的文件？

```sh
➜  kube-prometheus-release-0.8 find . -user lelema
.
./experimental
```

案例6；如何根据inode号查找文件？

```sh
➜  kube-prometheus-release-0.8 ll -i | tail -1
21013135 drwxr-xr-x   3 lelema  staff    96B  3 21  2022 tests
➜  kube-prometheus-release-0.8 find . -inum 21071059
./1.txt
```

案例7：如何根据编号搜索文件。链接？ 

```sh
➜  kube-prometheus-release-0.8 find . -links 3
./experimental
./tests
```

案例8: 如何根据权限搜索文件？ 

```sh
➜  kube-prometheus-release-0.8 find . -perm 0777
./test.sh
```

案例9：有执行权限的文件

```sh
➜  kube-prometheus-release-0.8 find . -type f -perm +100
./build.sh
./scripts/monitor
```

+ `+100` 有执行权限
+ `-100`: 无执行权限

案例10：如何查找所有以字母sh结尾的的文件？

```sh
➜  kube-prometheus-release-0.8 find . -type f -name "*.sh"
./build.sh
./scripts/monitoring-deploy.sh
./scripts/minikube-start.sh
./scripts/minikube-start-kvm.sh
./scripts/generate-schemas.sh
./scripts/generate-versions.sh
./test.sh
```



案例11: 如何搜索last.txt文件之后创建的所有文件？ 

```sh
➜  kube-prometheus-release-0.8 find . -type f -newer test.sh
./1.txt
```



案例12: 如何查找给定目录下的所有空文件？ 

```sh
➜  kube-prometheus-release-0.8 find . -type f -size 0
./hack/jsonnet-docker-image
./examples/etcd-client.crt
./examples/etcd-client.key
./examples/etcd-client-ca.crt
./null.txt

# 深度为1
➜  kube-prometheus-release-0.8 find . -maxdepth 1 -size 0
./null.txt
```



案例13：仅查找深度2的空文件

```sh
➜  kube-prometheus-release-0.8 find . -mindepth 2 -maxdepth 2 -type f -size 0

./hack/jsonnet-docker-image
./examples/etcd-client.crt
./examples/etcd-client.key
./examples/etcd-client-ca.crt
```



案例14: 查找深度为2，最近5天修改的文件

```sh
➜  kube-prometheus-release-0.8 find . -type f -maxdepth 2 -mindepth 2 -mtime -5
./manifests/test.sh
```



案例15：如何查找所有空文件并将其删除？

```sh
➜  kube-prometheus-release-0.8 find . -maxdepth 1 -size 0 -exec rm {} \;
```



案例16: 如何搜索所有大小在1-50MB之间的文件？

```sh
➜  kube-prometheus-release-0.8 find . -type f -size +1M -size -50M
./manifests/grafana-dashboardDefinitions.yaml
```



案例17: 如何在 Linux 中搜索 15 天前的文件？

```sh
➜  kube-prometheus-release-0.8 find . -type f -mtime +15
```





#### grep

参数：

+ `-i`：忽略大小写
+ `-r` : 递归
+ `-R` 递归搜索目录下的文件，并跟随符号链接
+ `-h`: 不显示文件名
+ `-E`: 扩展正则表达式
+ `-v`:取反
+ `-w` : 精确匹配搜索，只匹配整个单词而不是匹配部分单词
+ `-c` : 统计次数
+ `-n` : 显示行号
+ `-e` : 指定多个关键字
+ `-l`：用于仅输出包含匹配关键字的文件名
+ `-F` 选项：它表示 `grep` 应该以固定字符串
+ `-f 1.txt` 选项：它指定了一个文件
+ `-A -B -C `: 后、前、前后几行
+ `-q` ：不显示输出，返回状态码
+ `-s`: 忽略错误

常用正则：

+ `\s` : 空格
+ `+` 一个或多个
+ `^\s*`:  0到多个空格开始
+ `($|#)` ：空行或#

案例 1：在 Linux 中使用 grep 命令搜索时忽略大小写 

```sh
➜  kube-prometheus-release-0.8 grep -rihE "image:\s+" manifests/

        image: directxman12/k8s-prometheus-adapter:v0.8.4
        image: grafana/grafana:7.5.4
        image: k8s.gcr.io/kube-state-metrics/kube-state-metrics:v2.0.0
```

+ `-i`：忽略大小写
+ `-r` : 递归
+ `-h`: 不显示文件名
+ `-E`: 扩展正则表达式
+ `\s` : 空格
+ `+` 一个或多个

去除前面的空格

```sh
➜  kube-prometheus-release-0.8 grep -rihE "image:\s+" manifests/ | awk -F ' '  '{print$1,$2}'

image: directxman12/k8s-prometheus-adapter:v0.8.4
image: grafana/grafana:7.5.4
image: k8s.gcr.io/kube-state-metrics/kube-state-metrics:v2.0.0
image: quay.io/brancz/kube-rbac-proxy:v0.8.0
image: quay.io/brancz/kube-rbac-proxy:v0.8.0
```



案例 2：在 Linux 中使用 grep 命令搜索除给定模式/关键字之外的所有内容

```sh
➜  kube-prometheus-release-0.8 grep -vE '^\s*($|#)' build.sh

set -e
set -x
set -o pipefail
PATH="$(pwd)/tmp/bin:${PATH}"
rm -rf manifests
mkdir -p manifests/setup
jsonnet -J vendor -m manifests "${1-example.jsonnet}" | xargs -I{} sh -c 'cat {} | gojsontoyaml > {}.yaml' -- {}
find manifests -type f ! -name '*.yaml' -delete
rm -f kustomization
```

+ `-v`:取反
+ `^\s*`:  0到多个空格开始
+ `($|#)` ：空行或#



案例 3：打印多少个在 Linux 中使用 grep 命令在文件中出现给定关键字的次数（计数） 

```sh
➜  kube-prometheus-release-0.8 grep -cw 'echo' test.sh
12
```

+ `-w` : 精确匹配搜索，只匹配整个单词而不是匹配部分单词
+ `-c` : 统计次数

案例 4：在 Linux 中使用 grep 命令搜索文件中给定关键字的精确匹配 

```
-w
```

案例 5：打印行号。在 Linux 中使用 grep 命令在文件中给定关键字的匹配项 

```sh
➜  kube-prometheus-release-0.8 grep -rhEn '^\s*image:\s+' manifests/
38:        image: directxman12/k8s-prometheus-adapter:v0.8.4
30:        image: grafana/grafana:7.5.4
```

+ `-n` : 显示行号

案例 6：在 Linux 中使用 grep 命令在多个文件中搜索给定关键字 

```sh
 ➜  kube-prometheus-release-0.8 grep  "rm" build.sh test.sh
build.sh:rm -rf manifests
build.sh:rm -f kustomization
test.sh:    rm -rf "test.jsonnet"
```

案例 7：在 Linux 中使用 grep 命令在多个文件中搜索给定关键字时抑制文件名 

```sh
➜  kube-prometheus-release-0.8 grep -h "rm" build.sh test.sh
rm -rf manifests
rm -f kustomization
    rm -rf "test.jsonnet"
```

案例 8：在 Linux 中使用 grep 命令在一个文件中搜索多个 关键字

```sh
➜  kube-prometheus-release-0.8 grep -e "echo" -e "rm" build.sh test.sh
build.sh:rm -rf manifests
build.sh:rm -f kustomization
test.sh:    echo "Testing: ${i}"
test.sh:    echo ""
```

+ `-e` : 指定多个关键字

案例 8：在多个文件中使用 grep 搜索多个关键字Linux 中的命令

```sh
➜  kube-prometheus-release-0.8 grep -e "echo" -e "rm" build.sh test.sh
```

案例 9：在 Linux 中使用 grep 命令仅打印与给定关键字匹配的文件名 

```sh
➜  kube-prometheus-release-0.8 grep -rl 'replicas' manifests/*
manifests/alertmanager-alertmanager.yaml
manifests/blackbox-exporter-deployment.yaml
manifests/grafana-dashboardDefinitions.yaml
manifests/grafana-deployment.yaml
```

+ `-l`：用于仅输出包含匹配关键字的文件名

案例 10：在 Linux 中使用 grep 命令从文件中获取关键字/模式并与另一个文件匹配

```sh
➜  kube-prometheus-release-0.8 cat 1.txt
rm
if
for
else
➜  kube-prometheus-release-0.8 grep -Ff 1.txt test.sh
# only exit with zero if all commands of the pipeline exit successfully
for i in examples/jsonnet-snippets/*.jsonnet; do
    rm -rf "test.jsonnet"
for i in examples/*.jsonnet; do
```

+ `-F` 选项：它表示 `grep` 应该以固定字符串
+ `-f 1.txt` 选项：它指定了一个文件

案例 11：在 Linux 中使用 grep 命令打印以给定关键字开头的匹配行

```sh
➜  kube-prometheus-release-0.8 grep "^rm" build.sh
rm -rf manifests
rm -f kustomization
```

+ `^`：以什么开始

案例 12：在 Linux 中使用 grep 命令打印以给定关键字结尾的匹配行

```sh
➜  kube-prometheus-release-0.8 grep "dir$" build.sh
# Make sure to start with a clean 'manifests' dir
```

+ `$`：以什么结尾

Case13: 假设一个目录（dirA）中有 100 个文件，我们需要在 Linux 中使用 grep 命令在所有文件中搜索某个关键字 

```sh
➜  kube-prometheus-release-0.8 grep -h 'image:' ./manifests/* | awk -F " " '{print $1,$2}'
grep: ./manifests/setup: Is a directory
image: quay.io/prometheus/alertmanager:v0.21.0
image: quay.io/prometheus/blackbox-exporter:v0.18.0
image: jimmidyson/configmap-reload:v0.5.0
```

Case14: 使用egrep 命令使用 grep 命令进行多个关键字搜索在 Linux 中

```sh
➜  kube-prometheus-release-0.8 egrep 'rm|echo|if' test.sh
# only exit with zero if all commands of the pipeline exit successfully
    echo "Testing: ${i}"
    echo ""
    echo "${snippet}" > "test.jsonnet"
    rm -rf "test.jsonnet"
```





#### sed

Linux SED（流编辑器）

语法：

```sh
sed [选项] '脚本' 输入文件
```

`选项` 是一些用于修改 `sed` 行为的可选参数。一些常见的选项包括：

- `-e`：允许在命令行中指定 `sed` 脚本。
- `-f`：从文件中读取 `sed` 脚本。
- `-i`：直接修改输入文件，而不是将结果输出到标准输出。
- `-n`：禁用默认的输出，只显示经过脚本处理的内容。

`编辑命令` 是要执行的操作，例如替换、删除、插入等。编辑命令通常以字母表示，例如：

- `s`：替换。
- `d`：删除。
- `p`：打印。
- `i`：插入。

案例一：如何仅显示给定的行或行范围？

+ -n: 只显示经过脚本处理的内容
+ -p : 打印

```sh
➜  kube-prometheus-release-0.8 sed -n '1,5p' test.sh
#!/usr/bin/env bash
set -e
# only exit with zero if all commands of the pipeline exit successfully
set -o pipefail

➜  kube-prometheus-release-0.8 sed -n '$p' test.sh
done
```

案例2：查看脚本中的 ‘rm’

```sh
➜  kube-prometheus-release-0.8 sed -n '/rm/p' test.sh
    rm -rf "test.jsonnet"
```

案例3: 如何在 sed 命令中使用多个表达式？

-e : 多个选项

```sh
➜  kube-prometheus-release-0.8 sed -n -e '2p' -e '5p' test.sh
set -e
➜  kube-prometheus-release-0.8 sed -n -e '/done/p' -e '/for/p' test.sh
for i in examples/jsonnet-snippets/*.jsonnet; do
done
for i in examples/*.jsonnet; do
done
```

案例5： 如何查看第 2 行接下来的 4 行？

```sh
sed -n ‘2,+4p’ file_name
```

案例6: 如何替换文件中的单词并显示？

```sh
sed -i '' 's/7.5.4/7.5.5/g' grafana-deployment.yaml
```

案例7: 修改是备份文件

```sh
sed -i '.bak' 's/7.5.5/7.5.9/g' grafana-deployment.yaml
```

案例8: 修改指定的行 和 不在指定的行

```sh
sed '5 s/<string_to_change>/<new_string>/g' file_name
sed '5! s/<string_to_change>/<new_string>/g' file_name
```

案例9：删除

```sh
sed -i '' '/image/d' grafana-deployment.yaml
```

==案例10==: 删除空行和注释行 ( 加 -i 实际删除)

```sh
sed '/^$/d' test.sh
```

案例11：如何将制表符替换为空格？

```
sed 's/\t/ /g' test.sh
```

==案例12==:  如何将 sed 命令的输出复制到单独的文件中？

```sh
kube-prometheus-release-0.8 sed -n '/echo/ w a.txt' test.sh
```

案例13: 在给定行号后添加

```sh
➜  kube-prometheus-release-0.8 sed '2s/^/#test\n/' test.sh
```

案例14：在给定行号后添加 追加一个文件的内容 w





#### awk

**`awk` 命令的基本结构：**

```
bashCopy code
awk 'pattern { action }' input-file
```

- `pattern`：一个条件或模式，用于匹配输入行。
- `action`：在满足 `pattern` 条件的情况下执行的操作。
- `input-file`：要处理的输入文件。

1. **字段分隔符 `-F`：**

   通过 `-F` 选项可以指定字段分隔符，默认是空格。例如：

   ```
   bashCopy code
   awk -F: '{ print $1, $3 }' /etc/passwd
   ```

2. **`$0` 和 `$n`：**

   - `$0` 代表整个当前行。
   - `$n` 代表当前行的第 n 个字段，字段以字段分隔符分隔。
   - `$NF` 最后一个

3. **`NF` 和 `NR`：**

   - `NF` 代表当前行的字段数量。
   - `NR` 代表当前处理的行号。

**`BEGIN` 和 `END` 块：**

- `BEGIN` 块中的命令在处理前执行一次。
- `END` 块中的命令在处理结束时执行一次。

**条件语句：**

可以使用 `if`、`else` 和 `else if` 等条件语句来控制执行流程。

**循环：**

`awk` 不支持传统的 `for` 和 `while` 循环，但你可以使用 `while` 块来模拟循环。



案例1 ：NR用法（条件 第几行）

+ `==` 和 `!=` `>` 等

```sh
 awk -F. 'NR == 2 {print $2}' 1.txt
```

案例2 ： 条件语句用法

```sh
➜  kube-prometheus-release-0.8 awk '{ if ($2!=0) {print "oooo"} else {print $NF} }' 1.txt

oooo
ESTABLISHED
ESTABLISHED
```

案例3:  awk查找打印

```
➜  kube-prometheus-release-0.8 awk '/SYN_SENT/ {print $0}' 1.txt
tcp4       0      0  leledeair.lan.64313    tsa01s13-in-f10..https SYN_SENT
tcp4       0      0  leledeair.lan.64312    tsa01s13-in-f10..https SYN_SENT
tcp4       0      0  leledeair.lan.64231    tsa03s08-in-f10..https SYN_SENT
tcp4       0      0  leledeair.lan.64230    tsa03s08-in-f10..https SYN_SENT
```

案例4：查找打印存在SYN_SENT 的行

```
➜  kube-prometheus-release-0.8 awk '/SYN_SENT/ {print $0}' 1.txt
tcp4       0      0  leledeair.lan.64313    tsa01s13-in-f10..https SYN_SENT
tcp4       0      0  leledeair.lan.64312    tsa01s13-in-f10..https SYN_SENT
tcp4       0      0  leledeair.lan.64231    tsa03s08-in-f10..https SYN_SENT
tcp4       0      0  leledeair.lan.64230    tsa03s08-in-f10..https SYN_SENT
```

#### 正则表达式

1. **基本正则表达式（BRE）**：基本正则表达式是最常见的正则表达式语法，用于工具如`grep`、`sed`等。它包括以下常见的元字符：
   - `.`：匹配任意单个字符。
   - `*`：匹配前一个字符的零个或多个副本。
   - `^`：匹配行的开头。
   - `$`：匹配行的结尾。
   - `[]`：字符类，匹配方括号内的任何字符。
   - `[^]`：否定字符类，匹配不在方括号内的任何字符。
   - `\`：转义字符。
2. **扩展正则表达式（ERE）**：扩展正则表达式是基本正则表达式的扩展，用于工具如`egrep`。它包括更多元字符，如：
   - `+`：匹配前一个字符的一个或多个副本。
   - `?`：匹配前一个字符的零个或一个副本。
   - `()`：分组，用于匹配和捕获子模式。
   - `|`：逻辑或，用于匹配多个模式中的一个。



---

### linux 排障



#### CPU使用率高问题排查

在服务的运行过程中，我们经常会看到CPU使用率会突然飙高的情况，有可能是因为请求量突增导致的，也有可能是程序设计的不合理导致的，这时候，我们需要一步步分析原因。

首先，我们需要查到是哪个进程导致的CPU飙高，我们可以用top命令查看CPU使用情况：top 命令可以实时显示系统的运行状态和 CPU 的使用率，你可以通过一些参数来过滤或排序进程，比如 -i 表示忽略空闲进程，-c 表示显示完整的命令行，-p 表示指定进程 ID 等。

我们重点关注以下几个指标：

-  %us：表示用户空间程序的 CPU 使用率（没有通过 nice 调度）
- %sy：表示系统空间的 CPU 使用率，主要是内核程序
- %ni：表示用户空间程序通过 nice 调度的 CPU 使用率
- %id：表示 CPU 的空闲时间
- %wa：表示等待 I/O 的 CPU 时间
- PID：表示进程 ID
- COMMAND：表示进程名或命令行

然后，我们再通过ps命令查看进程信息，如ps -ef |grep “pid",找到对应进程的目录位置，启动具体命令。ps 命令可以显示进程的详细信息，通过一些参数来选择或格式化输出，比如 -e 表示显示所有进程，-f 表示显示完整格式，-o 表示自定义输出字段等。**我们重点关注以下几个字段：**

- PID：表示进程 ID
- PPID：表示父进程 ID
- USER：表示进程所属用户
- STAT：表示进程状态
- TIME：表示累计占用 CPU 时间
- CMD：表示命令名或命令行

最后，我们再通过top -H -p pid 查看进程的对应哪个线程使用的CPU占用过高。

#### 磁盘IO高问题排查

磁盘IO突增问题在运维过程中也是常见的问题，原因有业务量的突增，程序读写磁盘不合理导致，要到知道具体原因，首先我们得先找到具体是哪个进程导致的IO高，这时候我们要用到iotop命令。

iotop 命令类似于 top 命令，但是专门针对 IO 活动的进程进行监控和分析，你可以通过一些参数来控制输出或过滤条件，比如 -o 表示只显示有 IO 活动的进程，-a 表示累计模式，-P 表示只显示进程而不是线程等。

除了iotop命令外，还有iostat，dstat等命令也可以查看磁盘IO，可以查看具体对应磁盘IO的读写量大小。



#### 内存溢出问题排查

内存溢出是指程序中已经不再使用的对象仍然被其他对象引用而无法被垃圾回收器回收的情况。内存泄漏会导致内存占用不断增加，最终可能引发内存溢出。一般情况下，内存溢出，我们可以作如下处理：



#### 系统负载高问题排查

想要解决linux负载高问题，首先要知道什么叫系统负载。Linux系统负载是指系统中正在运行和等待运行的进程数，它可以用来衡量系统的繁忙程度。具体来说，负载是指在一段时间内运行队列中的平均进程数，包括正在运行的进程和等待CPU时间片的进程。负载的值通常用一个数字来表示，可以通过命令“top"、“uptime"等命令查看。

在Linux系统中，负载值通常是一个三元组，表示最近1分钟、5分钟、15分钟内的负载情况。当负载值超过CPU核心数的时候，说明系统负载已经很高，如果持续较长时间的高负载可能导致系统运行缓慢或者不响应。比如我们的CPU核数是4，当负载小于4时，说明进程没有在队列中等待执行，当负载大于4时，说明是有进程在队列中等待执行的。

一般情况下，导致linux系统负载高有如下原因：

① 进程过多：如果系统上运行着大量进程或服务，它们将占用CPU和内存资源，导致负载升高。

② CPU资源不足：如果CPU不足以处理当前系统的负载，系统的平均负载会升高。

③ 磁盘I/O：如果磁盘I/O操作非常频繁，如大量写入或读取数据，这也会导致系统的平均负载升高。

④ 内存不足：如果系统没有足够的可用内存来运行进程或服务，它们将开始使用交换空间，这也会导致系统负载升高。

⑤ 网络流量过大：如果系统正在处理大量的网络流量，这可能导致CPU和网络资源的饱和，从而导致系统负载升高。

⑥ 资源限制：如果系统管理员设置了资源限制，如CPU时间限制或进程数限制，这也可能导致系统的平均负载升高。

要解决Linux系统负载高的问题，可以尝试以下几个步骤：

① 使用top或htop等工具查看系统进程的资源占用情况，找出占用资源最多的进程。

② 调整系统服务或应用程序的配置，减少它们占用的资源。

③ 增加系统的CPU、内存或磁盘I/O能力。

④ 调整系统的资源限制，允许更多的资源用于运行进程或服务。

⑤ 对于网络流量过大的情况，可以尝试使用流量控制技术来平衡网络负载。



### 网路

#### tcpdump

本质是一个命令行的数据包捕获程序，

语法非常重要：

`选项`是用于指定`tcpdump`的不同配置和功能的标志。一些常见的选项包括：

- `-i`：指定要捕获的网络接口。
- `-n`：禁用将IP地址和端口号转换为主机名和服务名。
- `-c`：指定要捕获的数据包数量。
- `-X`：以十六进制和ASCII形式显示数据包内容。
- `-w`：将捕获的数据包保存到文件中，而不是将其显示在终端上。



+ `host` 指定目标，可以是ip、也可以是网卡
+ `dest` 目标 `src`源
+ `and` `or`
+ `net` 网段
+ `port` 端口

允许从 osi 模型的几乎所有层抓取，可以存储然后在 wireshark 等中分析



我使用它的次数超过了，wireshark 

 案例1: 监视 网卡

```sh
sudo tcpdump -i en0
```

将会捕获各种各样的数据

案例2: 扫描特定数据 （和 wireshark看到的很像）

```sh
sudo tcpdump -i en0 -v host www.baidu.com
```

案例3:

```
sudo tcpdump -v -i en0 dst 192.168.31.136 and src 192.168.31.136
```

案例4:

```
sudo tcpdump -i en0 -v net 192.168.31.0/24
```

案例5: tcp、udp

```sh
sudo tcpdump -v -i en0 tcp and net 192.168.31.0/24
```

案例6: 特定端口

```sh
sudo tcpdump -v -i en0 port 80
```

案例7: 保存

```
tcpdump -w /root/Desktop/traffic.pcap
```

---



### Shell脚本



### 手册



==tcpdump==

1. 如何解决网路问题？

tcpdump ，捕获网络接口上的数据包

```
tcpdump -i 接口 -w /tmp/a.paca
```

如何读取

```
tcpdump -r 文件
```

2.  如何捕获 源IP 和目标IP之间的数据包

```
tcpdump -v -i eth0 src 1.1.1.1 and dst host 2.2.2.2
```

3. 如何捕获接口是443的目标端口的网路流量

```
 tcpdump -v dst port 44  3
```

==iostat==

4. 如何判断存储设备的存储性能问题

+ 进行性能调整 iostats ，诊断存储设备的性能问题

```
iostats 
```

5. iostats相关选项

```sh
iostat -d # 显示磁盘
```

6. 显示CPU统计数据

```sh
root@serevr-1:~# iostat -c
Linux 5.15.0-41-generic (serevr-1) 	10/14/23 	_aarch64_	(2 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
```

==ldd==

+ 列表动态依赖项,动态的诊断应用程序的启动问题

7. 如果程序因为依赖项未启动，如何确定是哪些以来项

```sh
systemctl status sshd #查看文件
lld sshd
```

==netstart==

8.  如何显示所有协议的统计信息 -s

 ```sh
 netstart -lntp
 ```



9. 如何查看内核的路由表信息

+ 或者 route

```
root@serevr-1:~# netstat -r
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
```

==free==

10. 如果linux内存不足，或者只是想了解内存中有多少

```
root@serevr-1:~# free -g
               total        used        free      shared  buff/cache   available
Mem:               1           0           0           0           1           1
Swap:              1           0           1
```





在生产环境中排除如何与性能相关的问题时，都很重要

==lsof==

11.  如何检查文件已打开并带有起进程ID

```
lsof
```

12. 列出特定PID打开的文件

```sh
# 先ps -ef查看PID
lsof -p PID
```

13. 如何找出 deamon使用的端口好

```
lsof -i -P | grep 22
root@serevr-1:~# lsof -i -P | grep 349386
sshd      349386       parallels    4u  IPv4 2249432      0t0  TCP ubuntu-22.04-arm64.shared:22->10.211.55.2:52648 (ESTABLISHED)
```

==pidstat==

14. 如何解决IO相关的问题

```sh
#I/O
pidstat -d
pistata -p 111 -d 5
```

==t op==

15.  哪个命令用于显示系统摘要信息和当前利用率

```sh
top
```

16. 哪个命令可以显示CPU利用率详细信息、一些任务或内存利用率 以及僵尸进程数量

```sh
top
```

17.显示特定用户

```sh
top -u root
```

==kill==

18. 使用top命令杀死进程 

```sh
-9  # 强制
-15 # 正常
```

19. 如何简单的找出PID和进程详细信息

```sh
ps -ef | grep 
```

20. 强制终止进程

```sh
kill -9
```





21. TCP 和 UDP协议的区别

尽量不要详细的说，确保答案非常概括和准确。

安全性TCP比UDP高、TCP是面向连接的，UDP是连接列表，另外一点：TCP很慢，UDP很快。

UDP被实时应用程序使用、TCP将对需要传输数据可靠的程序使用。

TCP有流量控制，UPD没有。

 

22. 使用find 列出 目录下的目录文件

+ type
+ maxdepth

```sh
 find . -type d -maxdepth 1
```



23. 文件减压缩

+ tar -zxvf -zcvf 参数 -C
+ unzip 、 zip 参数 -d
+ gizp

 

24. 普通用户尝试远程登录其他服务器，收到错误信息

1. 服务器是否正常运行
2. 防火墙 阻止了，telnet端口



25. read磁盘相关

26. 什么是文件系统

+ ext4、ext3、xfs



27. 可以在不创建交换分区的情况下构建服务器吗？

关闭交换分区对于 Kubernetes 集群是一个更好的选择，以确保系统的可靠性和性能。



28.增量备份和差异备份的区别

增量备份仅备份自上次备份以来发生更改的数据

差异备份会备份自上次完整备份以来发生的所有更改，而不是仅备份自上次备份以来的变化。



==find==

31. 多个文本中搜索文本

```sh
find /var/log -type f -name '*.log' -exec grep -i 'error' {} \;
```



32.  如何删除30天前的日志

```sh
find /var/log/ -type f -mtime +30 -name '*.log' -exec ls -l \;
```



33. 删除45min前的

```sh
find /var/log/ -type f -ctime +45 -name '*.log' -exec ls -l \;
```



34.  NFS共享信息的位置

```
/etc/fstab
```



35.哪个文件用来指定网关

```sh
/etc/sysconfig/network
```



36. 如何在Linux扩展文件系统 

==lvm==

1. 新创建的分区`sda3`添加到LVM卷组。使用以下命令：

```
pvcreate /dev/sda3
vgextend centos /dev/sda3  # 假设centos是您的卷组名称
```

1. 扩展LVM逻辑卷`centos-root`。使用以下命令：

```
lvextend -l +100%FREE /dev/centos/root
```

1. 扩展文件系统。如果您的根分区是`ext4`，可以使用以下命令：

```
resize2fs /dev/centos/root
```

如果您的根分区是`xfs`，可以使用以下命令：

```
xfs_growfs /
```

 



##### 1.将用户和密码设置成永不过期

```
chage -M -1 u1
```

##### 2.为什么/etc/passwd和/etc/shadow文件不能合并

+ 用户信息对于系统非常重要，合并的话对于攻击者来说，用户名和密码在一个文件，确保不被攻击
+ 提高安全性， 合并在一起用户都可以访问，都不能成为root

##### 3.根据PID列出所有打开的文件

```
lsof -p PID
```

##### 4. 当你尝试挂载时，收到错误（可能是因为某些文件、目录、或者文件系统）

+ 确保/etc/fstab的配置正确（挂载点、挂载的目录是否存在、权限问题、网路问题）

##### 5.服务器重启花费了更多的时间，可能是什么原因？

+ 文件系统损坏

##### 6.尝试在分区下创建文件、但是收到了权限拒绝、可能是什么原因？但是没有空间问题没有权限问题。

+ 可能会认为与用户相关，但不，和用户没关系
+ 与空间也没关系、没有权限问题
+ 正确答案：与索引节点相关，用完了所有的索引节点
+ 解决方法：找到不需要的大文件移动到其他盘或者服务器

```
root@serevr-1:~# df -i /
Filesystem      Inodes  IUsed   IFree IUse% Mounted on
/dev/sda2      4128768 211003 3917765    6% /
```

##### 7.如何检查路由表信息内核

+ route -n 
+ ip route
+ netstat -rn

##### 8.如何设置粘性位，小s和大S区别 （只有root和所有者可以删除，即使其他用户有完全权限）

```
chmod o+t /opt/dump/
chmod +t /opt/dump/
```

权限：所有者、所有组、其他人

#####  9.如何看当前Linux系统有几颗物理CPU和每颗CPU的核数？

 ```
 lscpu
 cat /proc/cpuinfo
 ```

##### 10.查看系统负载有常用的命令，这三个数值表示什么含义呢？

```
top
uptime
```

三个数值分别表示一分钟、五分钟、十五分钟内系统的平均负载，即平均任务数

##### 11.如何查看当前系统都有哪些进程？

```
ps -aux
top
```

##### 12.ps 查看系统进程时，有一列为STAT， 如果当前进程的stat为Ss 表示什么含义？如果为Z表示什么含义？

+ S : 睡眠状态
+ s：主进程
+ Z：僵尸进程

##### 13.如何查看系统都开启了哪些端口？

```
netstart -lnp
```

##### 14.如何查看网络连接状况？

```
netstart -na
```

##### 15.想修改ip，需要编辑哪个配置文件，修改完配置文件后，如何重启网卡，使配置生效？

```
/etc/sysconfig/network-scripts/ifcft-eth0
ifdown eth0
ifup eth0
```

##### 16.能给一个网卡配置多个IP? 如果能，怎么配置？

```
可以
创建一个网卡ifcft-eth0:1 的文件
```

##### 17.如何查看当前主机的主机名，如何修改主机名？要想重启后依旧生效，需要修改哪个配置文件呢？

```
hostname
hostnamectl-hostnameset xx
/etc/hostname
```

##### 18.设置DNS需要修改哪个配置文件？

```
/etc/hosts
/etc/resolv.conf
```

##### 19.使用iptables 写一条规则：把来源IP为192.168.1.101访问本机80端口的包直接拒绝

```
iptables -i INPUT -s 192.168.1.101 -p tcp --dport 80 -j REJECT
```

##### 20.要想把iptable的规则保存到一个文件中如何做？如何恢复？

```
iptables-save > 1.txt
```

##### 21.任务计划格式中，前面5个数字分表表示什么含义？

```
分 时 日 月 年
```

##### 22、如何可以把系统中不用的服务关掉？

```
kill
```

##### 23.某个账号登陆linux后，系统会在哪些日志文件中记录相关信息？

```
/var/log/source
```

##### 24.网卡或者硬盘有问题时，我们可以通过使用哪个命令查看相关信息？

```
dmegs
/var/log/message
```

##### 25.分别使用xargs和exec实现这样的需求，把当前目录下所有后缀名为.txt的文件的权限修改为777

```
find ./ -type f -name "*.txt" -exec chmod 777 {} \;
find ./ -type f -name "*.txt" | xargs chmod 777
```

##### 26.有一个脚本运行时间可能超过2天，如何做才能使其不间断的运行，而且还可以随时观察脚本运行时的输出信息

+ 使用 nohup 将脚本在后台运行，并且输出结果重定向到一个文件

```
nohup ./your_script.sh > script_output.log 2>&1 &
```

##### 27.在Linux系统下如何按照下面要求抓包：只过滤出访问http服务的，目标ip为192.168.0.111，一共抓1000个包，并且保存到1.cap文件中？

```
tcpdump -w 1.cap -c 1000 dst 192.168.0.111 and port 80
```

##### 28.想在Linux命令行下访问某个网站，并且该网站域名还没有解析，如何做？

+ curl ip 或者 在/etc/hosts加一条解析

##### 29.自定义解析域名的时候，我们可以编辑哪个文件？是否可以一个ip对应多个域名？是否一个域名对应多个ip？

+ /etc/hosts
+ 可以一个ip对应多个域名
+ 不可用一个域名对应多个ip

##### 30.我们可以使用哪个命令查看系统的历史负载（比如说两天前的）？

```
sar
```

##### 31.在Linux下如何指定dns服务器，来解析某个域名？

```
/etc/resolv.conf
nameserver ip
```

##### 32.使用free查看内存使用情况时，哪个数值表示真正可用的内存量？

```
free -m 
avalabel
```

##### 33.有一天你突然发现公司网站访问速度变的很慢很慢，你该怎么办呢？

可以从系统负载和网卡流量方面入手分析。分析系统负载，使用w命令或者uptime命令查看系统负载，如果负载很高，则使用top命令查看CPU，MEM等占用情况，要么是CPU繁忙，要么是内存不够，如果这二者都正常，再去使用sar命令分析网卡流量，分析是不是遭到了攻击。一旦分析出问题的原因，采取对应的措施解决，如决定要不要杀死一些进程，或者禁止一些访问等。

##### 34.给您一台最小化安装的linux机器，如何进行基础优化？

（1）更新yum官方源
（2）关闭不需要的服务
（3）关闭不需要的TTY
（4）对TCP/IP网络参数进行调整。例如：优化Linux下的内核TCP参数以提高系统性能。
（5）设置时间同步
（6）优化最大文件数限制
（7）关闭SELINUX
（8）修改SSH登录配置
（9）清理登陆的时候显示的系统及内核版本
（10）删除不必要的系统用户和群组
（11）关闭重启ctl-alt-delete组合键
（12）设置一些全局变量
（13）设置history历史记录
（14）centos6.4最小化安装后启动网卡
（15）添加普通用户，设置sudo权限
（16）禁止root远程用户登录
（17）sed修改远程端口
（18）防火墙iptables配置。
（19）修改默认DNS
（20）安装必要软件,更新yum源 [epel源]
（21）更新内核和软件到最新版本
（22）去除上次登录的信息

##### 35.请说出内核调优配置文件名字？举例几个内核需要优化的参数配置？

```
/etc/sysctl.conf
```

\#允许系统打开的端口范围

```
net.ipv4.ip_local_port_range = 1024 65000
```

最大打开文件数，单个进程可分配的最大文件数

```
fs.nr_open = 10000000 
```

##### 36.怎样一页一页地查看一个大文件的内容呢?

```
more
```

##### 37.用 tcpdump 嗅探 80 端口的访问看看谁最高

```
tcpdump -v dst port 80
```

##### 38.查看当前系统每个 IP 的连接数

```
netstat -n | awk '/^tcp/ {print $5}'| awk -F: '{print $1}' | sort | uniq -c | sort -i
```

##### 39.列出linux常见打包工具并写相应解压缩参数(至少三种)

+ tar -zcf 、-zxf
+ gzip 
+ zip、unzip

##### 40.计划每星期天早8点服务器定时重启，如何实现？

```
crontab -e
* 8 * * 7 reboot
```

##### 41.当用户在浏览器当中输入一个网站，说说计算机对dns解释经过那些流程？

假设：本机跟本地dns还没有缓存。

（1）用户输入网址到浏览器；
（2）浏览器发出DNS请求信息；
（3）计算机首先查询本机HOST文件，看是否存在，存在直接返回结果，不存在，继续下一步；
（4）计算机按照本地DNS的顺序，向合法dns服务器查询IP结果；
（5）合法dns返回dns结果给本地dns，本地dns并缓存本结果，直到TTL过期，才再次查询此结果；
（6）返回IP结果给浏览器；
（7）浏览器根据IP信息，获取页面；

##### 42.编写个shell脚本将当前目录下大于10K的文件转移到/tmp目录下

```
#/bin/bash
find ./ -type f -size +10k -exec mv {} /tmp/ \;
```

##### 43.编写shell脚本获取本机的网络地址。

比如：本机的ip地址是：192.168.100.2/255.255.255.0，那么它的网络地址是192.168.100.1/255.255.255.0

```
ifconfig | grep -w inet | awk '$2 != "127.0.0.1" {print $2,$4}' | awk -F '.' '{print $1"."$2"."$3".0"}'
```

##### 44.linux下如何添加路由？

```sh
# 添加到主机路由

route add –host 192.168.168.110 dev eth0
route add –host 192.168.168.119 gw 192.168.168.1
# 添加到网络的路由

route add –net IP netmask MASK  deveth0
route add –net IP netmask MASK gw IP
```

##### 45.简述Tcp三次握手的过程

第一次握手，建立连接，客户端发送SYN包到服务器，并进入SYN_SEND状态，等待服务器确认；
第二次握手，服务器收到SYN，同时自己也发送一个SYN包和一个ACK包来确认客户端的SYN，并进入SYN_RECV；
第三次握手，客户端收到服务器发来的SYN+ACK后，回复服务器端一个ACK确认，发送完毕后，双方进入ESTABLISHED状态。
三次握手成功后，开始传输数据。

##### 46.请描述Linux系统优化的12个步骤

（1）登录系统:不使用root登录，通过sudo授权管理，使用普通用户登录。
（2）禁止SSH远程：更改默认的远程连接SSH服务及禁止root远程连接。
（3）时间同步：定时自动更新服务器时间。
（4）配置yum更新源，从国内更新下载安装rpm包。
（5）关闭selinux及iptables（iptables工作场景如有wan ip，一般要打开，高并发除外）
（6）调整文件描述符数量，进程及文件的打开都会消耗文件描述符。
（7）定时自动清理/var/spool/clientmquene/目录垃圾文件，防止节点被占满（c6.4默认没有sendmail，因此可以不配。）
（8）精简开机启动服务（crond、sshd、network、rsyslog）
（9）Linux内核参数优化/etc/sysctl.conf，执行sysct -p生效。
更改字符集，支持中文，但是还是建议使用英文，防止乱码问题出现。
（10）锁定关键系统文件（chattr +i /etc/passwd /etc/shadow /etc/group /etc/gshadow /etc/inittab 处理以上内容后，把chatter改名，就更安全了。）
（11）清空/etc/issue，去除系统及内核版本登陆前的屏幕显示。

##### 47.统计ip访问情况，要求分析nginx访问日志，找出访问页面数量在前十位的ip

```
cat access.log | awk '{print $1}' | uniq -c | sort -rn | head -10
```

##### 48.你对现在运维工程师的理解和以及对其工作的认识

运维工程师在公司当中责任重大，需要保证时刻为公司及客户提供最高、最快、最稳定、最安全的服务。运维工程师的一个小小的失误，很有可能会对公司及客户造成重大损失
。因此，运维工程师的工作需要严谨及富有创新精神。



### 漏洞修复

（以Apache Tomcat 9.0.0.M1版本至9.0.17版本、8.5.0版本至8.5.39版本和7.0.0版本至7.0.93版本中的CGI Servlet存在操作系统命令注入漏洞）

1. 访问tomcat的官网漏洞补丁网址
2. 会看到各个版本的tomcat的漏洞修复记录及方法
3. 例如 搜索Apache Tomcat远程代码执行漏洞(CVE-2019-0232)漏洞的补丁（[http://mail-archives.apache.org/mod_mbox/www-announce/201904.mbox/%3C13d878ec-5d49-c348-48d4-25a6c81b9605@apache.org%3E](http://mail-archives.apache.org/mod_mbox/www-announce/201904.mbox/<13d878ec-5d49-c348-48d4-25a6c81b9605@apache.org>)）
4. 可以看到详细的漏洞修复方法，及补丁



### kubectl

---

Kubernetes 有两个组件；控制平面和工作节点。如果您说“主节点”而不是“控制平面”，则它可能不包括 etcd。因为 etcd 可以设置在主节点外部。组件 kube-controller-manager 并没有准确地组合“所有”进程，而是组合了各种控制器进程，如节点控制器、服务帐户控制器等。 Kube-scheduler 不仅为工作节点调度工作，还为包括主节点在内的所有节点调度工作节点。例如，master节点内的pod由kube-scheduler调度。ETCD 不仅存储用户名和密码，还存储集群数据的状态，包括 pod、rs 等的状态。此外，kubelet、kube-proxy 和 Container Runtime（3 个主要组件）不仅在工作节点中运行，而且在所有主节点中运行节点。



kubernetes中重要的安全措施：主要是四级安全；云、集群、容器和代码强化 (4 C)。除了 RBAC 和适当的服务帐户之外，还可以使用准入控制器（例如 Pod 标准策略）、使用网络策略、使用外部机密管理器（例如保管库或 AWS 机密管理器）进行机密管理、使用安全功能以及与映像安全相关的其他步骤。