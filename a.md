


实际要解析的输入参数往往是你的脚本参数，这样`parseOpts`函数调用一般是：

```bash
parseOpts "a,a-long|b,b-long:|c,c-long+" "$@"
# "$@" 即是回放你的脚本参数
```

通过约定的全局变量来获取选项和参数：

* 选项名为`a`，通过全局变量`_OPT_VALUE_a`来获取选项的值。
* 选项名为`a-long`，通过全局变量`_OPT_VALUE_a_long`来获取选项的值。  
即，把选项名的`-`转`_`，再加上前缀`_OPT_VALUE_`对应的全局变量来获得选项值。
* 除了选项剩下的参数，通过全局变量`_OPT_ARGS`来获取。

按照惯例，输入参数中如果有`--`表示之后参数中不再有选项，即之后都是参数。

### 示例

```bash
# 导入parseOpts.sh
source /path/to/parseOpts.sh

parseOpts "a,a-long|b,b-long:|c,c-long+" -a -b bv --c-long c.sh -p pv -q qv arg1 \; aa bb cc
# 可以通过下面全局变量来获得解析的参数值：
#   _OPT_VALUE_a = true
#   _OPT_VALUE_a_long = true
#   _OPT_VALUE_b = bv
#   _OPT_VALUE_b_long = bv
#   _OPT_VALUE_c = (c.sh -p pv -q qv arg1) ，数组类型
#   _OPT_VALUE_c_long = (c.sh -p pv -q qv arg1) ，数组类型
#   _OPT_ARGS = (aa bb cc) ，数组类型
```

`--`的使用效果示例：

```bash
# 导入parseOpts.sh
source /path/to/parseOpts.sh

parseOpts "a,a-long|b,b-long:|c,c-long+" -a -b bv -- --c-long c.sh -p pv -q qv arg1 \; aa bb cc
# 可以通过下面全局变量来获得解析的参数值：
#   _OPT_VALUE_a = true
#   _OPT_VALUE_a_long = true
#   _OPT_VALUE_b = bv
#   _OPT_VALUE_b_long = bv
#   _OPT_VALUE_c 没有设置过
#   _OPT_VALUE_c_long 没有设置过
#   _OPT_ARGS = (--c-long c.sh -p pv -q qv arg1 ';' aa bb cc) ，数组类型
```

### 兼容性

这个脚本比较复杂，测试过的环境有：

1. `bash --version`  
`GNU bash, version 4.1.5(1)-release (x86_64-pc-linux-gnu)`  
`uname -a`  
`Linux foo-host 2.6.32-41-generic #94-Ubuntu SMP Fri Jul 6 18:00:34 UTC 2012 x86_64 GNU/Linux`
1. `bash --version`  
`GNU bash, version 3.2.53(1)-release (x86_64-apple-darwin14)`  
`uname -a`   
`Darwin foo-host 14.0.0 Darwin Kernel Version 14.0.0: Fri Sep 19 00:26:44 PDT 2014; root:xnu-2782.1.97~2/RELEASE_X86_64 x86_64 i386 MacBookPro10,1 Darwin`
1. `bash --version`  
`GNU bash, version 3.00.15(1)-release (i386-redhat-linux-gnu)`  
`uname -a`   
`Linux foo-host 2.6.9-103.ELxenU #1 SMP Wed Mar 14 16:31:15 CST 2012 i686 i686 i386 GNU/Linux`

### 贡献者

[Khotyn Huang](https://github.com/khotyn)指出`bash` `3.0`下使用有问题，并提供`bash` `3.0`的测试机器。

:beer: [xpl](xpl) and [xpf](xpf)
----------------------

* `xpl`：在文件浏览器中打开指定的文件或文件夹。  
\# `xpl`是`explorer`的缩写。
* `xpf`: 在文件浏览器中打开指定的文件或文件夹，并选中。   
\# `xpf`是`explorer and select file`的缩写。

### 用法

```bash
xpl
# 缺省打开当前目录
xpl <文件或是目录>...
# 打开多个文件或目录

xpf
# 缺省打开当前目录
xpf <文件或是目录>...
# 打开多个文件或目录
```

### 示例

```bash
xpl /path/to/dir
xpl /path/to/foo.txt
xpl /path/to/dir1 /path/to/foo1.txt
xpf /path/to/foo1.txt
xpf /path/to/dir1 /path/to/foo1.txt
```

### 贡献者

[Linhua Tan](https://github.com/toolchainX)修复Linux的选定Bug。

:key: 下载使用
----------------------

### 下载整个工程的脚本

#### 直接clone工程

使用简单、方便更新，不过要安装有`git`。

```bash
git clone git://github.com/oldratlee/useful-scripts.git

cd useful-scripts

# 使用Release分支的内容
git checkout release

# 更新脚本
git pull
```

包含2个分支：

- `master`：开发分支
- `release`：发布分支，功能稳定的脚本

当然如果你不想安装`git`,`github`是支持`svn`的：

```bash
svn co https://github.com/oldratlee/useful-scripts/branches/release

cd useful-scripts

# 更新脚本
svn up
```

PS：     
我的做法是把`useful-scripts` checkout到`$HOME/bin/useful-scripts`目录下，再把`$HOME/bin/useful-scripts`配置到`PATH`变量上，这样方便我本地使用所有的脚本。

#### 打包下载

下载文件[release.zip](https://github.com/oldratlee/useful-scripts/archive/release.zip)：

```bash
wget --no-check-certificate https://github.com/oldratlee/useful-scripts/archive/release.zip

unzip release.zip
```

### 下载和运行单个文件

以[`show-busy-java-threads.sh`](https://raw.github.com/oldratlee/useful-scripts/release/show-busy-java-threads.sh)为例。

#### `curl`文件直接用`bash`运行

```bash
curl -sLk 'https://raw.github.com/oldratlee/useful-scripts/release/show-busy-java-threads.sh' | bash
```

#### 下载单个文件

```bash
wget --no-check-certificate https://raw.github.com/oldratlee/useful-scripts/release/show-busy-java-threads.sh
chmod +x show-busy-java-threads.sh

./show-busy-java-threads.sh
```
