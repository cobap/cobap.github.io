# BSD Soluções Branding and Layout Updates Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Overhaul the branding to "BSD Soluções" with an inline SVG logo, stack the homepage sections vertically with a divider, expand the About page into a LinkedIn-style professional timeline, and integrate Formspree for contact form email handling.

**Architecture:** Modify Layout, Index, About, and Contact pages to update text, styles, markup, and form action URLs.

**Tech Stack:** Astro, CSS, SVG, Formspree.

---

### Task 1: Update Logo & Branding (BSD Soluções)

**Files:**
- Modify: `src/layouts/Layout.astro`

- [ ] **Step 1: Update Layout metadata, footer, and add inline SVG logo**

Overwrite `src/layouts/Layout.astro` to update logo branding and default title details:
```astro
---
import '../styles/global.css';

interface Props {
  title: string;
  description?: string;
  lang?: string;
}

const { title, description = "BSD Soluções - Tecnologia, Consultoria e Desenvolvimento", lang = "en" } = Astro.props;
const { pathname } = Astro.url;
const isActive = (path: string) => {
  const cleanPath = path.replace(/\/$/, '');
  const cleanPathname = pathname.replace(/\/$/, '');
  return cleanPathname === cleanPath || (cleanPath !== '' && cleanPathname.startsWith(cleanPath + '/'));
};
---

<!DOCTYPE html>
<html lang={lang}>
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
        <a href="/" class="logo-group">
          <svg class="logo-svg" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
            <defs>
              <linearGradient id="logo-grad" x1="0%" y1="0%" x2="100%" y2="100%">
                <stop offset="0%" stop-color="var(--accent-indigo)" />
                <stop offset="100%" stop-color="var(--accent-teal)" />
              </linearGradient>
            </defs>
            <polygon points="50,15 80,32 80,68 50,85 20,68 20,32" fill="none" stroke="url(#logo-grad)" stroke-width="8" />
            <polygon points="50,30 68,40 68,60 50,70 32,60 32,40" fill="none" stroke="url(#logo-grad)" stroke-width="4" />
            <line x1="50" y1="15" x2="50" y2="85" stroke="url(#logo-grad)" stroke-width="4" />
            <line x1="20" y1="32" x2="80" y2="68" stroke="url(#logo-grad)" stroke-width="4" />
            <line x1="20" y1="68" x2="80" y2="32" stroke="url(#logo-grad)" stroke-width="4" />
          </svg>
          <span class="logo-text">BSD Soluções</span>
        </a>
        <nav>
          <a href="/" class={isActive('/') ? 'active' : ''} aria-current={isActive('/') ? 'page' : undefined}>Home</a>
          <a href="/posts" class={isActive('/posts') ? 'active' : ''} aria-current={isActive('/posts') ? 'page' : undefined}>Blog</a>
          <a href="/about" class={isActive('/about') ? 'active' : ''} aria-current={isActive('/about') ? 'page' : undefined}>About</a>
          <a href="/contact" class={isActive('/contact') ? 'active' : ''} aria-current={isActive('/contact') ? 'page' : undefined}>Contact</a>
        </nav>
      </div>
    </header>

    <main class="content-wrapper">
      <slot />
    </main>

    <footer>
      <p>&copy; {new Date().getFullYear()} BSD Soluções. All rights reserved.</p>
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
    flex-wrap: wrap;
    gap: 12px;
  }
  @media (max-width: 480px) {
    .nav-container {
      justify-content: center;
      flex-direction: column;
    }
    nav a {
      margin: 0 10px;
    }
  }
  .logo-group {
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .logo-svg {
    width: 32px;
    height: 32px;
  }
  .logo-text {
    font-family: var(--font-header);
    font-weight: 800;
    font-size: 1.35rem;
    background: linear-gradient(45deg, var(--accent-indigo), var(--accent-teal));
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  nav a {
    margin-left: 20px;
    color: var(--text-secondary);
    font-weight: 500;
    padding-bottom: 4px;
  }
  nav a:hover {
    color: var(--text-primary);
  }
  nav a.active {
    color: var(--accent-teal);
    border-bottom: 2px solid var(--accent-teal);
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

- [ ] **Step 2: Build project check**

Run: `npm run build`
Expected: PASS

- [ ] **Step 3: Commit branding updates**

Run:
```bash
git add src/layouts/Layout.astro
git commit -m "feat: update site branding and dynamic SVG logo to BSD Soluções"
```

---

### Task 2: Vertical Break on Homepage

**Files:**
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Refactor Homepage markup to stack Projects and Blog vertically**

Overwrite `src/pages/index.astro` to use vertical stacking, horizontal card lists, and a visual gradient break:
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

<Layout 
  title="BSD Soluções | Home"
  description="Bem-vindo à BSD Soluções. Desenvolvimento de software, inteligência de dados, sistemas de recomendação e portfólio de engenharia."
>
  <section class="hero">
    <h1>BSD Soluções</h1>
    <p class="subtitle">Inteligência de dados, sistemas de recomendação e arquitetura de software de alta performance.</p>
  </section>

  <section class="projects-section">
    <h2>Recent Projects</h2>
    <div class="projects-grid">
      {projects.map((project) => (
        <article class="glass-card project-card">
          <h3>{project.title}</h3>
          <p>{project.description}</p>
          <div class="tech-row">
            {project.tech.map((t) => <span class="badge">{t}</span>)}
          </div>
          <a href={project.link} class="project-link" aria-label={`Learn more about ${project.title}`}>Learn More &rarr;</a>
        </article>
      ))}
    </div>
  </section>

  <hr class="gradient-divider" />

  <section class="posts-section">
    <div class="posts-header">
      <h2>Latest From Blog</h2>
      <a href="/posts" class="view-all">View All Posts &rarr;</a>
    </div>
    <div class="posts-grid">
      {posts.map((post) => (
        <article class="glass-card post-card">
          <a href={`/posts/${post.id}`}>
            <h3>{post.data.title}</h3>
            {post.data.subtitle && <p class="post-sub">{post.data.subtitle}</p>}
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
    background-clip: text;
    -webkit-text-fill-color: transparent;
    margin-bottom: 8px;
  }
  .subtitle {
    font-size: 1.25rem;
    color: var(--text-secondary);
    max-width: 700px;
    margin: 0 auto;
  }
  h2 {
    font-size: 1.75rem;
    margin-bottom: 24px;
    border-left: 4px solid var(--accent-teal);
    padding-left: 12px;
  }
  .projects-section, .posts-section {
    margin-bottom: 48px;
  }
  .projects-grid, .posts-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 24px;
  }
  @media (max-width: 768px) {
    .projects-grid, .posts-grid {
      grid-template-columns: 1fr;
    }
  }
  .project-card, .post-card {
    display: flex;
    flex-direction: column;
    gap: 12px;
    transition: transform 0.2s, border-color 0.2s;
  }
  .project-card:hover, .post-card:hover {
    transform: translateY(-3px);
    border-color: var(--accent-indigo);
  }
  .project-card h3, .post-card h3 {
    font-size: 1.25rem;
  }
  .project-card p, .post-sub {
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
    margin-top: auto;
    font-size: 0.9rem;
    font-weight: 600;
    align-self: flex-start;
  }
  .gradient-divider {
    border: 0;
    height: 1px;
    background: linear-gradient(to right, transparent, var(--border-line), var(--accent-teal), var(--accent-indigo), var(--border-line), transparent);
    margin: 50px 0;
    opacity: 0.7;
  }
  .posts-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 24px;
  }
  .posts-header h2 {
    margin-bottom: 0;
  }
  .view-all {
    font-weight: 600;
  }
  .post-card time {
    font-size: 0.8rem;
    color: var(--accent-teal);
    font-weight: 600;
    margin-top: auto;
  }
</style>
```

- [ ] **Step 2: Run build to verify compilation**

Run: `npm run build`
Expected: PASS

- [ ] **Step 3: Commit homepage layout**

Run:
```bash
git add src/pages/index.astro
git commit -m "feat: restructure homepage with stacked grids and gradient visual break"
```

---

### Task 3: About Me LinkedIn Expansion

**Files:**
- Modify: `src/pages/about.astro`

- [ ] **Step 1: Write expanded About Me page CV timeline**

Overwrite `src/pages/about.astro` to restructure the layout as an elegant resume/career history layout:
```astro
---
import Layout from '../layouts/Layout.astro';
---

<Layout 
  title="About Me | Antonio Carlos"
  description="Conheça o histórico profissional, formação e competências de Antonio Carlos, Engenheiro de Software especializado em sistemas de recomendação e fundador da BSD Soluções."
>
  <div class="about-container">
    <section class="glass-card profile-card">
      <h1>Antonio Carlos</h1>
      <p class="subtitle">Software Engineer & Founder of BSD Soluções</p>
      <p class="bio">
        Especialista em desenvolvimento de software com ampla experiência no ecossistema Python, engenharia de dados e arquitetura de sistemas escaláveis. Desenvolvo sistemas inteligentes de personalização e recomendação para otimização de conversão e produto em startups. Focado em traduzir regras de negócios complexas em infraestrutura computacional robusta e ágil.
      </p>
    </section>

    <section class="timeline-section">
      <h2>Work Experience</h2>
      <div class="timeline">
        <article class="timeline-item glass-card">
          <div class="timeline-header">
            <h3>Founder & Tech Lead</h3>
            <span class="company">BSD Soluções</span>
            <time class="period">2024 - Present</time>
          </div>
          <p class="description">
            Liderança de projetos de desenvolvimento ágil de software e modelagem de arquitetura corporativa. Implementação de soluções de inteligência de dados sob medida, integrando APIs, automações de fluxos de trabalho e modelagem preditiva para clientes corporativos.
          </p>
        </article>

        <article class="timeline-item glass-card">
          <div class="timeline-header">
            <h3>Software Engineer (Machine Learning & Data)</h3>
            <span class="company">Tech Startup</span>
            <time class="period">2022 - 2024</time>
          </div>
          <p class="description">
            Arquitetura e desenvolvimento de modelos de filtragem colaborativa e recomendação de produtos em tempo real. Otimização de pipelines de dados em Python e integração de serviços em nuvem, gerando impacto direto na retenção de usuários e métricas de conversão.
          </p>
        </article>

        <article class="timeline-item glass-card">
          <div class="timeline-header">
            <h3>Full Stack Developer</h3>
            <span class="company">Software House</span>
            <time class="period">2020 - 2022</time>
          </div>
          <p class="description">
            Desenvolvimento de aplicações web responsivas de ponta a ponta. Construção de APIs REST robustas em Python/Django e interfaces modernas com JavaScript e CSS, seguindo metodologias ágeis e padrões SOLID.
          </p>
        </article>
      </div>
    </section>

    <section class="skills-section">
      <h2>Skills & Expertise</h2>
      <div class="skills-categories">
        <div class="skills-group glass-card">
          <h3>Languages & Core</h3>
          <ul class="skills-list">
            <li>Python</li>
            <li>JavaScript / TypeScript</li>
            <li>SQL (PostgreSQL, MySQL)</li>
            <li>HTML5 / CSS3</li>
          </ul>
        </div>

        <div class="skills-group glass-card">
          <h3>Data & Machine Learning</h3>
          <ul class="skills-list">
            <li>Recommendation Systems</li>
            <li>Pandas & NumPy</li>
            <li>Scikit-Learn</li>
            <li>ETL Pipelines</li>
          </ul>
        </div>

        <div class="skills-group glass-card">
          <h3>Frameworks & Tools</h3>
          <ul class="skills-list">
            <li>Astro</li>
            <li>Django / FastAPI</li>
            <li>Docker</li>
            <li>Git & GitHub Actions</li>
          </ul>
        </div>
      </div>
    </section>
  </div>
</Layout>

<style>
  .about-container {
    max-width: 800px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    gap: 40px;
  }
  .profile-card {
    border-left: 4px solid var(--accent-indigo);
  }
  h1 {
    font-size: 2.75rem;
    margin-bottom: 6px;
    background: linear-gradient(45deg, var(--accent-indigo), var(--accent-teal));
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  .profile-card .subtitle {
    font-size: 1.25rem;
    color: var(--accent-teal);
    font-weight: 600;
    margin-bottom: 20px;
  }
  .bio {
    font-size: 1.1rem;
    color: var(--text-secondary);
  }
  h2 {
    font-size: 1.75rem;
    margin-bottom: 24px;
    border-left: 4px solid var(--accent-indigo);
    padding-left: 12px;
  }
  .timeline {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }
  .timeline-item {
    transition: transform 0.2s;
  }
  .timeline-item:hover {
    transform: translateX(4px);
    border-color: var(--accent-teal);
  }
  .timeline-header {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    flex-wrap: wrap;
    gap: 8px;
    margin-bottom: 12px;
  }
  .timeline-header h3 {
    font-size: 1.25rem;
  }
  .company {
    color: var(--accent-teal);
    font-weight: 600;
    font-size: 1rem;
  }
  .period {
    color: var(--text-secondary);
    font-size: 0.9rem;
    font-weight: 500;
  }
  .description {
    color: var(--text-secondary);
    font-size: 0.95rem;
  }
  .skills-categories {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
  }
  @media (max-width: 768px) {
    .skills-categories {
      grid-template-columns: 1fr;
    }
  }
  .skills-group h3 {
    font-size: 1.15rem;
    margin-bottom: 16px;
    color: var(--accent-indigo);
    border-bottom: 1px solid var(--border-line);
    padding-bottom: 8px;
  }
  .skills-list {
    list-style: none;
    padding: 0;
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
  .skills-list li {
    font-size: 0.95rem;
    color: var(--text-secondary);
    position: relative;
    padding-left: 16px;
  }
  .skills-list li::before {
    content: "•";
    color: var(--accent-teal);
    position: absolute;
    left: 0;
    font-weight: bold;
  }
</style>
```

- [ ] **Step 2: Run build to verify compilation**

Run: `npm run build`
Expected: PASS

- [ ] **Step 3: Commit CV timeline About page**

Run:
```bash
git add src/pages/about.astro
git commit -m "feat: expand about page to professional resume timeline layout"
```

---

### Task 4: Formspree Contact Integration

**Files:**
- Modify: `src/pages/contact.astro`

- [ ] **Step 1: Add Formspree endpoint config and instruction callout**

Overwrite `src/pages/contact.astro` to add POST submission action and description guide:
```astro
---
import Layout from '../layouts/Layout.astro';
---

<Layout 
  title="Contact Me | Antonio Carlos"
  description="Entre em contato com Antonio Carlos na BSD Soluções para colaborações, orçamentos ou dúvidas."
>
  <div class="contact-container">
    <div class="glass-card">
      <h1>Let's Connect</h1>
      <p class="intro">
        Feel free to reach out for collaboration, inquiries, or just to talk about tech!
      </p>

      <!-- Note: To activate form submission, replace 'YOUR_FORMSPREE_ID' with your Formspree ID. -->
      <form class="contact-form" action="https://formspree.io/f/xlezgqeq" method="POST">
        <div class="form-group">
          <label for="name">Name</label>
          <input type="text" id="name" name="name" required placeholder="Your Name" autocomplete="name" />
        </div>
        <div class="form-group">
          <label for="email">Email</label>
          <input type="email" id="email" name="email" required placeholder="your.email@example.com" autocomplete="email" />
        </div>
        <div class="form-group">
          <label for="message">Message</label>
          <textarea id="message" name="message" rows="5" required placeholder="Write your message here..."></textarea>
        </div>
        <button type="submit" class="submit-btn">Send Message</button>
      </form>

      <div class="setup-notice">
        <p>💡 <strong>Note:</strong> Emails are sent directly using Formspree. Replace the Formspree ID in `src/pages/contact.astro` form action with your own to receive messages.</p>
      </div>

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

<script>
  const form = document.querySelector<HTMLFormElement>('.contact-form');
  const submitBtn = form?.querySelector<HTMLButtonElement>('.submit-btn');

  form?.addEventListener('submit', async (e) => {
    // We let the browser submit normally using Formspree POST, 
    // but update button text to show active sending state.
    if (submitBtn) {
      submitBtn.textContent = 'Sending Message...';
      submitBtn.disabled = true;
    }
  });
</script>

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
    margin-bottom: 24px;
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
    outline: 2px solid var(--accent-teal);
    outline-offset: 2px;
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
  .setup-notice {
    background: hsla(180, 100%, 50%, 0.05);
    border: 1px dashed hsla(180, 100%, 50%, 0.2);
    border-radius: 6px;
    padding: 12px 16px;
    margin-bottom: 32px;
    font-size: 0.85rem;
    color: var(--text-secondary);
  }
  .setup-notice strong {
    color: var(--accent-teal);
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

- [ ] **Step 2: Run build to verify compilation**

Run: `npm run build`
Expected: PASS

- [ ] **Step 3: Commit Formspree configuration**

Run:
```bash
git add src/pages/contact.astro
git commit -m "feat: integrate Formspree action handler in contact form"
```
