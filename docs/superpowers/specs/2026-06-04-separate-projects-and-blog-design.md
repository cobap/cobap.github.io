# Design Specification: Separating Projects and Blog/Posts

This specification outlines the conceptual and visual separation of **Projects** (products with background images, logos, screenshots, and technical specs) and **Blog Posts** (pure knowledge-sharing, text-only articles) at `bsdsolucoes.com`.

## 1. Overview & Goals

- **Concept Separation**: 
  - **Projects** represent products or software artifacts. They require a premium visual presentation, logos, banner images, technology stacks, and links.
  - **Blog** represents knowledge sharing, authority, and ideas. It requires a distraction-free, typography-focused reading layout without heavy graphics.
- **Route Adjustments**:
  - Projects: Listing at `/projects`, individual showcase pages at `/projects/[...id]`.
  - Blog: Listing at `/posts`, individual article pages at `/posts/[...id]`.
- **Top Navigation Nav Link**: Add "Projects" to the header navigation.

---

## 2. Content Collections Schema (`src/content.config.ts`)

We will define two separate collections with strict schema validation.

### 2.1 `projects` Collection
- **Location**: `src/content/projects/`
- **Fields**:
  - `title`: `z.string()` — The name of the project/product.
  - `description`: `z.string()` — A short description or tagline.
  - `date`: `z.coerce.date()` — The release or launch date.
  - `background`: `z.string()` — Path to the full-bleed hero banner image.
  - `logo`: `z.string().optional()` — Path to the floating square/circular project logo.
  - `tech`: `z.array(z.string())` — Array of tools and technologies used.
  - `link`: `z.string().optional()` — External site, demo, or GitHub repository URL.
  - `lang`: `z.string().optional()` — Page language code.

### 2.2 `blog` Collection (Existing)
- **Location**: `src/content/blog/`
- **Fields**:
  - `title`: `z.string()` — Title of the article.
  - `subtitle`: `z.string().optional()` — Subtitle or summary.
  - `date`: `z.coerce.date()` — Date published.
  - `lang`: `z.string().optional()` — Article language code.

---

## 3. UI/UX Layout Design

### 3.1 Single Project Template (`src/pages/projects/[...id].astro`)
- **Full-Width Hero Section**:
  - Full-width background image with a dark, semi-transparent overlay to ensure contrast.
  - Centered logo/icon floating badge.
  - Title and subtitle overlaid on the hero.
- **Two-Column Layout** (Desktop):
  - **Left/Center Column (Main, 65%)**: Markdown body content (descriptions, screenshots, architectural summaries, and tables).
  - **Right Column (Sidebar, 35%)**: A sticky glassmorphism panel displaying:
    - **Tech Stack**: Small glowing pills with tech names.
    - **Date**: The launch date.
    - **Project Link**: High-contrast call-to-action button to check the product/repo.
- **Mobile Responsive**: Stacks to a single column, with the sidebar contents shifting below the hero or main content.

### 3.2 Single Blog Post Template (`src/pages/posts/[...id].astro`)
- **Typography-First Layout**:
  - No background image, no banners, no sidebars.
  - Centered reading column restricted to a max-width of `680px`.
  - Font size, line-height (`1.75`), and typography optimized for reading.
- **Header Metadata**:
  - Clean centered title.
  - Muted subtitle.
  - Date published + calculated reading time (e.g. "5 min read") using a dynamic word-count calculation.

### 3.3 Projects Listing Page (`src/pages/projects/index.astro`)
- Grid of project cards.
- Each card has a background image, absolute-positioned project logo, title, and tech tags.

### 3.4 Blog Posts Listing Page (`src/pages/posts/index.astro`)
- A list of articles in a minimalist text feed format with summary details.

---

## 4. Homepage (`src/pages/index.astro`) & Header Navigation

- **Navbar**: Add `<a href="/projects">Projects</a>` to the global layout.
- **Home Layout**:
  - **Recent Projects**: Displays the latest 2 projects side-by-side using the project card layouts.
  - **Latest Articles**: A minimalist, high-readability text feed of the latest 3 blog articles.
