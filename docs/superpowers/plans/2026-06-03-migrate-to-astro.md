# Astro Blog and Portfolio Migration Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Migrate the old Jekyll blog to a modern, high-performance Astro-based blog and portfolio at custom domain `bsdsolucoes.com` with a premium dark-mode visual system.

**Architecture:** Create a custom Astro application in the workspace root, configuring vanilla CSS styles, a common glassmorphic Layout component, content collections for blog posts, dynamic page routes, and an automated GitHub Pages deploy action.

**Tech Stack:** Astro, CSS, GitHub Actions, Hostinger DNS.

---

### Task 1: Scaffold Astro Project

**Files:**
- Create: `astro.config.mjs`
- Create: `package.json`
- Create: `tsconfig.json`
- Create: `src/env.d.ts`

- [ ] **Step 1: Scaffold Astro in the current directory**

Run: `npm create astro@latest ./ -- --template minimal --install --no-git --yes`
Expected: Astro is successfully scaffolded with all dependencies installed.

- [ ] **Step 2: Verify the scaffolded Astro build**

Run: `npm run build`
Expected: Builds without errors and generates a `dist/` directory.

- [ ] **Step 3: Commit scaffold files**

Run:
```bash
git add package.json package-lock.json astro.config.mjs tsconfig.json src/
git commit -m "chore: scaffold Astro project structure"
```

---

### Task 2: Configure Layout & Visual Identity (CSS Variables)

**Files:**
- Create: `src/styles/global.css`
- Create: `src/layouts/Layout.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create global CSS style sheet**

Create `src/styles/global.css` with the following content:
```css
:root {
  --bg-color: hsl(222, 47%, 11%);
  --card-bg: hsla(222, 47%, 16%, 0.7);
  --text-primary: hsl(210, 40%, 98%);
  --text-secondary: hsl(215, 20%, 65%);
  --accent-indigo: hsl(250, 95%, 70%);
  --accent-teal: hsl(180, 100%, 50%);
  --border-line: hsla(217, 10%, 50%, 0.15);
  --font-header: 'Outfit', sans-serif;
  --font-body: 'Inter', sans-serif;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  background-color: var(--bg-color);
  color: var(--text-primary);
  font-family: var(--font-body);
  line-height: 1.6;
  overflow-x: hidden;
}

h1, h2, h3, h4 {
  font-family: var(--font-header);
  font-weight: 700;
  color: var(--text-primary);
}

a {
  color: var(--accent-indigo);
  text-decoration: none;
  transition: color 0.2s ease;
}

a:hover {
  color: var(--accent-teal);
}

.glass-card {
  background: var(--card-bg);
  backdrop-filter: blur(10px);
  border: 1px solid var(--border-line);
  border-radius: 12px;
  padding: 24px;
}
```

- [ ] **Step 2: Create base layout component**

Create `src/layouts/Layout.astro`:
```astro
---
import '../styles/global.css';

interface Props {
  title: string;
  description?: string;
}

const { title, description = "Antonio's Tech Blog and Portfolio" } = Astro.props;
---

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="generator" content={Astro.generator} />
    <title>{title}</title>
    <meta name="description" content={description} />
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Outfit:wght@600;700;800&display=swap" rel="stylesheet">
  </head>
  <body>
    <header class="navbar">
      <div class="nav-container">
        <a href="/" class="logo">Antonio's Tech</a>
        <nav>
          <a href="/">Home</a>
          <a href="/posts">Blog</a>
          <a href="/about">About</a>
          <a href="/contact">Contact</a>
        </nav>
      </div>
    </header>

    <main class="content-wrapper">
      <slot />
    </main>

    <footer>
      <p>&copy; {new Date().getFullYear()} Antonio Carlos. Powered by Astro.</p>
    </footer>
  </body>
</html>

<style>
  .navbar {
    position: sticky;
    top: 0;
    z-index: 100;
    background: hsla(222, 47%, 11%, 0.85);
    backdrop-filter: blur(8px);
    border-bottom: 1px solid var(--border-line);
  }
  .nav-container {
    max-width: 1000px;
    margin: 0 auto;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 16px 24px;
  }
  .logo {
    font-family: var(--font-header);
    font-weight: 800;
    font-size: 1.25rem;
    color: var(--text-primary);
    background: linear-gradient(45deg, var(--accent-indigo), var(--accent-teal));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  nav a {
    margin-left: 20px;
    color: var(--text-secondary);
    font-weight: 500;
  }
  nav a:hover {
    color: var(--text-primary);
  }
  .content-wrapper {
    max-width: 1000px;
    margin: 0 auto;
    padding: 40px 24px;
    min-height: calc(100vh - 180px);
  }
  footer {
    border-top: 1px solid var(--border-line);
    text-align: center;
    padding: 24px;
    color: var(--text-secondary);
    font-size: 0.875rem;
  }
</style>
```

- [ ] **Step 3: Modify index.astro to load layout**

Overwite `src/pages/index.astro`:
```astro
---
import Layout from '../layouts/Layout.astro';
---

<Layout title="Antonio's Tech | Home">
  <div class="hero">
    <h1>Welcome to My Space</h1>
    <p>Exploring tech, recommendation systems, and professional growth.</p>
  </div>
</Layout>

<style>
  .hero {
    text-align: center;
    padding: 80px 0;
  }
  h1 {
    font-size: 3rem;
    margin-bottom: 16px;
    background: linear-gradient(to right, var(--accent-indigo), var(--accent-teal));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  p {
    font-size: 1.25rem;
    color: var(--text-secondary);
  }
</style>
```

- [ ] **Step 4: Run build to verify styling integration**

Run: `npm run build`
Expected: PASS

- [ ] **Step 5: Commit layout styling**

Run:
```bash
git add src/styles/global.css src/layouts/Layout.astro src/pages/index.astro
git commit -m "feat: add global styles and core Layout component"
```

---

### Task 3: Content Collections & Blog Post Migration

**Files:**
- Create: `src/content/config.ts`
- Create: `src/content/blog/modelo-recomendacao-python.md`
- Create: `src/pages/posts/index.astro`
- Create: `src/pages/posts/[...slug].astro`

- [ ] **Step 1: Define content config**

Create `src/content/config.ts`:
```typescript
import { defineCollection, z } from 'astro:content';

const blog = defineCollection({
  type: 'content',
  schema: z.object({
    title: z.string(),
    subtitle: z.string().optional(),
    date: z.coerce.date(),
    background: z.string().optional(),
  }),
});

export const collections = { blog };
```

- [ ] **Step 2: Migrate post file**

Create `src/content/blog/modelo-recomendacao-python.md` (copied and adapted from the original `_posts/2022-05-28-modelo-recomendacao-python.md`):
```markdown
---
title: "Recommendation Systems in Python (An Overview)"
subtitle: "Understanding how it works, methodology, and how I implement it on a startup."
date: 2022-05-28
background: "/img/posts/modelo-recomendacao-python/capa.jpg"
---

# Exemplo de uma tabela de recomendação:

Photo by Brett Jordan on Unsplash

Este é um exemplo de uma tabela de recomendação gerada pelo modelo:

![Recommendation](/img/posts/modelo-recomendacao-python/recommendation_table.png)
```

- [ ] **Step 3: Create blog archive page**

Create `src/pages/posts/index.astro`:
```astro
---
import { getCollection } from 'astro:content';
import Layout from '../../layouts/Layout.astro';

const posts = (await getCollection('blog')).sort(
  (a, b) => b.data.date.valueOf() - a.data.date.valueOf()
);
---

<Layout title="Blog Posts | Antonio's Tech">
  <div class="archive-container">
    <h1>All Posts</h1>
    <div class="post-grid">
      {posts.map((post) => (
        <article class="glass-card post-card">
          <a href={`/posts/${post.slug}`}>
            <h2>{post.data.title}</h2>
            {post.data.subtitle && <p class="subtitle">{post.data.subtitle}</p>}
            <time datetime={post.data.date.toISOString()}>
              {post.data.date.toLocaleDateString('en-US', {
                year: 'numeric',
                month: 'long',
                day: 'numeric',
              })}
            </time>
          </a>
        </article>
      ))}
    </div>
  </div>
</Layout>

<style>
  h1 {
    font-size: 2.5rem;
    margin-bottom: 32px;
  }
  .post-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: 24px;
  }
  .post-card {
    transition: transform 0.2s ease, box-shadow 0.2s ease;
  }
  .post-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 30px rgba(0, 0, 0, 0.3);
    border-color: var(--accent-indigo);
  }
  h2 {
    font-size: 1.5rem;
    margin-bottom: 8px;
    color: var(--text-primary);
  }
  .subtitle {
    color: var(--text-secondary);
    margin-bottom: 16px;
  }
  time {
    font-size: 0.875rem;
    color: var(--accent-teal);
    font-weight: 500;
  }
</style>
```

- [ ] **Step 4: Create single blog page rendering**

Create `src/pages/posts/[...slug].astro`:
```astro
---
import { getCollection } from 'astro:content';
import Layout from '../../layouts/Layout.astro';

export async function getStaticPaths() {
  const posts = await getCollection('blog');
  return posts.map((post) => ({
    params: { slug: post.slug },
    props: post,
  }));
}

const post = Astro.props;
const { Content } = await post.render();
---

<Layout title={post.data.title} description={post.data.subtitle}>
  <article class="post-content">
    {post.data.background && (
      <div class="banner-image" style={`background-image: url(${post.data.background})`} />
    )}
    <h1>{post.data.title}</h1>
    {post.data.subtitle && <p class="subtitle">{post.data.subtitle}</p>}
    <time datetime={post.data.date.toISOString()}>
      {post.data.date.toLocaleDateString('en-US', {
        year: 'numeric',
        month: 'long',
        day: 'numeric',
      })}
    </time>
    <hr class="separator" />
    <div class="markdown-body">
      <Content />
    </div>
  </article>
</Layout>

<style>
  .post-content {
    max-width: 700px;
    margin: 0 auto;
  }
  .banner-image {
    width: 100%;
    height: 300px;
    background-size: cover;
    background-position: center;
    border-radius: 12px;
    margin-bottom: 32px;
  }
  h1 {
    font-size: 2.75rem;
    margin-bottom: 12px;
    line-height: 1.2;
  }
  .subtitle {
    font-size: 1.25rem;
    color: var(--text-secondary);
    margin-bottom: 16px;
  }
  time {
    display: block;
    color: var(--accent-teal);
    font-weight: 500;
    margin-bottom: 24px;
  }
  .separator {
    border: 0;
    border-top: 1px solid var(--border-line);
    margin-bottom: 32px;
  }
  .markdown-body :global(p) {
    margin-bottom: 20px;
  }
  .markdown-body :global(img) {
    max-width: 100%;
    border-radius: 8px;
    margin: 24px 0;
  }
</style>
```

- [ ] **Step 5: Verify types and collection schema**

Run: `npx astro check`
Expected: Succeeds with zero type errors.

- [ ] **Step 6: Commit collection migration**

Run:
```bash
git add src/content/config.ts src/content/blog/modelo-recomendacao-python.md src/pages/posts/
git commit -m "feat: migrate blog posts schema and page views"
```

---

### Task 4: Bio, Projects & Contact Pages

**Files:**
- Create: `src/pages/about.astro`
- Create: `src/pages/contact.astro`

- [ ] **Step 1: Implement the About page**

Create `src/pages/about.astro`:
```astro
---
import Layout from '../layouts/Layout.astro';
---

<Layout title="About Me | Antonio Carlos">
  <div class="about-container">
    <div class="glass-card profile-card">
      <h1>About Me</h1>
      <p class="bio">
        Hi! I'm Antonio Carlos, a software developer and tech enthusiast. I enjoy working on data-driven projects, machine learning, and building interactive, high-performance web solutions. Currently exploring recommendation systems and scalable architectures.
      </p>
      
      <h2>Skills & Expertise</h2>
      <div class="skills-grid">
        <span class="skill-tag">Python</span>
        <span class="skill-tag">JavaScript / TypeScript</span>
        <span class="skill-tag">Astro</span>
        <span class="skill-tag">SQL</span>
        <span class="skill-tag">Recommendation Systems</span>
        <span class="skill-tag">Software Architecture</span>
      </div>
    </div>
  </div>
</Layout>

<style>
  .about-container {
    max-width: 750px;
    margin: 0 auto;
  }
  h1 {
    font-size: 2.5rem;
    margin-bottom: 20px;
    background: linear-gradient(45deg, var(--accent-indigo), var(--accent-teal));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  .bio {
    font-size: 1.125rem;
    color: var(--text-secondary);
    margin-bottom: 32px;
  }
  h2 {
    font-size: 1.5rem;
    margin-bottom: 16px;
  }
  .skills-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
  }
  .skill-tag {
    background: hsla(250, 95%, 70%, 0.15);
    color: var(--accent-indigo);
    border: 1px solid hsla(250, 95%, 70%, 0.3);
    padding: 8px 16px;
    border-radius: 20px;
    font-weight: 500;
    font-size: 0.9rem;
  }
</style>
```

- [ ] **Step 2: Implement the Contact page**

Create `src/pages/contact.astro`:
```astro
---
import Layout from '../layouts/Layout.astro';
---

<Layout title="Contact Me | Antonio Carlos">
  <div class="contact-container">
    <div class="glass-card">
      <h1>Let's Connect</h1>
      <p class="intro">
        Feel free to reach out for collaboration, inquiries, or just to talk about tech!
      </p>

      <form class="contact-form" onsubmit="event.preventDefault(); alert('Message sent!');">
        <div class="form-group">
          <label for="name">Name</label>
          <input type="text" id="name" required placeholder="Your Name" />
        </div>
        <div class="form-group">
          <label for="email">Email</label>
          <input type="email" id="email" required placeholder="your.email@example.com" />
        </div>
        <div class="form-group">
          <label for="message">Message</label>
          <textarea id="message" rows="5" required placeholder="Write your message here..."></textarea>
        </div>
        <button type="submit" class="submit-btn">Send Message</button>
      </form>

      <div class="social-links">
        <h2>Or find me on:</h2>
        <div class="links-row">
          <a href="https://github.com/cobap" target="_blank" rel="noopener noreferrer">GitHub</a>
          <a href="https://linkedin.com/in/antonio-coelho" target="_blank" rel="noopener noreferrer">LinkedIn</a>
          <a href="mailto:baptistaacbc@gmail.com">Email</a>
        </div>
      </div>
    </div>
  </div>
</Layout>

<style>
  .contact-container {
    max-width: 600px;
    margin: 0 auto;
  }
  h1 {
    font-size: 2.5rem;
    margin-bottom: 12px;
  }
  .intro {
    color: var(--text-secondary);
    margin-bottom: 32px;
  }
  .contact-form {
    display: flex;
    flex-direction: column;
    gap: 20px;
    margin-bottom: 40px;
  }
  .form-group {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
  label {
    font-weight: 500;
    font-size: 0.9rem;
  }
  input, textarea {
    background: hsla(222, 47%, 11%, 0.6);
    border: 1px solid var(--border-line);
    border-radius: 6px;
    padding: 12px;
    color: var(--text-primary);
    font-family: inherit;
    outline: none;
    transition: border-color 0.2s;
  }
  input:focus, textarea:focus {
    border-color: var(--accent-teal);
  }
  .submit-btn {
    background: linear-gradient(45deg, var(--accent-indigo), var(--accent-teal));
    color: var(--bg-color);
    border: none;
    padding: 14px;
    border-radius: 6px;
    font-weight: 700;
    cursor: pointer;
    transition: opacity 0.2s;
  }
  .submit-btn:hover {
    opacity: 0.9;
  }
  .social-links h2 {
    font-size: 1.25rem;
    margin-bottom: 16px;
  }
  .links-row {
    display: flex;
    gap: 24px;
  }
  .links-row a {
    font-weight: 600;
    font-size: 1.1rem;
  }
</style>
```

- [ ] **Step 3: Run build to check compiles**

Run: `npm run build`
Expected: PASS

- [ ] **Step 4: Commit views**

Run:
```bash
git add src/pages/about.astro src/pages/contact.astro
git commit -m "feat: add about and contact page views"
```

---

### Task 5: Dynamic Homepage & Portfolio Showcase

**Files:**
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Write dynamic showcase homepage**

Overwrite `src/pages/index.astro`:
```astro
---
import { getCollection } from 'astro:content';
import Layout from '../layouts/Layout.astro';

const posts = (await getCollection('blog'))
  .sort((a, b) => b.data.date.valueOf() - a.data.date.valueOf())
  .slice(0, 3);

const projects = [
  {
    title: "Recommendation System",
    description: "Built a collaborative filtering based recommendation system in Python for dynamic user personalization.",
    tech: ["Python", "Pandas", "Scikit-learn"],
    link: "/posts/modelo-recomendacao-python"
  },
  {
    title: "Astro Blog Portfolio",
    description: "Developed a custom Astro-based website featuring a responsive layout and glassmorphism styling.",
    tech: ["Astro", "TypeScript", "CSS Variables"],
    link: "https://github.com/cobap/cobap.github.io"
  }
];
---

<Layout title="Antonio's Tech | Home">
  <section class="hero">
    <h1>Antonio Carlos</h1>
    <p class="subtitle">Exploring tech, software development, and growth.</p>
  </section>

  <section class="grid-section">
    <div class="projects-area">
      <h2>Recent Projects</h2>
      <div class="projects-grid">
        {projects.map((project) => (
          <div class="glass-card project-card">
            <h3>{project.title}</h3>
            <p>{project.description}</p>
            <div class="tech-row">
              {project.tech.map((t) => <span class="badge">{t}</span>)}
            </div>
            <a href={project.link} class="project-link">Learn More &rarr;</a>
          </div>
        ))}
      </div>
    </div>

    <div class="posts-area">
      <h2>Latest From Blog</h2>
      <div class="posts-list">
        {posts.map((post) => (
          <article class="glass-card post-item">
            <a href={`/posts/${post.slug}`}>
              <h3>{post.data.title}</h3>
              <time datetime={post.data.date.toISOString()}>
                {post.data.date.toLocaleDateString('en-US', {
                  month: 'short',
                  day: 'numeric',
                  year: 'numeric',
                })}
              </time>
            </a>
          </article>
        ))}
        <a href="/posts" class="view-all">View All Posts &rarr;</a>
      </div>
    </div>
  </section>
</Layout>

<style>
  .hero {
    text-align: center;
    padding: 60px 0;
  }
  .hero h1 {
    font-size: 3.5rem;
    background: linear-gradient(45deg, var(--accent-indigo), var(--accent-teal));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    margin-bottom: 8px;
  }
  .subtitle {
    font-size: 1.25rem;
    color: var(--text-secondary);
  }
  .grid-section {
    display: grid;
    grid-template-columns: 3fr 2fr;
    gap: 40px;
    margin-top: 40px;
  }
  @media (max-width: 768px) {
    .grid-section {
      grid-template-columns: 1fr;
    }
  }
  h2 {
    font-size: 1.75rem;
    margin-bottom: 24px;
    border-left: 4px solid var(--accent-teal);
    padding-left: 12px;
  }
  .projects-grid {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }
  .project-card {
    display: flex;
    flex-direction: column;
    gap: 12px;
    transition: transform 0.2s, border-color 0.2s;
  }
  .project-card:hover {
    transform: translateY(-3px);
    border-color: var(--accent-indigo);
  }
  .project-card h3 {
    font-size: 1.25rem;
  }
  .project-card p {
    color: var(--text-secondary);
    font-size: 0.95rem;
  }
  .tech-row {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-top: 4px;
  }
  .badge {
    background: hsla(180, 100%, 50%, 0.1);
    color: var(--accent-teal);
    border: 1px solid hsla(180, 100%, 50%, 0.25);
    padding: 4px 10px;
    border-radius: 4px;
    font-size: 0.8rem;
    font-weight: 600;
  }
  .project-link {
    margin-top: 8px;
    font-size: 0.9rem;
    font-weight: 600;
  }
  .posts-list {
    display: flex;
    flex-direction: column;
    gap: 16px;
  }
  .post-item {
    transition: border-color 0.2s;
  }
  .post-item:hover {
    border-color: var(--accent-teal);
  }
  .post-item h3 {
    font-size: 1.1rem;
    margin-bottom: 8px;
  }
  .post-item time {
    font-size: 0.8rem;
    color: var(--text-secondary);
  }
  .view-all {
    display: inline-block;
    margin-top: 16px;
    font-weight: 600;
    align-self: flex-start;
  }
</style>
```

- [ ] **Step 2: Build project check**

Run: `npm run build`
Expected: PASS

- [ ] **Step 3: Commit homepage**

Run:
```bash
git add src/pages/index.astro
git commit -m "feat: enhance homepage with recent projects and latest posts"
```

---

### Task 6: GitHub Actions CI/CD and Custom Domain Configuration

**Files:**
- Create: `.github/workflows/deploy.yml`
- Create: `public/CNAME`
- Modify: `astro.config.mjs`

- [ ] **Step 1: Set CNAME domain routing**

Create `public/CNAME` containing:
```text
bsdsolucoes.com
```

- [ ] **Step 2: Configure custom deploy workflow**

Create `.github/workflows/deploy.yml`:
```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Install Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      - name: Install Dependencies
        run: npm ci

      - name: Build Site
        run: npm run build

      - name: Deploy to GitHub Pages
        uses: JamesIves/github-pages-deploy-action@v4
        with:
          folder: dist
          branch: gh-pages
```

- [ ] **Step 3: Set site URL in astro.config.mjs**

Modify `astro.config.mjs`:
```javascript
import { defineConfig } from 'astro:config';

// https://astro.build/config
export default defineConfig({
  site: 'https://bsdsolucoes.com',
});
```

- [ ] **Step 4: Check compilation**

Run: `npm run build`
Expected: PASS, verify `dist/CNAME` contains `bsdsolucoes.com`.

- [ ] **Step 5: Clean up old Jekyll files**

Run: `rm -rf Gemfile Gemfile.lock _config.yml _drafts _includes _layouts _posts _sass about.html contact.html posts bin index.html package.json-old package-lock.json-old jekyll-theme-clean-blog.gemspec .jekyll-cache`
Expected: Clean directory with only Astro files.

- [ ] **Step 6: Commit final configurations**

Run:
```bash
git add .github/workflows/deploy.yml public/CNAME astro.config.mjs
git rm -f Gemfile Gemfile.lock _config.yml about.html contact.html index.html jekyll-theme-clean-blog.gemspec
git commit -m "feat: configure github actions deploy workflow and clean up jekyll files"
```
