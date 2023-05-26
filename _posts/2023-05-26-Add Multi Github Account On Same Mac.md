---
title: 一台Mac上配置多个Github账号，工作/摸鱼两不误
date: 2023-05-26 22:20:00 +/-0800  
categories: [devtools, github]  
tags: [multi, github, account, mac, macos]
authors: [seven_altman, x_altman]
img_path: /assets/img/devtools/github/
#image:
#  path: code_preview.png
#  alt: Github  
pin: true

---

![github light img](github-light.png){: .light }
![github dark img](github-dark.png){: .dark }

在一台Mac上配置多个GitHub账号,可以按以下步骤操作:  
### 1. 生成多个SSH密钥:
```
bash
// 将email修改为你的第一账户的email，建议为常用的账号
ssh-keygen -q -t rsa -C "your_email_1@example.com" -f ~/.ssh/id_rsa_github_1 -N ""
// 将email修改为你的第二账户的email
ssh-keygen -q -t rsa -C "your_email_2@example.com" -f ~/.ssh/id_rsa_github_2 -N ""
```
上述命令将将会生成两个SSH密钥对`id_rsa_github_1`和`id_rsa_github_2`,分别对应两个GitHub账号。  

### 2. 添加SSH密钥到GitHub账号:
分别将`id_rsa_github_1.pub`和`id_rsa_github_2.pub`文件的内容添加到对应的GitHub账号的SSH Keys设置页面，注意别搞反了哦！ 

### 3. 配置SSH config文件:
在`~/.ssh/`目录下创建`config`文件：

```
#bash
touch ~/.ssh/config
```
然后在文件中输入以下内容并保存：

```
Host github.com
HostName github.com
IdentityFile ~/.ssh/id_rsa_github_1
PreferredAuthentications publickey

Host github_2.com 
HostName github.com
IdentityFile ~/.ssh/id_rsa_github_2
PreferredAuthentications publickey
```
这会将github.com对应到第一个GitHub账号,github_2.com对应到第二个GitHub账号。

### 4. 把专用密钥添加到高速缓存中

```
#bash
ssh-add --apple-use-keychain ${rsafile_1}
ssh-add --apple-use-keychain ${rsafile_2}
```

### 5. 测试SSH连接:

```
bash
ssh -T git@github.com
ssh -T git@github_2.com
```
正常情况下，你会得到如下消息：

```
Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
```
如果都可以成功登录,则SSH连接配置成功。
### 6. 清除global config配置
可以用`git config -l` 查看是否配置过`user.name`, `user.email`，如果配置过，则用以下命令unset:  

```
git config --global --unset user.name 
git config --global --unset user.email
```

### Congratulations!
> **使用方法**：克隆仓库时要使用对应的Host.
{: .prompt-info }

当克隆第一个GitHub账号的仓库时,使用git@<font color="#dd0000">github.com</font>:

```
#bash
git clone git@github.com:username/repo.git
```
当克隆第二个GitHub账号的仓库时,使用git@<font color="#dd0000">github_2.com</font>:

```
#bash 
git clone git@github_2.com:username/repo.git
```

这会自动使用对应的SSH密钥与GitHub账号连接。  
拉取成功后，为了方便，你可以在`cd`到对应的仓库目录下，用下述命令配置`user.name`和`user.email`

```
git config --local user.name "youname"
git config --local user.email "youemail@xx.com"
```
按照这些步骤,可以在一台Mac上配置任意多个GitHub账号,并可以方便地与之交互。