# sshgo

一个ssh登录服务器的shell脚本

## 所需环境

```sh
# linux
yum -y install git expect openssl shc

# mac
brew install expect git openssl shc coreutils

# mac增加GNU环境变量
export PATH=/usr/local/opt/coreutils/libexec/gnubin:$PATH
```

## 简单使用

- 下载授权运行

```sh
git clone https://www.github.com/mxpeng/sshgo
cd sshgo
chmod +x sshgo
./sshgo
```

- 添加配置

![](http://static.mxpeng.cn/img/20190826115907.png)

```sh
   mxpeng, 欢迎使用sshgo

1) 输入 [id]              登录服务器
2）输入 add               按照提示增加服务器
3）输入 delete [id]       删除服务器
4）输入 q | exit          退出
4）输入 r | refresh       刷新列表
5）输入 d | download [id] 下载服务器文件到本地
6）输入 u | upload   [id] 上传本地文件到服务器

---------------------------------------------------------------------------------------------

请输入操作命令或要操作的服务器序号:
---------------------------------------------------------------------------------------------

```

```
add

请按照如下格式输入
分组 服务器名称 IP地址 端口号 登录用户名 登录密码/秘钥文件Key 秘钥文件地址

```

```
A 服务器A xxx.xxx.xxx.xxx 22 user password 
或者
B 服务器B xxx.xxx.xxx.xxx 22 user key ~/private_key.pem
```

## 命令

- `./sshgo` 运行

- `./sshgo list` 查看所有服务器配置

- `./sshgo 1` 快捷登录配置的第一个服务器

## 高级

#### 全局使用`sshgo`

可以将`sshgo`软连接到`/usr/local/bin`，之后便可以在终端中全局使用`sshgo`，注意移动原文件后需要重新软连接
```sh
ln -s $PWD/sshgo /usr/local/bin
```

### 加密

- ssh连接信息使用`aes-256-cbc`对称加密存放在 `/usr/local/etc/sshgo/config` 文件夹中
- 加密密钥写在`sshgo`脚本中，默认密钥`xxxxxx`
- 注意替换后需要重新添加服务器信息，旧的将全部不可用
- shc命令执行后生成的`sshgo.x` 和 `sshgo.x.c`，`sshgo.x`是加密后的可执行的二进制文件，`sshgo.x.c`是生成`sshgo.x`的原文件(c语言)
- 下面命令中`123456`是新的密钥，请替换成要修改的密钥

```sh
# linux
cp sshgo sshgo.bak
sed -i "s/xxxxxx/123456/g" sshgo 
shc -v -r -f ./sshgo
mv -f sshgo.x sshgo
rm -rf sshgo.x.c

# mac
sed -i ".bak" "s/xxxxxx/123456/g" sshgo 
shc -v -r -f ./sshgo
mv -f sshgo.x sshgo
rm -rf sshgo.x.c
```

## 特殊说明
如果密码中含有以下特殊字符，请按照一下规则转义：
```$xslt
\ 需转义为 \\\
} 需转义为 \}
[ 需转义为 \[
$ 需转义为 \\\$
` 需转义为 \`
" 需转义为 \\\"
. 需转义为 \.

如密码为'123$456' 在CONFIG配置中写成'123\\\$456' 否则，提示要手动输入密码
```
