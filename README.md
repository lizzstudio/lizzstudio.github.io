# 木子里的小店

基于 Astro 4.x + TypeScript 的静态小店，部署在 Cloudflare Pages。

## 技术栈

- **框架**：Astro 4.16+
- **语言**：TypeScript（strict 模式）
- **内容**：MDX 博客（`@astrojs/mdx`）
- **SEO**：`@astrojs/sitemap` + `@astrojs/rss`
- **部署**：Cloudflare Pages（静态站点，`wrangler` 兼容）

## 本地开发

```bash
# 1. 安装依赖
npm install

# 2. 启动开发服务器（默认 http://localhost:4321）
npm run dev

# 3. 构建生产版本（输出到 ./dist）
npm run build

# 4. 本地预览生产构建
npm run preview
```

> 首次安装若遇网络问题，可设置镜像：`npm config set registry https://registry.npmmirror.com`

## 部署到 Cloudflare Pages

### 方式一：GitHub 自动部署（推荐）

1. 将本仓库推送到 GitHub（`main` 分支）。
2. 登录 Cloudflare 控制台 → **Workers & Pages** → **Create application** → **Pages** → **Connect to Git**。
3. 选择 `personal-site` 仓库，构建设置如下：

   | 配置项 | 值 |
   | --- | --- |
   | Framework preset | `Astro` |
   | Build command | `npm run build` |
   | Build output directory | `dist` |
   | Node version | `20` |

4. 保存后每次 `git push` 到 `main` 都会自动触发部署。
5. 默认域名为 `https://personal-site.pages.dev`，可在 **Custom domains** 绑定自有域名。

### 方式二：直接上传（适合临时调试）

```bash
npm run build
# 然后在 Cloudflare Pages 控制台选择 "Upload assets" 上传 ./dist 目录
```

`wrangler.toml` 仅用于本地 `wrangler pages dev` 调试或 CLI 部署，Cloudflare 控制台 Git 集成会自动识别 Astro 框架。

## 目录结构

```
personal-site/
├── public/                  # 静态资源（favicon、图片等，不经过构建）
├── src/
│   ├── components/          # Astro 组件（Header、Footer、Card 等）
│   ├── content/             # 内容集合（博客、项目等）
│   │   ├── config.ts        #   - 内容集合 schema
│   │   ├── blog/            #   - 博客 Markdown/MDX 源文件
│   │   └── projects/        #   - 项目 Markdown/MDX 源文件
│   ├── layouts/             # 页面布局
│   ├── pages/               # 路由（index.astro、about.astro、blog/[slug].astro 等）
│   └── styles/              # 全局样式
├── astro.config.mjs         # Astro 配置（集成、site URL）
├── tsconfig.json            # TypeScript 严格模式
├── wrangler.toml            # Cloudflare Pages 配置
├── package.json
└── README.md
```

## 怎么写新博客

1. 在 `src/content/blog/` 下新建 `my-post.md` 或 `my-post.mdx`。
2. 添加 frontmatter（必须字段已在 `src/content/config.ts` 的 schema 中定义）：

   ```md
   ---
   title: "我的第一篇博客"
   description: "一句话简介，会出现在列表和 SEO meta 中"
   pubDate: 2026-09-01
   tags: ["随笔", "Astro"]
   updatedDate: 2026-09-02  # 可选
   ---

   正文内容，支持 **Markdown** 和 MDX 组件。
   ```

3. 提交后列表页（通常是 `src/pages/blog/index.astro`）会自动出现新文章，详情页路由为 `/blog/my-post/`。

## 怎么加新项目

在 `src/content/projects/` 下新建 `project-name.md`，按对应 schema 填写 frontmatter 即可。`src/pages/projects/` 下的页面会自动列出。

## 常用命令

| 命令 | 说明 |
| --- | --- |
| `npm run dev` | 启动开发服务器（含热更新） |
| `npm run build` | 构建生产版本到 `dist/` |
| `npm run preview` | 本地预览生产构建 |
| `npm run astro -- check` | TypeScript / 内容集合类型检查 |

## 站点信息

- **站点 URL**：`https://mulizi.pages.dev`（在 `astro.config.mjs` 的 `site` 字段维护）
- **sitemap**：构建后自动生成于 `/sitemap-index.xml`
- **RSS**：通常在 `src/pages/rss.xml.ts` 中按需实现
