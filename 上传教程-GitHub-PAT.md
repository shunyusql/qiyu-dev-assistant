# 上传 Skill 到 GitHub 完整教程

本教程教你把 `qiyu-dev-assistant` skill 上传到 GitHub。

## 步骤总览

```
1. 生成 PAT 令牌（一次性）
2. 在 GitHub 创建仓库
3. 修改推送脚本并执行
4. 验证推送成功
```

---

## 第 1 步：生成 PAT 令牌（Personal Access Token）

PAT 相当于 GitHub 的"应用密码"，用于命令行推送。

1. 登录 [github.com](https://github.com)，点击右上角**头像** → **Settings（设置）**

2. 在左侧菜单最底部，点击 **Developer settings（开发者设置）**

3. 左侧展开 **Personal access tokens** → 点击 **Tokens (classic)**

4. 点击右上角 **Generate new token** → 选择 **Generate new token (classic)**

5. 填写表单：
   - **Note（备注）**：随便写，例如 `push skill`
   - **Expiration（过期时间）**：建议选 `90 days`（90天）
   - **Select scopes（权限）**：**勾选 `repo`**（完整勾选，这是推送所必需的权限）

6. 点击页面底部绿色按钮 **Generate token（生成令牌）**

7. **立即复制**生成的令牌（形如 `ghp_xxxxxxxxxxxxxxxxxxxx`）——**它只显示这一次**，关掉页面就看不到了，只能重新生成

> ⚠️ 重要安全提醒：
> - PAT 等同于你的 GitHub 密码，**不要发给任何人**，不要截图发群里
> - 如果泄露，去同一页面点 **Delete** 删除再重新生成
> - 推送脚本会把 PAT 写进本地 `.git/config`，仅存在你自己电脑上

---

## 第 2 步：在 GitHub 创建仓库

1. 打开 [github.com/new](https://github.com/new)

2. 填写：
   - **Repository name（仓库名）**：`qiyu-dev-assistant`
   - 可见性（Visibility）：选 **Public（公开）** 或 **Private（私有）**，随意

3. **注意**：**不要**勾选 "Add a README file"、"Add .gitignore"、"Choose a license" 这三项（保持空仓库），否则推送会冲突

4. 点击 **Create repository（创建仓库）**

---

## 第 3 步：修改推送脚本并执行

1. 用记事本打开 `C:\Users\admin\Desktop\测试\push-to-github.ps1`

2. 修改文件顶部的三个变量：

```powershell
$GITHUB_USERNAME = "你的GitHub用户名"     # 例如 zhangsan
$REPO_NAME       = "qiyu-dev-assistant"   # 与第2步建的仓库名一致
$PAT_TOKEN       = "ghp_xxxxxxxxxxxx"     # 第1步复制的令牌
```

3. 保存文件

4. 在 PowerShell 中执行（右键开始菜单 → Windows PowerShell）：

```powershell
powershell -ExecutionPolicy Bypass -File "C:\Users\admin\Desktop\测试\push-to-github.ps1"
```

---

## 第 4 步：验证

脚本运行后，如果看到：

```
推送成功！你的 skill 已发布到：
  https://github.com/你的用户名/qiyu-dev-assistant
```

就成功了！打开这个网址即可看到你的 skill 文件。

---

## 常见问题

| 问题 | 解决方法 |
|------|---------|
| 推送失败：仓库不存在 | 先去 github.com/new 创建同名空仓库 |
| 推送失败：权限不足 | PAT 生成时没勾 `repo` 权限，重新生成 |
| 推送失败：认证失败 | PAT 拼错 / 过期 / 用户名写错 |
| 提示 remote origin already exists | 脚本会自动先移除旧 origin，重跑即可 |
| 想改提交者姓名 | 改脚本里的 user.name / user.email 两行 |

---

## 以后想更新 skill

每次改完文件后，重新执行：

```powershell
powershell -ExecutionPolicy Bypass -File "C:\Users\admin\Desktop\测试\push-to-github.ps1"
```

脚本会自动提交新改动并推送（脚本只处理初始提交和推送，如需补充提交命令可手动执行 `git add -A && git commit -m "更新" && git push`）。
