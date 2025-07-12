## 初始化 Git 仓库并上传至 GitHub

### 1. 初始化本地 Git 仓库
在项目根目录下执行以下命令：
```bash
git init
```
### 2. 创建 .gitignore 文件
在项目根目录下创建 `.gitignore` 文件，添加以下内容以忽略 build、log 和 install 目录：
```gitignore
# 构建目录
build/
dist/

# 日志目录
log/
logs/
*.log

# 安装目录
install/
node_modules/

# 系统文件
.DS_Store
Thumbs.db

# IDE 文件
.vscode/
.idea/
*.swp
*.swo

# 环境变量文件
.env
.env.local
.env.development.local
.env.test.local
.env.production.local
```

### 3. 添加文件到 Git 仓库
```bash
git add .
git commit -m "Initial commit"
```

### 4. 创建远程仓库
1. 登录 GitHub，点击右上角的 "+"，选择 "New repository"
2. 填写仓库名称（例如：control_car），选择仓库类型（Public 或 Private）
3. 点击 "Create repository"

### 5. 关联远程仓库并推送代码
将本地仓库与远程仓库关联：
```bash
git remote add origin https://github.com/yourusername/control_car.git
```

将代码推送到远程仓库：
```bash
git push -u origin main
```

**注意**：首次推送时，Git 会提示输入 GitHub 的用户名和密码。建议使用 GitHub 的 Personal Access Token（PAT）作为密码，以提高安全性。

## 日常开发流程

### 检查文件状态
```bash
git status
```

### 添加和提交更改
```bash
# 添加所有更改的文件
git add .

# 或者添加特定文件
git add filename.txt

# 提交更改
git commit -m "描述您的更改"
```

### 推送到远程仓库
```bash
git push origin main
```

### 拉取远程更改
```bash
git pull origin main
```

## 其他说明

### 忽略文件
`.gitignore` 文件用于指定 Git 应忽略的文件和目录，避免将不必要的文件提交到版本控制中。本项目忽略了以下类型的文件：
- 构建文件（build/、dist/）
- 日志文件（log/、logs/、*.log）
- 安装文件（install/、node_modules/）
- 系统和IDE生成的文件

### 协议说明

#### HTTPS 协议
使用 HTTPS 协议推送代码时，Git 会提示输入用户名和密码。为了提高安全性，建议使用 GitHub 的 Personal Access Token（PAT）作为密码。

**设置 PAT 的步骤：**
1. 登录 GitHub，进入 Settings → Developer settings → Personal access tokens
2. 点击 "Generate new token"
3. 选择所需的权限，点击 "Generate token"
4. 复制生成的 token，在推送时用作密码

#### SSH 协议
如果您希望使用 SSH 协议推送代码，需要先配置 SSH 密钥，并将公钥添加到 GitHub 的 SSH 设置中。

**配置 SSH 的步骤：**
```bash
# 生成 SSH 密钥
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# 将公钥添加到 GitHub
# 复制 ~/.ssh/id_rsa.pub 的内容到 GitHub Settings → SSH and GPG keys

# 测试 SSH 连接
ssh -T git@github.com

# 使用 SSH 协议添加远程仓库
git remote add origin git@github.com:yourusername/control_car.git
```

## 常见问题解决

### 推送被拒绝
```bash
# 先拉取远程更改
git pull origin main

# 解决冲突后再推送
git push origin main
```

### 清除Git缓存
```bash
# 清除所有文件的Git缓存
git rm -r --cached .

# 重新添加文件
git add .
git commit -m "Update .gitignore"
```

**提示**：请确保在推送代码前检查 `.gitignore` 文件是否正确配置，避免上传构建日志和依赖文件。