---
name: github-publish-skills
description: 将 Hermes Agent skills 发布到 GitHub 仓库的完整流程。包含认证、仓库创建、文件推送。
category: github
---

# 发布 Skills 到 GitHub

## 前置条件

### Token 类型（关键！）

| Token 类型 | 前缀 | 权限 | 推荐 |
|-----------|------|------|------|
| **Classic PAT** | `ghp_` | 完整 scopes，可创建仓库、推送 | ✅ 必须 |
| **Fine-grained PAT** | `github_pat_` | X-OAuth-Scopes 显示 `none`，API 返回 403 | ❌ 不可用 |

**创建 Classic PAT**: https://github.com/settings/tokens → Tokens (classic) → Generate new token (classic)
- 勾选 `repo` scope
- 有效期 90 天

### 验证 token 权限

```python
import urllib.request, json, ssl

token = "ghp_xxx"
ctx = ssl.create_default_context()
req = urllib.request.Request(
    "https://api.github.com/user",
    headers={
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3+json",
        "User-Agent": "Hermes-Agent"
    }
)
with urllib.request.urlopen(req, timeout=30, context=ctx) as resp:
    scopes = resp.headers.get("X-OAuth-Scopes", "none")
    data = json.loads(resp.read())
    print(f"User: {data['login']}, Scopes: {scopes}")
```

## 网络坑点：京东云 curl 超时

**现象**: 京东云服务器上 `curl` 访问 `api.github.com:443` TLS 握手超时，但 `ping` 可达。`gh auth login` 也会超时。

**解决**: 用 Python `urllib` 替代 `curl` 调用 GitHub API。git push 走 git protocol 可能不受影响。

## 发布流程

```python
import urllib.request, json, ssl, os, subprocess, shutil

token = "ghp_xxx"
ctx = ssl.create_default_context()
username = "your-username"
repo_name = "hermes-skills"

# 1. 创建仓库
repo_data = json.dumps({
    "name": repo_name,
    "description": "My custom Hermes Agent skills",
    "private": False,
    "auto_init": True
}).encode()

req = urllib.request.Request(
    f"https://api.github.com/user/repos",
    data=repo_data,
    headers={
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3+json",
        "User-Agent": "Hermes-Agent"
    }
)

try:
    with urllib.request.urlopen(req, timeout=30, context=ctx) as resp:
        data = json.loads(resp.read())
        print(f"✅ 仓库创建成功: {data['html_url']}")
except urllib.error.HTTPError as e:
    body = e.read().decode()
    if "already exists" in body:
        print("⚠️ 仓库已存在")
    else:
        raise

# 2. 配置 git
os.system(f'git config --global user.name "{username}"')
auth_url = f"https://{username}:{token}@github.com"
with open(os.path.expanduser("~/.git-credentials"), "w") as f:
    f.write(auth_url + "\n")
os.chmod(os.path.expanduser("~/.git-credentials"), 0o600)
os.system('git config --global credential.helper store')

# 3. 克隆
repo_dir = "/tmp/hermes-skills-publish"
os.system(f"rm -rf {repo_dir}")
embed_url = f"https://{username}:{token}@github.com/{username}/{repo_name}.git"
subprocess.run(["git", "clone", embed_url, repo_dir], capture_output=True, timeout=60)

# 4. 复制 skills
skills_to_publish = ["dogfood", "openclaw-deploy", "openclaw-upgrade-502-fix"]
for name in skills_to_publish:
    src = f"/root/.hermes/skills/{name}"
    dst = os.path.join(repo_dir, name)
    if os.path.exists(dst):
        shutil.rmtree(dst)
    if os.path.exists(src):
        shutil.copytree(src, dst)
        print(f"✅ {name}")

# 5. 添加 README
readme = f"""# Hermes Skills

个人自定义 Hermes Agent skills 集合。

## 使用方法
```bash
git clone https://github.com/{username}/{repo_name}.git
cp -r {repo_name}/skill-name ~/.hermes/skills/
```
"""
with open(os.path.join(repo_dir, "README.md"), "w") as f:
    f.write(readme)

# 6. 提交推送
subprocess.run(["git", "add", "."], cwd=repo_dir, capture_output=True)
subprocess.run(["git", "commit", "-m", "publish skills"], cwd=repo_dir, capture_output=True)
result = subprocess.run(["git", "push"], cwd=repo_dir, capture_output=True, text=True, timeout=60)
print(f"Push: {'✅' if result.returncode == 0 else '❌'} {result.stderr.strip()}")
```

## 验证

```python
req = urllib.request.Request(
    f"https://api.github.com/repos/{username}/{repo_name}/contents/",
    headers={"Authorization": f"token {token}", "Accept": "application/vnd.github.v3+json"}
)
with urllib.request.urlopen(req, timeout=30, context=ctx) as resp:
    files = json.loads(resp.read())
    for f in files:
        print(f"  📁 {f['name']} ({f['type']})")
```

## 常见错误

| 错误 | 原因 | 解决 |
|------|------|------|
| 403 "Resource not accessible by personal access token" | 用了 fine-grained PAT | 改用 Classic PAT (ghp_) |
| curl 超时 | 服务器网络限制 | 用 Python urllib |
| gh auth login 超时 | 同上 | 跳过 gh，直接用 git + token |
