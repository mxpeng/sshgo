# sshgo
一个SSH登录服务器的shell脚本

## 使用
1).给sshgo文件执行的权限,并执行sshgo
```shell
  chmod u+x sshgo
  ./sshgo
```
2).可以将sshgo 软连接到 /usr/local/bin ,之后便可以在终端中全局使用sshgo
```shell
  chmod u+x sshgo
  ln -s $PWD/sshgo /usr/local/bin
  sshgo
```
    注意: ln -s 之后的路径都要是完整的路径地址

3).命令使用

`sshgo` - 服务器登陆、新增、删除

`sshgo list` - 查看所有服务器配置

`sshgo 1` - 登录第一个配置的服务器

4).加密

SSH连接信息使用`aes-256-cbc`对称加密存放在 `./config` 文件夹中，加密密钥写在sshgo脚本中，注意替换默认密钥

加密只是为了不让SSH连接信息明文显示，防君子不防小人

可以进一步使用`encryption`进一步加密下shell脚本，隐藏sshgo中的密钥

```shell
chmod u+x encryption
./encryption
```

操作后会生成`sshgo.x` 和 `sshgo.x.c` 

`sshgo.x`是加密后的可执行的二进制文件
`sshgo.x.c`是生成`sshgo.x`的原文件(c语言)

这时候执行脚本就改为

```
./sshgo.x 
```

## 添加配置

运行sshgo
```
./sshgo
```

显示
```shell

-----------------------------------
-     请输入登录的服务器序号      -
-----------------------------------


login [arg] 登陆  |  add 增加  |  delete [arg] 删除

请输入操作命令和要操作的服务器序号:
-----------------------------------

add

请按照如下格式输入
分组 服务器名称 IP地址 端口号 登录用户名 登录密码/秘钥文件Key 秘钥文件地址

```

输入服务器信息
```
GroupName ServerName xxx.xxx.xxx.xxx 22 user password 
或者
GroupName ServerName xxx.xxx.xxx.xxx 22 user key ~/private_key.pem
```

## 提示
### 使用本脚本前，请确认已安装expect 和 openssl

1) Linux 下 安装expect 和 openssl
```shell
 yum install expect
 yum install openssl
```
2) Mac 下 安装expect 和 openssl
```shell
 brew install homebrew/dupes/expect
 brew install openssl
```
### 使用shell加密脚本前，请安装 shc

1) Linux 下 安装shc
```shell
 yum install shc
```
2) Mac 下 安装shc
```shell
 brew install shc
```

## 特殊说明
如果密码中含有以下特殊字符，请按照一下规则转义：
- \ 需转义为 \\\\\
- } 需转义为 \\}
- [ 需转义为 \\[
- $ 需转义为 \\\\\\$
- \` 需转义为 \\`
- " 需转义为 \\\\\\"
- . 需转义为 \\.

```
如密码为'-OU[]98' 在CONFIG配置中写成'-OU\[]98'
否则，提示要手动输入密码
```
