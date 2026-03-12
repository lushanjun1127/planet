# GitHub Secrets 配置指南

## 📋 必须配置的密钥

为了让 GitHub Actions 能够自动部署到 Cloudflare Pages，你需要在 GitHub 仓库中配置以下密钥。

### 步骤 1：获取 Cloudflare API Token

1. 访问 https://dash.cloudflare.com/profile/api-tokens
2. 点击 "创建令牌"
3. 选择 "自定义令牌"
4. 配置以下权限：

**账户资源权限：**
- Account Settings - 读取
- Workers Scripts - 编辑
- D1 - 编辑
- KV Storage - 编辑  
- Pages - 编辑

5. 点击 "继续显示摘要"
6. 复制生成的 API Token（只显示一次，请妥善保管）

### 步骤 2：获取 Cloudflare Account ID

1. 访问 https://dash.cloudflare.com
2. 登录后，在右侧边栏找到你的账户
3. 点击账户名称进入账户主页
4. 在右侧可以找到 "账户 ID"
5. 复制这个 ID

或者从已部署的 Worker 页面获取：
- 访问任何 Worker 的详情页面
- 在 URL 中可以看到账户 ID

### 步骤 3：在 GitHub 配置 Secrets

1. 访问你的 GitHub 仓库：https://github.com/lushanjun1127/planet
2. 点击 "Settings" 标签页
3. 在左侧菜单选择 "Secrets and variables" → "Actions"
4. 点击 "New repository secret" 按钮

添加以下两个密钥：

#### 密钥 1: CLOUDFLARE_API_TOKEN
- **Name**: `CLOUDFLARE_API_TOKEN`
- **Value**: （你在步骤 1 中复制的 API Token）

#### 密钥 2: CLOUDFLARE_ACCOUNT_ID
- **Name**: `CLOUDFLARE_ACCOUNT_ID`
- **Value**: `95d6ce6b1fb1d75ac6927acf9738aa14`

5. 点击 "Add secret" 保存

## ✅ 验证配置

配置完成后，GitHub Secrets 应该包含：

```
CLOUDFLARE_API_TOKEN    (你的 API Token)
CLOUDFLARE_ACCOUNT_ID   95d6ce6b1fb1d75ac6927acf9738aa14
```

## 🚀 触发部署

配置好密钥后，推送代码到 main分支会自动触发部署：

```bash
git push origin main
```

或者手动触发：
1. 访问 https://github.com/lushanjun1127/planet/actions
2. 点击 "Deploy to Cloudflare Pages" 工作流
3. 点击 "Run workflow" 按钮
4. 选择 main分支
5. 点击 "Run workflow"

## 📊 查看部署状态

1. 访问 Actions 页面：https://github.com/lushanjun1127/planet/actions
2. 查看正在运行的工作流
3. 点击具体的运行记录查看详细日志

成功部署后，你会看到绿色的 ✓ 标记。

## 🔗 访问部署后的网站

部署成功后，Cloudflare Pages 会提供以下访问地址：

- **生产环境**: `https://planet.lushanjun.pages.dev`
- **预览环境**: 每次 PR 都会生成独立的预览 URL

## ⚠️ 常见问题

### 问题 1: DNS_PROBE_FINISHED_NXDOMAIN

**原因**: 部署还未完成或项目未创建

**解决方法**:
1. 检查 GitHub Actions 是否运行成功
2. 等待 3-5 分钟让 CDN 同步
3. 如果是首次部署，需要手动在 Cloudflare 创建 Pages 项目

### 问题 2: 部署失败 - 认证错误

**原因**: API Token 配置不正确

**解决方法**:
1. 检查 Secret 中的 API Token 是否正确
2. 确认 Token 有足够的权限
3. 重新生成 Token 并更新 Secret

### 问题 3: 部署失败 - 找不到项目

**原因**: Pages 项目还未创建

**解决方法**:
首次部署需要手动操作：
1. 访问 https://dash.cloudflare.com
2. 进入 Workers & Pages
3. 点击 "创建应用程序" → "Pages"
4. 选择 "连接到 Git"
5. 选择 `lushanjun1127/planet` 仓库
6. 配置构建设置：
   - 构建命令：`npm run build`
   - 构建输出目录：`dist`
7. 点击 "保存并部署"

## 💡 优化建议

### 1. 首次部署建议手动创建项目

虽然 GitHub Actions 可以自动部署，但首次建议通过 Cloudflare Dashboard 手动创建项目：

1. 这样可以可视化配置所有设置
2. 可以确认 D1 数据库和 KV 命名空间绑定正确
3. 可以设置自定义域名等高级功能

### 2. 后续更新使用自动部署

首次配置好后，之后的所有推送到 main分支都会自动部署。

### 3. 使用 wrangler.toml 本地测试

在本地可以使用 wrangler 测试：

```bash
npm run preview
```

这会在本地模拟 Cloudflare Pages 环境。

## 📞 获取帮助

如果遇到问题：
1. 查看 GitHub Actions 日志
2. 查看 Cloudflare Dashboard 的部署日志
3. 检查 wrangler.toml 配置是否正确

---

**配置完成后，再次推送代码就会自动部署了！** 🚀
