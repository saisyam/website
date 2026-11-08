# saisyam.dev

Personal site of Saisyam Dampuri: AI projects and writing. Built with [Hugo](https://gohugo.io) and a custom theme in `themes/saisyam`.

## Run locally

```bash
hugo server        # http://localhost:1313
```

## Add content

```bash
hugo new posts/my-post-title.md          # new blog post (starts as draft: true)
```

New project: copy one of the files in `content/projects/` and edit the front matter (`status`, `label`, `weight`, `stack`).

## Deploy (Cloudflare Pages)

Connect this repo in Cloudflare → Workers & Pages → Create → Pages → Connect to Git.

- Build command: `hugo --gc --minify`
- Output directory: `public`
- Environment variable: `HUGO_VERSION` = `0.140.2`

Then add `saisyam.dev` under the project's **Custom domains**.
