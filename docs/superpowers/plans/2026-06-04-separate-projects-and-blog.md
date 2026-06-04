# Separating Projects and Blog/Posts Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Separate the visual layouts and URL routers for Projects/Products and Blog/Posts to reflect their conceptual differences.

**Architecture:** Configure separate Astro v6 content collections (`projects` and `blog`). Implement dynamic route templates for `/projects/[...id]` using a dual-column product showcase, and `/posts/[...id]` using a typography-focused text layout.

**Tech Stack:** Astro v6, TypeScript, HTML, CSS.

---

### Task 1: Content configuration and migration of the project post

**Files:**
- Modify: `src/content.config.ts`
- Create: `public/img/projects/recommendation-logo.svg`
- Move: `src/content/blog/modelo-recomendacao-python.md` -> `src/content/projects/modelo-recomendacao-python.md`
- Create: `src/content/blog/o-poder-dos-sistemas-recomendacao-negocios.md`

- [ ] **Step 1: Update src/content.config.ts**
  Replace content with the following:
  ```typescript
  import { defineCollection, z } from 'astro:content';
  import { glob } from 'astro/loaders';

  const blog = defineCollection({
    loader: glob({ pattern: '**/[^_]*.{md,mdx}', base: './src/content/blog' }),
    schema: z.object({
      title: z.string(),
      subtitle: z.string().optional(),
      date: z.coerce.date(),
      lang: z.string().optional(),
    }),
  });

  const projects = defineCollection({
    loader: glob({ pattern: '**/[^_]*.{md,mdx}', base: './src/content/projects' }),
    schema: z.object({
      title: z.string(),
      description: z.string(),
      date: z.coerce.date(),
      background: z.string(),
      logo: z.string().optional(),
      tech: z.array(z.string()),
      link: z.string().optional(),
      lang: z.string().optional(),
    }),
  });

  export const collections = { blog, projects };
  ```

- [ ] **Step 2: Create a placeholder logo for the recommendation systems project**
  Create `public/img/projects/recommendation-logo.svg` with:
  ```xml
  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" width="100" height="100">
    <defs>
      <linearGradient id="logo-grad-project" x1="0%" y1="0%" x2="100%" y2="100%">
        <stop offset="0%" stop-color="#8b5cf6" />
        <stop offset="100%" stop-color="#14b8a6" />
      </linearGradient>
    </defs>
    <rect width="100" height="100" rx="20" fill="#0f172a" />
    <circle cx="35" cy="50" r="8" fill="url(#logo-grad-project)" />
    <circle cx="65" cy="35" r="8" fill="url(#logo-grad-project)" />
    <circle cx="65" cy="65" r="8" fill="url(#logo-grad-project)" />
    <line x1="35" y1="50" x2="65" y2="35" stroke="#334155" stroke-width="4" />
    <line x1="35" y1="50" x2="65" y2="65" stroke="#334155" stroke-width="4" />
  </svg>
  ```

- [ ] **Step 3: Move and update the recommendation systems post to the projects folder**
  Move the file `src/content/blog/modelo-recomendacao-python.md` to `src/content/projects/modelo-recomendacao-python.md` and edit its frontmatter to match the new schema:
  ```markdown
  ---
  title: "Recommendation Systems in Python (An Overview)"
  description: "Understanding how it works, methodology, and how I implement it on a startup."
  date: 2022-05-28
  background: "/img/posts/modelo-recomendacao-python/capa.jpg"
  logo: "/img/projects/recommendation-logo.svg"
  tech: ["Python", "Scikit-Learn", "Collaborative Filtering", "Pandas"]
  link: "https://github.com/cobap/modelo-recomendacao-python"
  lang: pt
  ---
  
  ## Exemplo de uma tabela de recomendação:
  
  Photo by Brett Jordan on Unsplash
  
  Este é um exemplo de uma tabela de recomendação gerada pelo modelo:
  
  ![Recommendation](/img/posts/modelo-recomendacao-python/recommendation_table.png)
  ```

- [ ] **Step 4: Create the new text-centric blog post in Portuguese**
  Create `src/content/blog/o-poder-dos-sistemas-recomendacao-negocios.md` with:
  ```markdown
  ---
  title: "O Poder dos Sistemas de Recomendação nos Negócios"
  subtitle: "Como algoritmos inteligentes geram engajamento, aumentam o ticket médio e fidelizam clientes."
  date: 2026-06-04
  lang: pt
  ---

  No cenário digital atual, onde os consumidores são bombardeados por uma quantidade infinita de opções, a capacidade de apresentar o produto ou conteúdo certo no momento exato tornou-se um diferencial competitivo crucial. É aqui que entram os **sistemas de recomendação**.

  Mais do que ferramentas técnicas, esses sistemas são impulsionadores de receita comprovados. De acordo com estudos de mercado, cerca de **35% das vendas da Amazon** e mais de **80% do que assistimos na Netflix** são resultados diretos de recomendações algorítmicas personalizadas.

  Neste artigo, vamos explorar como essas ferramentas impactam as métricas de negócios e por que sua empresa deveria implementá-las.

  ---

  ## 1. Aumento do Ticket Médio e Vendas Cruzadas (Cross-Selling)

  Um dos benefícios mais imediatos de um modelo de recomendação é o estímulo a compras adicionais. Ao analisar os itens que um cliente colocou no carrinho, o algoritmo pode inferir necessidades complementares:

  - Se um usuário compra uma câmera DSLR, o sistema sugere cartões de memória, lentes ou tripés.
  - A recomendação é contextualizada, tornando-a útil em vez de intrusiva.

  Esse processo eleva o valor médio dos pedidos (AOV - *Average Order Value*), otimizando a conversão de tráfego que você já possui.

  ---

  ## 2. Redução do Custo de Aquisição de Clientes (CAC) através da Retenção

  Adquirir um novo cliente é de 5 a 25 vezes mais caro do que manter um cliente existente. Os algoritmos de recomendação desempenham um papel central nas estratégias de retenção:

  - **Personalização de Retorno:** Quando um cliente retorna ao site ou app, ele se depara com uma vitrine montada exclusivamente para seus gostos e preferências.
  - **E-mails e Notificações Inteligentes:** Enviar sugestões baseadas no histórico de navegação reativa usuários inativos de forma altamente assertiva.

  Ao manter o cliente engajado com novidades relevantes, você estende o *Lifetime Value* (LTV) e reduz a pressão por novas aquisições de tráfego pago.

  ---

  ## 3. Como Começar a Implementação?

  A complexidade de um sistema de recomendação pode variar desde soluções mais simples e baseadas em regras até modelos robustos de Inteligência Artificial:

  1. **Filtragem Baseada em Conteúdo (Content-Based):** Recomenda itens semelhantes aos que o usuário gostou no passado, analisando características do produto (ex: tags, categoria).
  2. **Filtragem Colaborativa (Collaborative Filtering):** Analisa padrões de comportamento de múltiplos usuários. Se o Usuário A e o Usuário B compraram os itens X e Y, e o Usuário A comprou o item Z, o sistema sugere Z para o Usuário B.
  3. **Modelos Híbridos:** Combinam ambas as técnicas com aprendizado profundo (Deep Learning) para máxima precisão.

  Na **BSD Soluções**, nós auxiliamos startups e empresas consolidadas a modelar e implantar essas arquiteturas diretamente em seus pipelines de dados, criando soluções sob medida.
  ```

- [ ] **Step 5: Verify schema changes**
  Run: `npx astro check`
  Expected: Command builds/validates type definitions successfully.

- [ ] **Step 6: Commit changes**
  Run: `git add src/content.config.ts public/img/projects/recommendation-logo.svg src/content/ && git commit -m "feat: configure blog and projects collections"`

---

### Task 2: Layout / Navigation Updates

**Files:**
- Modify: `src/layouts/Layout.astro:51-56`

- [ ] **Step 1: Add Projects to navigation navbar**
  Update the navigation links in `src/layouts/Layout.astro` to add Projects:
  ```html
  <nav>
    <a href="/" class={isActive('/') ? 'active' : ''} aria-current={isActive('/') ? 'page' : undefined}>Home</a>
    <a href="/projects" class={isActive('/projects') ? 'active' : ''} aria-current={isActive('/projects') ? 'page' : undefined}>Projects</a>
    <a href="/posts" class={isActive('/posts') ? 'active' : ''} aria-current={isActive('/posts') ? 'page' : undefined}>Blog</a>
    <a href="/about" class={isActive('/about') ? 'active' : ''} aria-current={isActive('/about') ? 'page' : undefined}>About</a>
    <a href="/contact" class={isActive('/contact') ? 'active' : ''} aria-current={isActive('/contact') ? 'page' : undefined}>Contact</a>
  </nav>
  ```

- [ ] **Step 2: Verify compile check**
  Run: `npx astro check`
  Expected: Successful type checks.

- [ ] **Step 3: Commit**
  Run: `git add src/layouts/Layout.astro && git commit -m "feat: add projects link to layout header"`

---

### Task 3: Projects Listing Page

**Files:**
- Create: `src/pages/projects/index.astro`

- [ ] **Step 1: Create the projects listing page**
  Create `src/pages/projects/index.astro` with the project cards grid styling:
  ```astro
  ---
  import { getCollection } from 'astro:content';
  import Layout from '../../layouts/Layout.astro';

  const projects = (await getCollection('projects')).sort(
    (a, b) => b.data.date.valueOf() - a.data.date.valueOf()
  );
  ---

  <Layout title="Projects | BSD Soluções" description="Explore our portfolio of digital products, AI systems, and custom software solutions.">
    <div class="projects-container">
      <div class="header-section">
        <h1>Our Projects</h1>
        <p class="subtitle">Digital products and technical solutions engineered by BSD Soluções.</p>
      </div>

      <div class="projects-grid">
        {projects.map((project) => (
          <a href={`/projects/${project.id}`} class="project-card-link">
            <article class="project-card">
              <div class="card-hero" style={`background-image: linear-gradient(rgba(15, 23, 42, 0.6), rgba(15, 23, 42, 0.85)), url(${project.data.background})`}>
                {project.data.logo && (
                  <img src={project.data.logo} alt={`${project.data.title} logo`} class="card-logo" />
                )}
              </div>
              <div class="card-content">
                <h2>{project.data.title}</h2>
                <p>{project.data.description}</p>
                <div class="card-tech">
                  {project.data.tech.slice(0, 3).map((t) => (
                    <span class="tech-tag">{t}</span>
                  ))}
                  {project.data.tech.length > 3 && (
                    <span class="tech-tag more">+{project.data.tech.length - 3}</span>
                  )}
                </div>
              </div>
            </article>
          </a>
        ))}
      </div>
    </div>
  </Layout>

  <style>
    .projects-container {
      max-width: 1000px;
      margin: 0 auto;
    }
    .header-section {
      text-align: center;
      margin-bottom: 48px;
    }
    h1 {
      font-size: 3rem;
      font-weight: 800;
      font-family: var(--font-header);
      margin-bottom: 12px;
      background: linear-gradient(45deg, #ffffff, var(--text-secondary));
      -webkit-background-clip: text;
      background-clip: text;
      -webkit-text-fill-color: transparent;
    }
    .subtitle {
      font-size: 1.25rem;
      color: var(--text-secondary);
      max-width: 600px;
      margin: 0 auto;
    }
    
    .projects-grid {
      display: grid;
      grid-template-columns: 1fr;
      gap: 32px;
    }
    @media (min-width: 768px) {
      .projects-grid {
        grid-template-columns: repeat(2, 1fr);
      }
    }

    .project-card-link {
      text-decoration: none;
      color: inherit;
    }
    .project-card {
      background: var(--bg-card);
      border: 1px solid var(--border-line);
      border-radius: 16px;
      overflow: hidden;
      height: 100%;
      display: flex;
      flex-direction: column;
      transition: all 0.3s ease;
      backdrop-filter: blur(8px);
    }
    .project-card:hover {
      transform: translateY(-4px);
      border-color: var(--accent-indigo);
      box-shadow: 0 12px 30px rgba(139, 92, 246, 0.15);
    }
    .card-hero {
      height: 180px;
      background-size: cover;
      background-position: center;
      position: relative;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .card-logo {
      width: 60px;
      height: 60px;
      border-radius: 12px;
      background: var(--bg-card);
      padding: 6px;
      border: 1.5px solid var(--border-line);
    }
    .card-content {
      padding: 24px;
      display: flex;
      flex-direction: column;
      flex-grow: 1;
    }
    .card-content h2 {
      font-size: 1.5rem;
      font-family: var(--font-header);
      margin: 0 0 12px 0;
      color: #ffffff;
    }
    .card-content p {
      color: var(--text-secondary);
      font-size: 0.95rem;
      line-height: 1.5;
      margin: 0 0 20px 0;
      flex-grow: 1;
    }
    .card-tech {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
    }
    .tech-tag {
      font-size: 0.75rem;
      padding: 2px 8px;
      border-radius: 4px;
      background: hsla(250, 95%, 70%, 0.1);
      color: var(--accent-indigo);
      border: 1px solid hsla(250, 95%, 70%, 0.15);
      font-weight: 500;
    }
    .tech-tag.more {
      background: hsla(180, 100%, 50%, 0.1);
      color: var(--accent-teal);
      border: 1px solid hsla(180, 100%, 50%, 0.15);
    }
  </style>
  ```

- [ ] **Step 2: Verify page compilability**
  Run: `npx astro check`
  Expected: Successful validation.

- [ ] **Step 3: Commit**
  Run: `git add src/pages/projects/index.astro && git commit -m "feat: add projects listing page"`

---

### Task 4: Project Details Page

**Files:**
- Create: `src/pages/projects/[...id].astro`

- [ ] **Step 1: Create dynamic route page for single projects**
  Create `src/pages/projects/[...id].astro` with the dynamic loader and two-column specification layout:
  ```astro
  ---
  import { getCollection, render } from 'astro:content';
  import Layout from '../../layouts/Layout.astro';

  export async function getStaticPaths() {
    const projects = await getCollection('projects');
    return projects.map((project) => ({
      params: { id: project.id },
      props: project,
    }));
  }

  const project = Astro.props;
  const { Content } = await render(project);
  ---

  <Layout title={project.data.title} description={project.data.description} lang={project.data.lang}>
    <div class="project-container">
      <!-- Full-Width Hero Section -->
      <div class="project-hero" style={`background-image: linear-gradient(rgba(15, 23, 42, 0.8), rgba(15, 23, 42, 0.95)), url(${project.data.background})`}>
        <div class="hero-content">
          {project.data.logo && (
            <img src={project.data.logo} alt={`${project.data.title} logo`} class="project-logo" />
          )}
          <h1 class="project-title">{project.data.title}</h1>
          <p class="project-description">{project.data.description}</p>
        </div>
      </div>

      <!-- Two Column Layout -->
      <div class="project-layout">
        <!-- Main Content Column -->
        <main class="project-main">
          <div class="markdown-body">
            <Content />
          </div>
        </main>

        <!-- Sidebar Column -->
        <aside class="project-sidebar">
          <div class="sidebar-card">
            <h3>Specifications</h3>
            
            <div class="spec-group">
              <span class="spec-label">Released</span>
              <span class="spec-value">
                {project.data.date.toLocaleDateString('en-US', {
                  year: 'numeric',
                  month: 'long',
                  day: 'numeric',
                })}
              </span>
            </div>

            <div class="spec-group">
              <span class="spec-label">Tech Stack</span>
              <div class="tech-pills">
                {project.data.tech.map((t) => (
                  <span class="tech-pill">{t}</span>
                ))}
              </div>
            </div>

            {project.data.link && (
              <a href={project.data.link} target="_blank" rel="noopener noreferrer" class="project-link-btn">
                Visit Product
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" class="btn-icon">
                  <path fill-rule="evenodd" d="M5.22 14.78a.75.75 0 001.06 0l7.22-7.22v5.69a.75.75 0 001.5 0v-7.5a.75.75 0 00-.75-.75h-7.5a.75.75 0 000 1.5h5.69l-7.22 7.22a.75.75 0 000 1.06z" clip-rule="evenodd" />
                </svg>
              </a>
            )}
          </div>
        </aside>
      </div>
    </div>
  </Layout>

  <style>
    .project-container {
      max-width: 1000px;
      margin: 0 auto;
    }
    .project-hero {
      border-radius: 16px;
      background-size: cover;
      background-position: center;
      padding: 60px 40px;
      margin-bottom: 40px;
      border: 1px solid var(--border-line);
      text-align: center;
    }
    .hero-content {
      max-width: 800px;
      margin: 0 auto;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 16px;
    }
    .project-logo {
      width: 80px;
      height: 80px;
      border-radius: 16px;
      border: 2px solid var(--border-line);
      background: var(--bg-card);
      padding: 8px;
      box-shadow: 0 8px 30px rgba(0, 0, 0, 0.3);
    }
    .project-title {
      font-size: 2.5rem;
      font-weight: 800;
      font-family: var(--font-header);
      margin: 0;
      background: linear-gradient(to right, #ffffff, var(--text-secondary));
      -webkit-background-clip: text;
      background-clip: text;
      -webkit-text-fill-color: transparent;
    }
    .project-description {
      font-size: 1.25rem;
      color: var(--text-secondary);
      margin: 0;
      max-width: 600px;
    }
    
    .project-layout {
      display: grid;
      grid-template-columns: 1fr;
      gap: 32px;
    }
    @media (min-width: 768px) {
      .project-layout {
        grid-template-columns: 1.8fr 1fr;
      }
    }

    .project-main {
      background: var(--bg-card);
      border: 1px solid var(--border-line);
      border-radius: 16px;
      padding: 32px;
      backdrop-filter: blur(8px);
    }
    
    .project-sidebar {
      display: flex;
      flex-direction: column;
      gap: 24px;
    }
    .sidebar-card {
      position: sticky;
      top: 100px;
      background: var(--bg-card);
      border: 1px solid var(--border-line);
      border-radius: 16px;
      padding: 24px;
      backdrop-filter: blur(8px);
      display: flex;
      flex-direction: column;
      gap: 20px;
    }
    .sidebar-card h3 {
      font-family: var(--font-header);
      font-size: 1.25rem;
      margin: 0 0 8px 0;
      color: var(--text-primary);
      border-bottom: 1px solid var(--border-line);
      padding-bottom: 12px;
    }
    .spec-group {
      display: flex;
      flex-direction: column;
      gap: 8px;
    }
    .spec-label {
      font-size: 0.875rem;
      color: var(--text-secondary);
      text-transform: uppercase;
      letter-spacing: 0.05em;
    }
    .spec-value {
      font-size: 1rem;
      color: var(--text-primary);
      font-weight: 500;
    }
    .tech-pills {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
    }
    .tech-pill {
      font-size: 0.85rem;
      padding: 4px 10px;
      border-radius: 6px;
      background: hsla(250, 95%, 70%, 0.1);
      color: var(--accent-indigo);
      border: 1px solid hsla(250, 95%, 70%, 0.2);
      font-weight: 500;
    }
    .project-link-btn {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      background: linear-gradient(135deg, var(--accent-indigo), var(--accent-teal));
      color: #ffffff;
      font-weight: 600;
      padding: 12px 20px;
      border-radius: 8px;
      text-decoration: none;
      transition: all 0.3s ease;
      border: none;
      cursor: pointer;
    }
    .project-link-btn:hover {
      transform: translateY(-2px);
      box-shadow: 0 4px 20px rgba(20, 184, 166, 0.4);
    }
    .btn-icon {
      width: 18px;
      height: 18px;
    }
    .markdown-body :global(p) {
      margin-bottom: 20px;
      line-height: 1.6;
      color: var(--text-secondary);
    }
    .markdown-body :global(h2) {
      font-family: var(--font-header);
      font-size: 1.75rem;
      margin-top: 32px;
      margin-bottom: 16px;
      color: var(--text-primary);
    }
    .markdown-body :global(img) {
      max-width: 100%;
      border-radius: 8px;
      margin: 24px 0;
      border: 1px solid var(--border-line);
    }
  </style>
  ```

- [ ] **Step 2: Verify route generation**
  Run: `npx astro check`
  Expected: Successful compilation check.

- [ ] **Step 3: Commit**
  Run: `git add src/pages/projects/\[...id\].astro && git commit -m "feat: implement single project dynamic route page"`

---

### Task 5: Blog Listing & Detail Page updates

**Files:**
- Modify: `src/pages/posts/[...id].astro`
- Modify: `src/pages/posts/index.astro`

- [ ] **Step 1: Update src/pages/posts/[...id].astro for typography-first reading layout**
  Rewrite `src/pages/posts/[...id].astro`:
  ```astro
  ---
  import { getCollection, render } from 'astro:content';
  import Layout from '../../layouts/Layout.astro';

  export async function getStaticPaths() {
    const posts = await getCollection('blog');
    return posts.map((post) => ({
      params: { id: post.id },
      props: post,
    }));
  }

  const post = Astro.props;
  const { Content } = await render(post);

  // Calculate reading time dynamically
  const wordCount = post.body ? post.body.split(/\s+/).length : 0;
  const readingTime = Math.max(1, Math.ceil(wordCount / 200));
  ---

  <Layout title={post.data.title} description={post.data.subtitle} lang={post.data.lang}>
    <article class="post-content">
      <header class="post-header">
        <time datetime={post.data.date.toISOString()}>
          {post.data.date.toLocaleDateString('pt-BR', {
            year: 'numeric',
            month: 'long',
            day: 'numeric',
          })}
        </time>
        <h1>{post.data.title}</h1>
        {post.data.subtitle && <p class="subtitle">{post.data.subtitle}</p>}
        <div class="read-metadata">
          <span>⏱️ {readingTime} min de leitura</span>
        </div>
      </header>
      
      <hr class="separator" />
      
      <div class="markdown-body">
        <Content />
      </div>
    </article>
  </Layout>

  <style>
    .post-content {
      max-width: 680px;
      margin: 40px auto;
      font-size: 1.125rem;
      line-height: 1.8;
    }
    .post-header {
      text-align: center;
      margin-bottom: 32px;
    }
    time {
      display: block;
      color: var(--accent-teal);
      font-weight: 600;
      font-size: 0.95rem;
      text-transform: uppercase;
      letter-spacing: 0.05em;
      margin-bottom: 12px;
    }
    h1 {
      font-size: 2.5rem;
      font-weight: 800;
      font-family: var(--font-header);
      margin-bottom: 16px;
      line-height: 1.25;
      color: #ffffff;
    }
    .subtitle {
      font-size: 1.2rem;
      color: var(--text-secondary);
      line-height: 1.5;
      margin-bottom: 16px;
    }
    .read-metadata {
      font-size: 0.875rem;
      color: var(--text-secondary);
    }
    .separator {
      border: 0;
      border-top: 1px solid var(--border-line);
      margin-bottom: 40px;
    }
    .markdown-body :global(p) {
      margin-bottom: 24px;
      color: var(--text-secondary);
    }
    .markdown-body :global(h2) {
      font-family: var(--font-header);
      font-size: 1.75rem;
      margin-top: 40px;
      margin-bottom: 16px;
      color: #ffffff;
    }
    .markdown-body :global(ul), .markdown-body :global(ol) {
      margin-bottom: 24px;
      padding-left: 24px;
      color: var(--text-secondary);
    }
    .markdown-body :global(li) {
      margin-bottom: 8px;
    }
    .markdown-body :global(hr) {
      border: 0;
      border-top: 1px solid var(--border-line);
      margin: 32px 0;
    }
  </style>
  ```

- [ ] **Step 2: Update src/pages/posts/index.astro for styling consistency**
  Rewrite `src/pages/posts/index.astro` to list posts:
  ```astro
  ---
  import { getCollection } from 'astro:content';
  import Layout from '../../layouts/Layout.astro';

  const posts = (await getCollection('blog')).sort(
    (a, b) => b.data.date.valueOf() - a.data.date.valueOf()
  );
  ---

  <Layout 
    title="Blog | BSD Soluções"
    description="Explore technical insights and knowledge articles about software engineering and machine learning."
  >
    <div class="archive-container">
      <div class="header-section">
        <h1>Blog</h1>
        <p class="subtitle">Deep dives and technical insights from our engineering team.</p>
      </div>

      <div class="post-grid">
        {posts.map((post) => (
          <article class="glass-card post-card">
            <a href={`/posts/${post.id}`}>
              <h2>{post.data.title}</h2>
              {post.data.subtitle && <p class="post-sub">{post.data.subtitle}</p>}
              <time datetime={post.data.date.toISOString()}>
                {post.data.date.toLocaleDateString('pt-BR', {
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
    .archive-container {
      max-width: 800px;
      margin: 0 auto;
    }
    .header-section {
      text-align: center;
      margin-bottom: 48px;
    }
    h1 {
      font-size: 3rem;
      font-weight: 800;
      font-family: var(--font-header);
      margin-bottom: 12px;
      background: linear-gradient(45deg, #ffffff, var(--text-secondary));
      -webkit-background-clip: text;
      background-clip: text;
      -webkit-text-fill-color: transparent;
    }
    .subtitle {
      font-size: 1.25rem;
      color: var(--text-secondary);
      margin: 0;
    }
    .post-grid {
      display: flex;
      flex-direction: column;
      gap: 24px;
    }
    .post-card {
      transition: all 0.3s ease;
      padding: 24px;
      border: 1px solid var(--border-line);
      border-radius: 12px;
      background: var(--bg-card);
    }
    .post-card:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 30px rgba(0, 0, 0, 0.3);
      border-color: var(--accent-indigo);
    }
    .post-card a {
      text-decoration: none;
      color: inherit;
      display: block;
    }
    h2 {
      font-size: 1.5rem;
      margin: 0 0 8px 0;
      color: #ffffff;
      font-family: var(--font-header);
    }
    .post-sub {
      color: var(--text-secondary);
      margin: 0 0 16px 0;
      font-size: 1rem;
      line-height: 1.4;
    }
    time {
      font-size: 0.875rem;
      color: var(--accent-teal);
      font-weight: 600;
      text-transform: uppercase;
      letter-spacing: 0.05em;
    }
  </style>
  ```

- [ ] **Step 3: Verify compile step**
  Run: `npx astro check`
  Expected: Successful compilation checks.

- [ ] **Step 4: Commit**
  Run: `git add src/pages/posts/ && git commit -m "feat: restructure blog lists and single posts for typography readability"`

---

### Task 6: Homepage Refactoring

**Files:**
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Rewrite homepage to list dynamic projects and blog posts**
  Rewrite `src/pages/index.astro` to fetch collections dynamically and display them using our standard layout styling:
  ```astro
  ---
  import { getCollection } from 'astro:content';
  import Layout from '../layouts/Layout.astro';

  const posts = (await getCollection('blog'))
    .sort((a, b) => b.data.date.valueOf() - a.data.date.valueOf())
    .slice(0, 3);

  const projects = (await getCollection('projects'))
    .sort((a, b) => b.data.date.valueOf() - a.data.date.valueOf())
    .slice(0, 2);
  ---

  <Layout 
    title="BSD Soluções | Home"
    description="Bem-vindo à BSD Soluções. Desenvolvimento de software, inteligência de dados, sistemas de recomendação e engenharia."
  >
    <section class="hero">
      <h1>BSD Soluções</h1>
      <p class="subtitle">Inteligência de dados, sistemas de recomendação e arquitetura de software de alta performance.</p>
    </section>

    <section class="projects-section">
      <div class="section-header">
        <h2>Recent Projects</h2>
        <a href="/projects" class="view-all">View All Projects &rarr;</a>
      </div>
      <div class="projects-grid">
        {projects.map((project) => (
          <a href={`/projects/${project.id}`} class="project-card-link">
            <article class="project-card">
              <div class="card-hero" style={`background-image: linear-gradient(rgba(15, 23, 42, 0.6), rgba(15, 23, 42, 0.85)), url(${project.data.background})`}>
                {project.data.logo && (
                  <img src={project.data.logo} alt={`${project.data.title} logo`} class="card-logo" />
                )}
              </div>
              <div class="card-content">
                <h3>{project.data.title}</h3>
                <p>{project.data.description}</p>
                <div class="card-tech">
                  {project.data.tech.slice(0, 3).map((t) => (
                    <span class="tech-tag">{t}</span>
                  ))}
                </div>
              </div>
            </article>
          </a>
        ))}
      </div>
    </section>

    <hr class="gradient-divider" />

    <section class="posts-section">
      <div class="section-header">
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
                {post.data.date.toLocaleDateString('pt-BR', {
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
      border-left: 4px solid var(--accent-teal);
      padding-left: 12px;
      margin: 0;
    }
    .section-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 24px;
    }
    .view-all {
      font-weight: 600;
      font-size: 0.95rem;
      text-decoration: none;
      color: var(--accent-teal);
    }
    .view-all:hover {
      color: var(--text-primary);
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
    .project-card-link {
      text-decoration: none;
      color: inherit;
    }
    .project-card {
      background: var(--bg-card);
      border: 1px solid var(--border-line);
      border-radius: 16px;
      overflow: hidden;
      height: 100%;
      display: flex;
      flex-direction: column;
      transition: all 0.3s ease;
      backdrop-filter: blur(8px);
    }
    .project-card:hover {
      transform: translateY(-3px);
      border-color: var(--accent-indigo);
      box-shadow: 0 10px 25px rgba(139, 92, 246, 0.15);
    }
    .card-hero {
      height: 150px;
      background-size: cover;
      background-position: center;
      position: relative;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .card-logo {
      width: 50px;
      height: 50px;
      border-radius: 10px;
      background: var(--bg-card);
      padding: 6px;
      border: 1.5px solid var(--border-line);
    }
    .card-content {
      padding: 20px;
      display: flex;
      flex-direction: column;
      flex-grow: 1;
    }
    .card-content h3 {
      font-size: 1.25rem;
      margin: 0 0 8px 0;
      color: #ffffff;
    }
    .card-content p {
      color: var(--text-secondary);
      font-size: 0.9rem;
      line-height: 1.5;
      margin: 0 0 16px 0;
      flex-grow: 1;
    }
    .card-tech {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
    }
    .tech-tag {
      font-size: 0.7rem;
      padding: 2px 6px;
      border-radius: 4px;
      background: hsla(250, 95%, 70%, 0.1);
      color: var(--accent-indigo);
      border: 1px solid hsla(250, 95%, 70%, 0.15);
      font-weight: 500;
    }
    .post-card {
      display: flex;
      flex-direction: column;
      gap: 12px;
      transition: all 0.3s ease;
      padding: 20px;
      border: 1px solid var(--border-line);
      border-radius: 12px;
      background: var(--bg-card);
    }
    .post-card:hover {
      transform: translateY(-3px);
      border-color: var(--accent-indigo);
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
    }
    .post-card a {
      text-decoration: none;
      color: inherit;
      display: flex;
      flex-direction: column;
      height: 100%;
    }
    .post-card h3 {
      font-size: 1.25rem;
      margin: 0 0 8px 0;
      color: #ffffff;
    }
    .post-sub {
      color: var(--text-secondary);
      font-size: 0.9rem;
      line-height: 1.4;
      margin: 0 0 12px 0;
      flex-grow: 1;
    }
    .post-card time {
      font-size: 0.8rem;
      color: var(--accent-teal);
      font-weight: 600;
      text-transform: uppercase;
      letter-spacing: 0.05em;
    }
    .gradient-divider {
      border: 0;
      height: 1px;
      background: linear-gradient(to right, transparent, var(--border-line), var(--accent-teal), var(--accent-indigo), var(--border-line), transparent);
      margin: 40px 0;
      opacity: 0.7;
    }
  </style>
  ```

- [ ] **Step 2: Run final build verification**
  Run: `npm run build`
  Expected: Production static files generated successfully under `dist/` with no loader or compiler errors.

- [ ] **Step 3: Commit**
  Run: `git add src/pages/index.astro && git commit -m "feat: dynamically load and display projects and posts on homepage"`
