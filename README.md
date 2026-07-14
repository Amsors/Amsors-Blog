# Amsors' Blog

基于 Astro + Fuwari 的中文静态博客，使用 GitHub 管理内容，由 Cloudflare Pages 自动构建发布。

## 本地开发

需要 Node.js 22 和 pnpm 9.14.4。

```bash
pnpm install
pnpm dev
```

打开 `http://localhost:4321` 预览。提交前运行：

```bash
pnpm check
pnpm build
pnpm preview
```

## 写作

```bash
pnpm new-post my-article
```

文章会生成在 `src/content/posts/my-article.md`，默认是草稿。写完后补全 Frontmatter，并把 `draft` 改为 `false`。

站点名称、导航、个人资料和主题色在 `src/config.ts` 中修改。

## 文档

- [GitHub 与 Cloudflare Pages 部署](docs/DEPLOYMENT.md)
- [博客写作与日常维护](docs/MAINTENANCE.md)

## 常用命令

| 命令 | 作用 |
| --- | --- |
| `pnpm dev` | 启动本地开发服务器 |
| `pnpm new-post <slug>` | 创建草稿 |
| `pnpm check` | 执行 Astro/TypeScript 检查 |
| `pnpm build` | 生成 `dist` 并建立 Pagefind 搜索索引 |
| `pnpm preview` | 本地预览生产构建 |
| `pnpm format` | 格式化 `src` |

Fuwari 主题代码遵循仓库中的 MIT License；博客文章默认使用 CC BY-NC-SA 4.0。
