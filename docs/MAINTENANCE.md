# 博客写作与日常维护

## 发布一篇文章

1. 使用稳定的英文 slug 创建草稿：

   ```bash
   pnpm new-post astro-notes
   ```

2. 编辑 `src/content/posts/astro-notes.md`。完整 Frontmatter 示例：

   ```yaml
   ---
   title: 'Astro 学习笔记'
   published: 2026-07-14
   updated: 2026-07-15
   description: '用一句可独立阅读的话说明文章内容。'
   image: './cover.webp'
   tags: [Astro, Frontend]
   category: 前端
   draft: false
   lang: ''
   ---
   ```

3. 如果文章有配图，建议改用目录结构：

   ```text
   src/content/posts/astro-notes/
   ├── index.md
   ├── cover.webp
   └── architecture.webp
   ```

   在 `index.md` 中使用 `./cover.webp`。优先使用 WebP/AVIF，封面建议约 1200×630，避免提交过大原图。

4. 发布前检查：

   ```bash
   pnpm check
   pnpm build
   pnpm preview
   ```

5. 提交并推送：

   ```bash
   git add src/content/posts/astro-notes.md
   git commit -m "post: add Astro notes"
   git push
   ```

使用分支写长文时，可推送分支并建 Pull Request。Cloudflare Pages 会提供预览地址，合并到 `main` 后再正式发布。

## 草稿、更新和撤稿

- `draft: true`：生产构建不显示文章。新建文章默认为草稿。
- 更新旧文时保留 `published`，新增或修改 `updated`。
- 临时撤稿可改为 `draft: true`；需要保留旧 URL 时不要改文件路径。
- 删除或改名已被收录的文章会产生 404。确有需要时，在 `public/_redirects` 添加永久跳转：

  ```text
  /posts/old-slug/ /posts/new-slug/ 301
  ```

## 站点配置

- `src/config.ts`：标题、副标题、语言、主题色、导航、头像、社交链接和文章许可证。
- `src/content/spec/about.md`：关于页。
- `astro.config.mjs`：构建与 Markdown 扩展。站点地址优先由 `PUBLIC_SITE_URL` 提供。
- `public/favicon/`：站点图标。替换时保持现有文件名和尺寸，或同步修改 `src/config.ts` 的 `favicon`。

## 依赖升级

Dependabot 已配置检查 npm 和 GitHub Actions 更新。升级时不要一次合并大量主版本：

1. 先看 release notes 和 Fuwari 上游变更。
2. 在独立分支更新并保留 `pnpm-lock.yaml`。
3. 运行 `pnpm check && pnpm build`。
4. 用 Cloudflare 预览地址检查首页、文章页、深色模式、搜索、RSS 和移动端。
5. 再合并到 `main`。

## 定期检查清单

每月或每次大改后检查：

- GitHub Actions 和 Cloudflare Pages 最新构建为绿色。
- `rss.xml`、`sitemap-index.xml` 中域名正确。
- 搜索能找到最新文章。
- 站内链接、图片和 GitHub 卡片没有失效。
- 自定义域名证书正常，`PUBLIC_SITE_URL` 未退回 `pages.dev`。
- Dependabot 报警已评估，高风险依赖已优先处理。
