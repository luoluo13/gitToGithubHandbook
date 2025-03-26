# Git 与 GitHub 使用指南

本文档详细介绍如何使用 Git 与 GitHub 进行项目管理和协作开发，包含从基本指令到多人协作规则的说明。

---

## 一、在本地连接 GitHub 账户

为了在本地使用git管理工具操作 GitHub 仓库，首先需要配置一个 GitHub 账户，以下将介绍通过 **HTTPS** 和 **SSH** 两种方式连接 GitHub。

### 1.1 通过 HTTPS 方式连接

这是使用 `.git` 链接连接到 GitHub 的方式。这种方式较为简单，但需要每次推送代码时输入用户名和密码（或通过 GitHub CLI 工具免密认证）。

#### **步骤：**
1. 配置全局用户名和邮箱
   ```bash
   git config --global user.name "你的GitHub用户名"
   git config --global user.email "你的GitHub注册邮箱"
   ```

2. 克隆远程仓库到本地
   复制 GitHub 仓库的 `.git` 地址（如 `https://github.com/your-username/repository.git`），通过以下命令克隆到本地：
   
   ```bash
   git clone https://github.com/your-username/repository.git
   ```
   
3. 为了避免每次都输入用户名和密码，可以在本地通过 GitHub CLI 或 GitHub Token 配置免密操作（[参考 GitHub 官方指南](https://docs.github.com/en/authentication)）。

---

### 1.2 通过 SSH 方式连接

使用 SSH 方式可以更加安全地推送和拉取代码，而且只需要设置一次，无需频繁输入用户名和密码。

#### **主要步骤：**
1. 检查是否有 SSH Key
   在终端执行：
   ```bash
   ls ~/.ssh
   ```
   如果看到了 **id_rsa** 和 **id_rsa.pub**，说明你已经有 SSH Key。如果没有，执行以下步骤生成 SSH Key。

2. 创建新的 SSH Key
   执行以下命令生成 SSH Key：
   ```bash
   ssh-keygen -t ed25519 -C "你的GitHub注册邮箱"
   ```
   按提示操作，会生成一个 SSH 密钥对，默认保存在 `~/.ssh/id_ed25519`。

3. 添加 SSH Key 到 GitHub
   执行以下命令复制公钥内容：
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```
   - 将复制的内容添加到 GitHub 设置：
     打开 GitHub -> **Settings -> SSH and GPG keys -> New SSH key**，然后粘贴。

4. 测试 SSH 连接
   运行以下命令测试是否设置成功：
   ```bash
   ssh -T git@github.com
   ```
   如果显示类似以下内容，说明配置成功：
   ```
   Hi your-username! You've successfully authenticated, but GitHub does not provide shell access.
   ```

5. 克隆仓库
   使用 SSH 克隆远程仓库：
   ```bash
   git clone git@github.com:your-username/repository.git
   ```

---

## 二、Git 的基础指令用法

以下是一些常用的基础操作指令汇总：

### 2.1 初始化本地仓库
```bash
git init
```
在项目文件夹中运行此命令，会初始化一个 Git 仓库。此仓库会记录文件变更并方便管理版本。

---

### 2.2 添加文件到暂存区
```bash
git add <文件名>       # 添加某个文件到暂存区
git add .              # 添加当前目录下的所有文件到暂存区
```

---

### 2.3 提交文件到本地仓库
```bash
git commit -m "说明性提交信息"
```
添加文件到暂存区后使用 `git commit` 将其提交到本地仓库。

---

### 2.4 关联远程仓库
```bash
git remote add origin <仓库地址>
```
运行该命令可将当前本地仓库与 GitHub 远程仓库关联。

---

### 2.5 拉取远程分支代码
```bash
git pull origin <分支名>
```
拉取远程仓库的代码并与当前分支进行合并。

---

### 2.6 推送代码到远程仓库
```bash
git push origin <分支名>
```
将本地代码推送到远程仓库的目标分支上。

---

### 2.7 查看分支
```bash
git branch           # 查看本地分支
git branch -r        # 查看远程分支
git branch -a        # 查看本地和远程的所有分支
```

---

### 2.8 创建并切换新分支
```bash
git branch <分支名>      # 创建新分支
git checkout <分支名>    # 切换到指定分支
git checkout -b <分支名> # 创建新分支并立即切换到该分支
```

---

### 2.9 合并分支
```bash
git merge <来源分支名>
```
在当前分支中运行该命令会将 `来源分支` 的代码合并到当前分支中。

---

### 2.10 查看提交历史
```bash
git log
```
查看提交历史及详细的提交记录。

---

### 2.11 检查当前文件状态
```bash
git status
```
检测目前哪些文件修改了，哪些文件未提交。

---

## 三、多人协作开发规则

在多人协作中，需要约定操作规则，以保证代码整洁和团队协作流畅。以下是团队协作的具体规则：

### 3.1 分支管理规则
1. **主分支（`main`）**：
   - 作为稳定版本的分支，包含经过测试和确认无误的代码。
   - 只有项目管理员（即主创）有权合并到 `main` 分支，普通协作者无权限直接修改。

2. **个人分支**：
   - 每个协作者创建自己的分支（以用户名或任务命名，如 `feature/username`）。
   - 每个协作者仅在自己的分支中开发，每周一从 `main` 分支拉取最新代码以保持更新。

3. **Pull Request (PR)**：
   - 每周协作者提交拉取请求（Pull Request），由主创于周六审核代码并合并到 `main` 分支。

---

### 3.2 团队协作工作流

1. **主创创建远程仓库和 `main` 分支**：
   - 主创初始化远程仓库并创建 `main` 分支。
   - 启用分支保护规则，禁止协作者直接推送到 `main`。

2. **协作者创建并管理自己的分支**：
   ```bash
   # 从远程仓库克隆代码
   git clone git@github.com:project-owner/repository.git
   
   # 切换到自己的分支（提前从 main 拉取最新代码）
   git checkout -b feature/username main
   ```

3. **开发代码并提交**：
   - 协作者在自己的分支上进行功能开发；
   - 提交代码到自己的分支：
     ```bash
     git add .
     git commit -m "开发了某某功能"
     git push origin feature/username
     ```

4. **主创同步最新代码并合并**：
   - 协作者每周提交 Pull Request 请求合并到 `main`；
   - 主创在 GitHub 上审核代码，通过后合并到 `main` 分支。

---

### 3.3 示例操作流程

1. 协作者从 `main` 拉取最新代码：
   ```bash
   git fetch origin
   git checkout main
   git pull origin main
   ```

2. 合并 `main` 到个人分支：
   ```bash
   git checkout feature/username
   git merge main
   ```

3. 开发完成后提交代码并发起 Pull Request：
   ```bash
   # 提交到个人分支
   git push origin feature/username
   
   # 发起 PR（通过 GitHub Web 界面进行）
   ```

4. 主创在 GitHub 上审核合并代码，并部署到 `main`。

---

### 3.4 分支保护配置

在 GitHub 上，可设置如下规则来保护 `main` 分支：
1. 打开 GitHub 仓库页面，进入 `Settings -> Branches`。
2. 添加 `Branch protection rules`：
   - 勾选 `Require a pull request before merging` 禁止直接推送到 `main`。
   - 勾选 `Require approvals` 需要主创或管理员审核后才能合并。



### 0.0 附记（非新手向，仅用作备忘）

####		20250326

		1. 使用git commit -m时，若在此之前使用了git add .命令，则-m参数会作用于工作目录中的所有文件。为了避免污染提交记录，请尽可能避免这两个代码同时使用！
		1. 如果在https连接方式下使用git出现connect reset情况，建议改为ssh连接（git remote set-url origin git@github.com:username/repo.git）。
