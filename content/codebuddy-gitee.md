---
title: CodeBuddy 连接 Gitee 仓库指南
date: 2026-02-11
---

在 CodeBuddy（或类似的云端 IDE/远程开发环境）中连接 Gitee 仓库，最推荐且最稳定的方式是使用 **SSH 密钥**。这样可以避免每次推送代码都输入密码，且比 HTTPS 更安全。

以下是详细的操作步骤：

## 核心步骤概览

1. **在 CodeBuddy 中生成 SSH 密钥**
2. **将公钥添加到 Gitee 账户**
3. **在 CodeBuddy 中通过 SSH 地址克隆或关联仓库**

---

## 第一步：在 CodeBuddy 中生成 SSH 密钥

1. 打开你的 CodeBuddy 工作区，找到 **Terminal（终端）** 窗口。
2. 输入以下命令生成密钥（请将邮箱替换为你 Gitee 注册的邮箱）：

```
ssh-keygen -t rsa -C "your_email@example.com"
```

3. **一路按回车**（Enter）即可。当提示设置 passphrase 时，为了方便通常直接回车留空。
4. 查看生成的**公钥**内容，输入以下命令并复制输出的全部内容（以 `ssh-rsa` 开头的内容）：

```
cat ~/.ssh/id_rsa.pub
```

## 第二步：将公钥添加到 Gitee (码云)

1. 登录你的 [Gitee](https://gitee.com/) 账号。
2. 点击右上角头像，选择 **设置 (Settings)**。
3. 在左侧菜单栏找到 **安全设置** -> **SSH公钥**。
4. **添加公钥**：

- **标题**：可以随便填（例如：`CodeBuddy`）。
- **公钥**：将刚才在 CodeBuddy 终端里复制的那串 `ssh-rsa ...` 内容粘贴进去。

1. 点击确定，并按提示输入 Gitee 登录密码验证。

## 第三步：验证连接

回到 CodeBuddy 的终端，输入以下命令测试是否连接成功：

```
ssh -T git@gitee.com
```

- 如果这是第一次连接，它会提示 `Are you sure you want to continue connecting?`，请输入 `yes` 并回车。
- 如果看到 `Hi XXX! You've successfully authenticated...`，说明连接成功！

---

## 第四步：拉取或关联代码

根据你的实际情况，选择以下一种方式操作：

### 情况 A：这是一个新环境，你想把 Gitee 上的代码拉下来

1. 在 Gitee 仓库页面，点击 **克隆/下载**，选择 **SSH** 选项，复制地址（类似 `git@gitee.com:username/project.git`）。
2. 在 CodeBuddy 终端输入：

```
git clone git@gitee.com:你的用户名/你的仓库名.git
```

### 情况 B：CodeBuddy 里已经有代码，想推送到 Gitee 新仓库

1. 在 Gitee 上新建一个空仓库。
2. 在 CodeBuddy 终端进入你的项目目录，依次执行：

```
# 初始化 git (如果还没初始化)
git init

# 关联远程仓库 (使用 SSH 地址)
git remote add origin git@gitee.com:你的用户名/你的仓库名.git

# 推送代码
git add .
git commit -m "第一次提交"
git push -u origin master
# 注意：如果 Gitee 默认分支是 main，请把 master 换成 main
```

## 常见问题提示

- **为什么要配置 Git 用户名？**  
    如果是新环境，提交代码前最好配置一下你的身份，否则提交记录里没有名字：

```
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

- **权限拒绝 (Permission denied)？**  
    如果执行 `ssh -T` 失败，请检查是否完整复制了公钥，或者 CodeBuddy 的终端是否有多套 SSH 密钥（需配置 `~/.ssh/config`，但通常默认情况下不需要）。
