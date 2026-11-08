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

## Deploy (Cloudflare Workers)

Connected in Cloudflare → Workers & Pages → Create → Import a repository. `wrangler.jsonc` serves the built `public/` folder as static assets.

- Build command: `curl -sL https://github.com/gohugoio/hugo/releases/download/v0.140.2/hugo_extended_0.140.2_linux-amd64.tar.gz | tar xz hugo && ./hugo --gc --minify`
- Deploy command: `npx wrangler deploy`

Then add `saisyam.dev` under the Worker's **Settings → Domains & Routes**.
