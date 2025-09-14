# Vercel 部署配置指南

## 部署问题修复

之前的部署错误是因为 Vercel 试图构建 `auth-proxy` 应用，但该应用没有构建脚本。现在已经修复了以下问题：

### 1. 修复的文件

#### `vercel.json`
```json
{
  "github": {
    "silent": true
  },
  "buildCommand": "bun run build --filter=@saasfly/nextjs",
  "outputDirectory": "apps/nextjs/.next",
  "installCommand": "bun install",
  "framework": "nextjs",
  "rootDirectory": "apps/nextjs"
}
```

#### `apps/auth-proxy/package.json`
添加了空的构建脚本：
```json
"build": "echo 'No build needed for auth-proxy'"
```

### 2. 在 Vercel 中配置环境变量

在 Vercel 项目设置中添加以下环境变量：

#### 必需的环境变量：
```
NEXT_PUBLIC_APP_URL=https://your-domain.vercel.app
NEXTAUTH_URL=https://your-domain.vercel.app
NEXTAUTH_SECRET=your-generated-secret-here
POSTGRES_URL=postgresql://...
STRIPE_API_KEY=sk_live_... (或 sk_test_...)
STRIPE_WEBHOOK_SECRET=whsec_...
GITHUB_CLIENT_ID=your-github-oauth-app-id
GITHUB_CLIENT_SECRET=your-github-oauth-app-secret
```

#### 可选的环境变量：
```
NEXT_PUBLIC_STRIPE_PRO_MONTHLY_PRICE_ID=price_...
NEXT_PUBLIC_STRIPE_PRO_YEARLY_PRICE_ID=price_...
NEXT_PUBLIC_STRIPE_BUSINESS_MONTHLY_PRICE_ID=price_...
NEXT_PUBLIC_STRIPE_BUSINESS_YEARLY_PRICE_ID=price_...
RESEND_API_KEY=re_...
RESEND_FROM=noreply@yourdomain.com
ADMIN_EMAIL=admin@yourdomain.com
```

### 3. 部署步骤

1. 确保所有必需的环境变量都已在 Vercel 中配置
2. 推送代码到 GitHub
3. Vercel 将自动触发部署

### 4. 常见问题

- **构建失败**: 确保所有必需的环境变量都已设置
- **数据库连接错误**: 检查 `POSTGRES_URL` 是否正确
- **Stripe 错误**: 确保 Stripe 密钥和 webhook 密钥正确配置

### 5. 本地开发

创建 `.env.local` 文件并添加所需的环境变量（参考上面的列表）。
