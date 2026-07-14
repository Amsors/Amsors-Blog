# GitHub 与 Cloudflare Pages 部署

## 1. 推送到 GitHub

当前仓库已配置远程地址 `https://github.com/Amsors/Amsors-Blog`。先在 GitHub 确认该仓库已创建，且不要再勾选生成 README、License 或 `.gitignore`。

```bash
git add .
git commit -m "feat: initialize Astro Fuwari blog"
git push -u origin main
```

推送后在 GitHub 的 `Actions` 页面确认 `CI` 工作流通过。仓库为公开项目时，可在 `Settings > Rules > Rulesets` 为 `main` 添加分支规则：禁止 force push，并要求 `build` 检查通过后才能合并。个人博客若习惯直接推送 `main`，可只禁止 force push，不强制 PR。

## 2. 创建 Cloudflare Pages 项目

1. 登录 Cloudflare Dashboard，进入 `Workers & Pages`。
2. 选择 `Create application > Pages > Connect to Git`。界面文案可能小幅调整，关键是创建 **Pages** 的 Git 集成项目。
3. 授权 `Cloudflare Workers and Pages` GitHub App，建议只授权 `Amsors/Amsors-Blog`。
4. 选择仓库并设置：

| 项目 | 值 |
| --- | --- |
| Project name | `amsors-blog` |
| Production branch | `main` |
| Framework preset | `Astro` |
| Build command | `pnpm build` |
| Build output directory | `dist` |
| Root directory | `/` 或留空 |

5. 在生产环境变量中设置：

| 变量 | 值 | 说明 |
| --- | --- | --- |
| `NODE_VERSION` | `22` | 与 `.node-version` 保持一致 |
| `PUBLIC_SITE_URL` | `https://amsors-blog.pages.dev/` | 用于 canonical URL、Sitemap 和 RSS |

6. 点击部署。成功后检查首页、`/about/`、`/archive/`、`/rss.xml` 和 `/sitemap-index.xml`。

Cloudflare Git 集成会在每次推送 `main` 时发布生产站点，其他分支和 Pull Request 则生成独立预览地址。

## 3. 绑定自定义域名

1. 进入 Pages 项目的 `Custom domains > Set up a domain`。
2. 输入根域名（如 `example.com`）或子域名（如 `blog.example.com`）。
3. 根域名需将 nameserver 托管给 Cloudflare。子域名使用外部 DNS 时，先在 Pages 中添加域名，再按提示添加指向 `amsors-blog.pages.dev` 的 CNAME；不要只手工添加 CNAME。
4. 在 `Settings > Environment variables` 将生产环境的 `PUBLIC_SITE_URL` 改为最终地址，例如 `https://blog.example.com/`。
5. 在 `Deployments` 对最新提交选择 `Retry deployment`，使 Sitemap、RSS 和 canonical URL 重新生成。

如果同时使用根域名和 `www`，选定一个主地址，再通过 Cloudflare Redirect Rules 将另一个做 301 跳转，并把 `PUBLIC_SITE_URL` 设为主地址。

## 4. 部署故障排查

- 依赖安装失败：确认 Cloudflare 使用 Node 22，并且仓库中有 `pnpm-lock.yaml`。
- 页面是旧内容：先确认最新提交已建立新 deployment，再检查生产分支是否为 `main`。
- Sitemap 中域名不对：修正 `PUBLIC_SITE_URL` 并重新部署。
- GitHub 仓库不可见：在 GitHub `Settings > Applications` 检查 `Cloudflare Workers and Pages` 对该仓库的授权。
- 自定义域名未激活：确认已先在 Pages 项目中添加该域名，再检查 DNS 与 CAA 记录。
