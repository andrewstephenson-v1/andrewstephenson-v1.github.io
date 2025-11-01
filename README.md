# Andrew Stephenson's Blog

Personal blog built with Jekyll and hosted on GitHub Pages.

**Live Site**: [https://andrewstephenson-v1.github.io](https://andrewstephenson-v1.github.io)

---

## Project Structure

```
andrewstephenson-v1.github.io/
├── _config.yml              # Jekyll configuration
├── _posts/                  # Blog posts (YYYY-MM-DD-title.md format)
├── _site/                   # Generated site (gitignored)
├── assets/                  # Custom styles and assets
│   └── main.scss           # Custom CSS/SCSS
├── .github/
│   └── workflows/
│       └── jekyll.yml      # GitHub Actions deployment workflow
├── Gemfile                  # Ruby dependencies
├── index.md                 # Homepage content
└── README.md                # This file
```

---

## Local Development

### Prerequisites

- **Ruby 2.5+** (preferably 3.0+)
- **Bundler** gem installed

### Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/andrewstephenson-v1/andrewstephenson-v1.github.io.git
   cd andrewstephenson-v1.github.io
   ```

2. **Install dependencies**:
   ```bash
   bundle install
   ```

### Running Locally

**Start the Jekyll development server**:
```bash
bundle exec jekyll serve
```

The site will be available at: `http://localhost:4000`

**Options**:
- `--livereload` - Auto-refresh browser on changes
- `--drafts` - Include draft posts (in `_drafts/` folder)
- `--incremental` - Faster builds (only rebuilds changed files)

**Full command with options**:
```bash
bundle exec jekyll serve --livereload --drafts
```

**Stop the server**: Press `Ctrl+C`

---

## Creating New Posts

### Post Filename Format

All posts must be placed in the `_posts/` directory with this naming convention:

```
YYYY-MM-DD-title-with-dashes.md
```

**Examples**:
- `2024-11-05-my-first-post.md`
- `2024-12-20-getting-started-with-jekyll.md`

### Post Front Matter Template

Every post must start with YAML front matter:

```yaml
---
layout: post
title: "Your Post Title"
date: 2024-11-05 09:00:00 +0000
description: "Brief description for SEO and previews"
tags: [tag1, tag2, tag3]
---

Your post content goes here in Markdown format.

## Section Heading

Content...
```

**Required fields**:
- `layout: post` - Uses the post template
- `title` - Post title (use quotes if it contains special characters)
- `date` - Full date and time (posts with future dates won't display until that date)

**Optional fields**:
- `description` - SEO meta description
- `tags` - Array of tags for categorization

### Quick Post Creation

Create a new post from template:

```bash
# Create new post file
cat > _posts/$(date +%Y-%m-%d)-my-new-post.md << 'EOF'
---
layout: post
title: "My New Post"
date: $(date +%Y-%m-%d) 12:00:00 +0000
description: "Post description here"
tags: [update]
---

Your content here...
EOF
```

---

## Testing Before Publishing

### 1. Local Testing Checklist

Before pushing to GitHub, verify locally:

```bash
# Start local server
bundle exec jekyll serve --livereload
```

**Check**:
- [ ] All posts display correctly at `http://localhost:4000`
- [ ] Links work (internal and external)
- [ ] Images load properly
- [ ] Custom styles apply correctly
- [ ] No Jekyll build errors in terminal
- [ ] Mobile responsive (resize browser window)

### 2. Check Jekyll Build

Test the build process:

```bash
# Clean previous builds
bundle exec jekyll clean

# Build site (without serving)
bundle exec jekyll build

# Check _site/ directory was created
ls -la _site/
```

**Look for**:
- No errors during build
- `_site/` directory contains generated HTML
- All posts converted to HTML in `_site/YYYY/MM/DD/`

### 3. Validate Markdown

Check for common issues:
- Front matter formatting (must have `---` delimiters)
- Valid YAML syntax in front matter
- Date format: `YYYY-MM-DD HH:MM:SS +ZZZZ`
- Filenames match date in front matter

### 4. Link Checking

Test internal links:

```bash
# Install html-proofer (one-time)
gem install html-proofer

# Check links (after building)
htmlproofer ./_site --disable-external
```

---

## Publishing

### Automatic Deployment

The site automatically deploys when you push to the `main` branch:

```bash
# Stage changes
git add .

# Commit with descriptive message
git commit -m "Add new post about topic X"

# Push to GitHub
git push origin main
```

**Deployment process**:
1. Push triggers GitHub Actions workflow (`.github/workflows/jekyll.yml`)
2. Jekyll builds the site
3. Deploys to GitHub Pages
4. Live in ~1-2 minutes

### Check Deployment Status

1. **Go to Actions tab**: [https://github.com/andrewstephenson-v1/andrewstephenson-v1.github.io/actions](https://github.com/andrewstephenson-v1/andrewstephenson-v1.github.io/actions)
2. **Look for latest workflow run**:
   - 🟢 Green checkmark = Deployed successfully
   - 🔴 Red X = Build failed (click to see error logs)
   - 🟡 Yellow circle = Currently building

3. **View deployment**:
   - Check [https://andrewstephenson-v1.github.io](https://andrewstephenson-v1.github.io)
   - Hard refresh browser: `Cmd+Shift+R` (Mac) or `Ctrl+Shift+R` (Windows/Linux)

### Rollback if Needed

If a deployment breaks the site:

```bash
# Revert last commit
git revert HEAD

# Push to deploy previous version
git push origin main
```

---

## Configuration

### Site Settings

Edit `_config.yml` to change:

- **Site title**: `title: Your Name`
- **Description**: `description: Your blog description`
- **URL**: `url: https://yourusername.github.io`
- **Author info**: `author.name` and `author.email`
- **Social links**: Under `minima.social_links`
- **Theme skin**: `minima.skin` (options: classic, dark, auto, solarized)

**Important**: After editing `_config.yml`, restart Jekyll server for changes to take effect.

### Custom Styling

Edit `assets/main.scss` to customize:
- Colors (`$accent`, `$text-color`, etc.)
- Fonts
- Layout widths
- Component styling

---

## Troubleshooting

### Build Fails Locally

```bash
# Update dependencies
bundle update

# Clean and rebuild
bundle exec jekyll clean
bundle exec jekyll build --verbose
```

### Build Fails on GitHub

1. Check Actions tab for error logs
2. Common issues:
   - Invalid front matter YAML
   - Future-dated posts (won't show until that date)
   - Missing required fields in front matter
   - File encoding issues (use UTF-8)

### Posts Not Showing

- **Check date**: Posts dated in the future won't appear
- **Check filename**: Must match `YYYY-MM-DD-title.md` format
- **Check front matter**: Must have `layout: post`
- **Clear browser cache**: Hard refresh (`Cmd+Shift+R`)

### Styling Not Applying

- Restart Jekyll server after editing `_config.yml`
- Clear browser cache
- Check for CSS syntax errors in `assets/main.scss`
- Verify SCSS front matter (requires `---` delimiters at top)

---

## Resources

- **Jekyll Documentation**: [https://jekyllrb.com/docs/](https://jekyllrb.com/docs/)
- **Minima Theme**: [https://github.com/jekyll/minima](https://github.com/jekyll/minima)
- **GitHub Pages**: [https://docs.github.com/en/pages](https://docs.github.com/en/pages)
- **Markdown Guide**: [https://www.markdownguide.org/](https://www.markdownguide.org/)

---

## License

This is a personal blog. Content is © Andrew Stephenson. Code is MIT licensed.