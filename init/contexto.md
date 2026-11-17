## `/init`

- Realiza a criação da contextualização inicial do projeto
- Gera .md do CLAUDE

## CLAUDE.md

- Claude lê no inicio de cada sessão
- Arquivo organizado, enxuto

### O que ter no CLAUDE.md

- Comandos que Claude não consegue adivinhar
- Regras de estilo de código que diferem dos padrões
- Instruções de testes
- Versionamento do código (nomenclatura de branches, convenções de PR, workFlow)
- Decisões arquitetônicas específicas para seu projeto
- Particularidades do ambiente de desenvolvimento (envs)
- construir CLAUDE.md com ênfase em palavras como:
  - "IMPORTANTE", "DEVE-SE", "NUNCA FAÇA"

### O que não ter no CLAUDE.md

- Qualquer coisa que Claude consiga descobrir lendo código
- Documentação detalhada da API
- Infos que mudam frequentemente
- Explicações longas
- Convenções de Linguagens (para suprir isso, é usado skills)

---

### Exemplo de CLAUDE.md

```md
# Project: [Nome do Projeto]

## Tech Stack

- Next.js 16 (App Router), React 19, TypeScript
- TailwindCSS 4, shadcn/ui
- React Hook Form + Zod (validação)
- Server Component First

## Commands

- `npm run dev` — servidor local (porta 3000)
- `npm run build` — build de produção

## Architecture

- App Router: rotas em `app/`, agrupadas por `(grupo)/`
- Server Components por padrão — só adicionar `'use client'` quando usar hooks/eventos/browser APIs
- Mutações via Server Actions em `actions/` — NUNCA chamar DB direto em Client Components
- `components/ui/` — primitivos reutilizáveis (shadcn)
- `components/` — componentes de feature
- `lib/` — helpers, clients (supabase, stripe), configurações
- `types/` — tipos globais e schemas Zod compartilhados

## Code Style

- NEVER use `any` explícito — usar `unknown` + type guard
- Imports: ES modules (import/export), sem require()
- Tailwind only — sem CSS inline, sem styled-components
- Novos design tokens vão em `tailwind.config.ts` antes de usar
- Nomes de arquivo: kebab-case. Componentes: PascalCase

## Environment Variables

- `NEXT_PUBLIC_*` apenas para valores seguros no client
- Segredos (DB, API keys) apenas em Server Actions ou Route Handlers
- Copiar `.env.example` para `.env.local` ao clonar

## Workflow

- ALWAYS run `npm run type-check && npm run lint` após uma série de mudanças
- Rodar um teste por vez, não o suite completo: `npm run test -- NomeDoArquivo`
- Branch naming: `feat/`, `fix/`, `chore/` + descrição em kebab-case
- Commits em inglês, imperativo: "add OAuth callback handler"

## Common Gotchas

- `revalidatePath()` e `revalidateTag()` só funcionam em Server Actions/Route Handlers
- Middleware em `middleware.ts` na raiz — não dentro de `app/`
- Supabase client no server: usar `createServerClient` (cookies). No client: `createBrowserClient`
- Imagens externas precisam de domínio autorizado em `next.config.ts` (remotePatterns)
```
