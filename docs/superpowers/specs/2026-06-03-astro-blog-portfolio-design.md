# Design Specification: Astro Blog and Portfolio Migration

This document outlines the architecture, layout, styling system, and deployment strategy for migrating the old Jekyll blog to a modern, high-performance Astro-based blog and portfolio at custom domain `bsdsolucoes.com`.

## 1. Overview & Goals

- **Goal**: Rebuild the old blog (`cobap.github.io`) using **Astro** for optimal performance, modern developer portfolio capabilities, and a premium look.
- **Content Conservation**: Migrate the existing blog post (Recommendation Systems in Python) and structure the about/contact sections to showcase actual user details.
- **Aesthetic standard**: Modern, sleek dark mode by default, custom CSS (vanilla), glassmorphism cards, gradients, and micro-interactions.
- **Custom Domain**: Deploy to **GitHub Pages** and route through the user's domain `bsdsolucoes.com` registered at Hostinger.

---

## 2. Visual Identity & Styling System

The application will use a custom-tailored CSS variables system for maximum flexibility, responsiveness, and aesthetic appeal.

### Color Palette (HSL)
- **Background**: HSL Slate-Dark (`hsl(222, 47%, 11%)`)
- **Card Background**: HSL Slate-Card (`hsla(222, 47%, 16%, 0.7)`) with backdrop blur
- **Primary Text**: White/Off-white (`hsl(210, 40%, 98%)`)
- **Secondary Text**: Muted blue-gray (`hsl(215, 20%, 65%)`)
- **Accent 1 (Indigo)**: `hsl(250, 95%, 70%)`
- **Accent 2 (Teal)**: `hsl(180, 100%, 50%)`
- **Border/Line**: `hsla(217, 10%, 50%, 0.15)`

### Typography
- Header font: **Outfit** (modern, geometric sans-serif)
- Body font: **Inter** (highly readable sans-serif)
- Code blocks: Fira Code or JetBrains Mono

### Interactivity & Effects
- Backdrop filters for glassmorphism panels.
- Subtle glow animations around main layout components.
- Smooth transitions (`300ms ease`) on interactive card hovers and link states.

---

## 3. Site Structure & Pages

### 3.1 Pages (`src/pages/`)
- `/` (Home): Main landing page. Features a hero banner, a brief intro bio, a showcase grid of top projects, and the 3 latest blog posts.
- `/posts/`: A paginated index of all blog posts sorted by date.
- `/posts/[...slug]`: The single article viewer. High-quality reading styles, structured headers, and clean syntax highlighting.
- `/about`: Details about the owner, professional background, expertise, and a resume/skills section.
- `/contact`: A clean contact form interface and direct links to active social media profiles (GitHub, LinkedIn, Email).

### 3.2 Component Breakdown (`src/components/`)
- `Layout.astro`: Common wrapper with SEO metadata, global CSS imports, navbar, footer, and basic site shell.
- `Navbar.astro`: Glassmorphic navigation header with logo and links.
- `Footer.astro`: Minimal footer with copy and links.
- `BlogPostCard.astro`: Summary card for posts with hover scale and gradient accent line.
- `ProjectCard.astro`: Clean card layout for showcasing portfolio items.

### 3.3 Content Collections (`src/content/`)
- Astro Content Collections will be configured for `blog`.
- **Migration Mapping**:
  - `_posts/2022-05-28-modelo-recomendacao-python.md` -> `src/content/blog/modelo-recomendacao-python.md`.
  - The frontmatter fields will map directly: `title`, `subtitle`, `date`, `background`, and `layout` (layout field can be ignored in favor of Astro layout mapping).

---

## 4. Technical Stack

- **Framework**: Astro (v4+)
- **CSS**: Vanilla CSS with custom layout styling
- **Icons**: SVG assets for inline icons (GitHub, LinkedIn, Email)
- **Deployment**: GitHub Pages workflow (`.github/workflows/deploy.yml`)
- **Custom Domain**: `bsdsolucoes.com` via a `public/CNAME` file and Hostinger DNS configuration.

---

## 5. Deployment & DNS Setup

### 5.1 Hostinger DNS configuration
Ensure the following records are set in the Hostinger control panel for domain `bsdsolucoes.com`:

| Type | Host | Points to | TTL |
| :--- | :--- | :--- | :--- |
| A | `@` | `185.199.108.153` | 14400 (or default) |
| A | `@` | `185.199.109.153` | 14400 |
| A | `@` | `185.199.110.153` | 14400 |
| A | `@` | `185.199.111.153` | 14400 |
| CNAME | `www` | `cobap.github.io` | 14400 |

### 5.2 GitHub Pages activation
1. Once pushed to GitHub, navigate to repository settings.
2. Select **Pages** from the sidebar.
3. Choose deployment source as the `gh-pages` branch.
4. Set the custom domain to `bsdsolucoes.com`.
5. Check the **Enforce HTTPS** box once DNS records resolve.
