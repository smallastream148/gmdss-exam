# Cloudflare Pages 部署指南

## 推荐方案:Git 自动部署

### 步骤 1:创建 GitHub 仓库

1. 登录 https://github.com
2. 点击右上角 `+` → `New repository`
3. 填写:
   - **Repository name**:`gmdss-exam`(或您喜欢的名字)
   - **Description**:GMDSS 整合讲评
   - **Public**(公开,免费 Cloudflare Pages 需要)
   - **不要**勾选 Add a README / .gitignore / license(我们本地已有)
4. 点 `Create repository`

### 步骤 2:推送代码到 GitHub

复制 GitHub 给出的命令,在本项目目录下执行:

```bash
git init
git add .
git commit -m "Initial commit: GMDSS 整合讲评"
git branch -M main
git remote add origin https://github.com/你的用户名/gmdss-exam.git
git push -u origin main
```

### 步骤 3:Cloudflare Pages 连接 Git

1. 登录 https://dash.cloudflare.com/
2. 左侧菜单 → **Workers & Pages** → **Pages** → **Create application** → **Pages** → **Connect to Git**
3. 选择 **GitHub** → 授权 Cloudflare 访问您的 GitHub
4. 选仓库 `gmdss-exam`
5. **Project name**:`gmdss-exam`(会成为 `gmdss-exam.pages.dev`)
6. **Build settings**:
   - **Framework preset**:`None`(纯静态)
   - **Build command**:留空
   - **Build output directory**:`/`(项目根目录)
7. 点 **Save and Deploy**

### 步骤 4:等待部署

- 通常 1–3 分钟
- 完成后 Cloudflare 会发邮件
- 访问 `https://gmdss-exam.pages.dev` 即可

### 步骤 5:绑定自定义域名(可选)

1. Cloudflare Pages → 项目 → **Custom domains** → **Set up a custom domain**
2. 输入您的域名(如 `gmdss.example.com`)
3. 按提示在您的域名 DNS 添加 CNAME 记录
4. Cloudflare 自动签发 HTTPS 证书

---

## 备选方案:Wrangler CLI 直传

适合不想用 Git 的场景。

```bash
npm install -g wrangler
wrangler login
wrangler pages deploy . --project-name=gmdss-exam
```

---

## 备选方案:Dashboard 拖拽上传

1. Cloudflare Dashboard → **Workers & Pages** → **Pages** → **Create application** → **Pages** → **Upload assets**
2. **Project name**:`gmdss-exam`
3. 拖整个项目文件夹(包含 audio/ data/ 三个 html)到上传区
4. 点 **Deploy site**

---

## 🔧 常见问题

### Q1:部署后页面打开是 404?

确认 **Build output directory** 填的是 `/`,Cloudflare 会从仓库根目录找 `index.html`。

### Q2:音频播放不了?

- 检查浏览器 Network,看 mp3 请求是否 404
- 如果文件太大(>25MB),需要拆分或压缩
- 我们最大单个 mp3 < 1MB,应该没问题

### Q3:iframe 不显示?

- 确认两个子页面文件名无中文错误
- 浏览器 F12 → Console 看跨源错误
- 两个页面在同一域名下,不应有跨源问题

### Q4:更新后页面没变?

- Cloudflare 默认缓存,可能要等 1-5 分钟
- 或在 Cloudflare Pages 项目 → **Deployments** → 最新部署 → **Retry deployment**

---

## 💡 优化建议(可选)

### 1. 压缩音频(节省 ~50% 体积)

把 mp3 转 opus:

```bash
ffmpeg -i input.mp3 -c:a libopus -b:a 32k output.opus
```

但 Cloudflare Pages 静态托管单个 mp3 < 25MB 限制已满足,可不动。

### 2. 添加 _headers 文件

创建 `_headers` 文件自定义 HTTP 头:

```
/audio/*
  Cache-Control: public, max-age=31536000, immutable

/*
  X-Content-Type-Options: nosniff
```

### 3. 添加 _redirects

如需 www → 主域 重定向:

```
https://www.example.com/* https://example.com/:splat 301
```