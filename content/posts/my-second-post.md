---
title: "My Second Post"
date: 2026-03-15
draft: false
description: "Testing auto deploy"
tags: ["hugo-commands"]
---

# Hugo Blog Cheat Sheet
### sourabhkourav.pages.dev

---

## 🚀 One-Time Setup Commands

```bash
# Create new Hugo site
hugo new site sourabhkourav

# Go into project folder
cd D:\dev\hugo-blog\sourabhkourav

# Initialize Git
git init

# Add PaperMod theme
git submodule add https://github.com/adityatelange/hugo-PaperMod themes/PaperMod

# Connect to GitHub
git remote add origin https://github.com/sourabhkourav/sourabhkourav-blog.git
git branch -M main
```

---

## 📝 Creating Content

```bash
# Create a new blog post
hugo new posts/my-post-title.md

# Create a standalone page
hugo new about.md
hugo new contact.md
hugo new uses.md
```

---

## 🖊️ Post Front Matter Template

```markdown
---
title: "Your Post Title"
date: 2026-03-15
draft: false
description: "A short description"
tags: ["tech", "coding"]
categories: ["Tech"]
cover:
    image: "/images/cover.jpg"
    alt: "cover image description"
---

Your post content here...
```

> ⚠️ Always set `draft: false` to make post visible publicly.

---

## 👀 Preview Locally

```bash
# Start local server (includes draft posts)
hugo server -D

# Open in browser
# http://localhost:1313
```

> 💡 Hugo auto-refreshes on every file save. No restart needed!

---

## 🚢 Daily Publish Workflow

```bash
# Stage all changes
git add .

# Commit with a message
git commit -m "new post: your post title"

# Push to GitHub (auto-deploys to Cloudflare)
git push
```

> ⚡ Cloudflare Pages auto-rebuilds in ~30 seconds after every push!

---

## 🖼️ Adding Images

1. Put images in `static/images/` folder
2. Use in post:

```markdown
# Inline image
![Alt text](/images/photo.jpg)

# Cover image (in front matter)
cover:
    image: "/images/photo.jpg"
    alt: "description"
```

---

## 📁 Folder Structure

```
sourabhkourav/
├── content/
│   ├── posts/          ← blog posts go here
│   │   ├── hello-world.md
│   │   └── my-second-post.md
│   ├── about.md        ← standalone pages
│   └── contact.md
├── static/
│   └── images/         ← images go here
├── themes/
│   └── PaperMod/       ← never edit this folder!
└── hugo.toml           ← site settings & menu
```

---

## ⚙️ hugo.toml Quick Reference

```toml
# Change site description
description = "Your new description"

# Add new menu item
[[menu.main]]
  name = "Contact"
  url = "/contact/"
  weight = 3

# Toggle features
ShowReadingTime = true
ShowShareButtons = true
ShowCodeCopyButtons = true

# Add Google Analytics
[services]
  [services.googleAnalytics]
    id = "G-XXXXXXXXXX"
```

---

## 🏷️ Tags & Categories

Just add to your post front matter — pages are auto-generated:

```markdown
tags: ["python", "tutorial"]
categories: ["Tech"]
```

Auto-generates:
- `sourabhkourav.pages.dev/tags/python`
- `sourabhkourav.pages.dev/categories/tech`

---

## 🔗 Your Links

| What | URL |
|---|---|
| Live Blog | https://sourabhkourav.pages.dev |
| GitHub Repo | https://github.com/sourabhkourav/sourabhkourav-blog |
| Local Preview | http://localhost:1313 |
| Cloudflare Dashboard | https://dash.cloudflare.com |

---

## ⚠️ Important Rules

- Never edit anything inside `themes/` folder
- Always set `draft: false` before publishing
- Run `hugo server -D` to preview drafts locally
- When you buy `sourabhkourav.dev`, update `baseURL` in `hugo.toml`