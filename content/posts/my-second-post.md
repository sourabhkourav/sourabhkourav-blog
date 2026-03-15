---
title: "Hugo Blog - Quick Command Reference"
date: 2026-03-15
draft: false
description: "All commands I use to manage this blog"
tags: ["hugo", "reference"]
---

## 🚀 One-Time Setup
```bash
# Create new Hugo site
hugo new site sourabhkourav
cd D:\dev\hugo-blog\sourabhkourav

# Initialize Git
git init

# Add PaperMod theme
git submodule add https://github.com/adityatelange/hugo-PaperMod themes/PaperMod

# Connect to GitHub
git remote add origin https://github.com/sourabhkourav/sourabhkourav-blog.git
git branch -M main
git push -u origin main
```

## 📝 Creating a New Post
```bash
# Step 1 — Create post file
hugo new posts/my-post-title.md

# Step 2 — Open and edit the file
# content/posts/my-post-title.md
```

Post front matter template:
```markdown
---
title: "Your Post Title"
date: 2026-03-15
draft: false
description: "Short description for SEO"
tags: ["tech", "coding"]
categories: ["Tech"]
cover:
    image: "/images/cover.jpg"
    alt: "cover image"
---

Your content here...
```

## 📄 Creating a New Page (with Menu)
```bash
# Step 1 — Create page file
hugo new contact.md
hugo new uses.md
hugo new now.md
```

Step 2 — Open `hugo.toml` and add to menu:
```toml
[[menu.main]]
  name = "Contact"
  url = "/contact/"
  weight = 3

[[menu.main]]
  name = "Uses"
  url = "/uses/"
  weight = 4
```

Step 3 — Push to deploy:
```bash
git add .
git commit -m "add contact page"
git push
```

## ✏️ Draft → Preview → Publish Workflow
```bash
# Step 1 — Create post (draft: true by default)
hugo new posts/my-post-title.md

# Step 2 — Preview draft locally
hugo server -D
# Open http://localhost:1313 to see draft

# Step 3 — When happy, open the .md file
# Change draft: true → draft: false

# Step 4 — Preview again without drafts
# (exactly like live site)
hugo server

# Step 5 — Publish
git add .
git commit -m "publish: my post title"
git push
```

## 📅 Schedule a Future Post
```bash
# Step 1 — Create post with a future date
hugo new posts/my-future-post.md
```

Set a future date in front matter:
```markdown
---
title: "My Future Post"
date: 2026-04-01
draft: false
---
```
```bash
# Step 2 — Preview future posts locally
hugo server -D -F

# Note: Cloudflare will only show it
# after the date passes automatically
```

## 🚢 Daily Publish Workflow
```bash
# Stage all changes
git add .

# Commit with message
git commit -m "new post: your post title"

# Push to GitHub (auto-deploys to Cloudflare in ~30 sec)
git push
```

## 🖼️ Images
```bash
# Step 1 — Put image in static/images/ folder
# Example: static/images/my-photo.jpg

# Step 2 — Use in post content
![Alt text](/images/my-photo.jpg)

# Step 3 — Use as cover image (in front matter)
cover:
    image: "/images/my-photo.jpg"
    alt: "description of image"
    caption: "optional caption"
    relative: false
```

## ⚙️ hugo.toml Common Changes
```toml
# Change site title
title = "Sourabh Kourav"

# Change description
description = "Tech, Life & Finance — by Sourabh Kourav"

# Add new menu item
[[menu.main]]
  name = "Contact"
  url = "/contact/"
  weight = 3

# Toggle features
ShowReadingTime = true
ShowShareButtons = true
ShowCodeCopyButtons = true
ShowBreadCrumbs = true

# Add Google Analytics
[services]
  [services.googleAnalytics]
    id = "G-XXXXXXXXXX"

# Update base URL when domain is bought
baseURL = "https://sourabhkourav.dev/"
```

## 🏷️ Tags & Categories
```markdown
# Add to post front matter
tags: ["python", "tutorial"]
categories: ["Tech"]

# Auto-generates pages at:
# sourabhkourav.pages.dev/tags/python
# sourabhkourav.pages.dev/categories/tech
```

## 🔄 Update Theme
```bash
# Update PaperMod to latest version
git submodule update --remote themes/PaperMod
git add .
git commit -m "update PaperMod theme"
git push
```

## 🗑️ Delete a Post or Page
```bash
# Simply delete the .md file
del content\posts\my-post.md

# Then push
git add .
git commit -m "remove post: title"
git push
```

## 📋 Git Reference
```bash
# Check status of changed files
git status

# See commit history
git log --oneline

# Undo last commit (keeps changes)
git reset --soft HEAD~1

# Pull latest from GitHub
git pull
```

## 📁 Folder Structure
```
sourabhkourav/
├── content/
│   ├── posts/          ← blog posts
│   └── about.md        ← standalone pages
├── static/
│   └── images/         ← all images go here
├── themes/
│   └── PaperMod/       ← never edit this!
└── hugo.toml           ← site config
```

## 🔗 Important Links

| What | URL |
|---|---|
| Live Blog | https://sourabhkourav.pages.dev |
| GitHub Repo | https://github.com/sourabhkourav/sourabhkourav-blog |
| Local Preview | http://localhost:1313 |
| Cloudflare Dashboard | https://dash.cloudflare.com |
| Hugo Themes | https://themes.gohugo.io |
| PaperMod Docs | https://github.com/adityatelange/hugo-PaperMod/wiki |

## ⚠️ Important Rules

- Never edit anything inside `themes/` folder
- Always set `draft: false` before publishing
- Images always go in `static/images/` folder
- Run `hugo server -D` to preview before pushing
- When you buy `sourabhkourav.dev` update `baseURL` in `hugo.toml`