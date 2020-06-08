# GIT环境配置

### GIT安装完成后配置

```text
# 配置用户名
git config --global user.name "username“//(GitLab用户名)
# 配置邮箱
git config --global user.email "username@email.com"//(Gitlab邮箱,企业邮箱账号)
```

### 生成ssh

```text
ssh-keygen -t rsa
```

然后连敲三次回车键，结束后去系统盘目录下（一般在 C:\Users\你的用户名\.ssh）查看是否有ssh文件夹生成，此文件夹中以下两个文件

![](../.gitbook/assets/image%20%281%29.png)

将ssh文件夹中的公钥（ id\_rsa.pub）添加到GItLab管理平台中

![](../.gitbook/assets/image%20%282%29.png)

在Git Bush命令框（就是刚才配置账号和邮箱的命令框）中继续输入以下命令，回车

![](../.gitbook/assets/image%20%283%29.png)

