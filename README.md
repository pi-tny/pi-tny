# TNY — Catálogo digital e gestão de pedidos

Sistema fullstack para catálogo digital de uma marca de moda masculina. Clientes navegam
pelos produtos, montam o carrinho e finalizam o pedido via **WhatsApp**; administradores
gerenciam catálogo, pedidos e leads por um painel protegido por **JWT**.

Este repositório é a página central do projeto — reúne a documentação e liga o frontend
e o backend, que vivem em repositórios separados.

## Repositórios

| Camada | Repositório | Stack |
|---|---|---|
| Frontend (SPA) | https://github.com/pi-tny/tny-frontend | React 19 · Vite · Tailwind CSS 4 |
| Backend (API REST) | https://github.com/pi-tny/tny-backend | Fastify 5 · Prisma 7 · TypeScript |

## Visão geral

O TNY tem duas frentes:

- **Vitrine pública** — navegação por categorias e produtos, visualização de variantes
  (cor/tamanho), carrinho persistido e checkout com geração de link de WhatsApp
  (mensagem pré-formatada com itens, quantidades e total).
- **Painel administrativo** — área protegida por JWT para gerenciar categorias, produtos,
  variantes, imagens, pedidos, leads e administradores.

O pedido é finalizado pelo WhatsApp para reduzir complexidade (pagamentos, conciliação,
compliance) e favorecer o atendimento direto — importante para marcas de nicho.

## Modelo de dados (MER)

![Diagrama Entidade-Relacionamento do TNY](mer.png)

Entidades centrais: `Product`, `Variant`, `Image`, `Category`, `Order`, `OrderItem`,
`Lead` e `Admin`.

## Arquitetura

```
┌──────────────────────┐      HTTP/JSON      ┌──────────────────────┐      ┌───────────────────────────┐
│  tny-frontend        │  ───────────────▶   │  tny-backend         │ ───▶ │  Banco de dados           │
│  React · Vite · TW   │   (VITE_API_URL)    │  Fastify · Prisma    │      │  SQLite (dev) / PG (prod) │
└──────────────────────┘                     └──────────────────────┘      └───────────────────────────┘
```

## Como rodar (resumo)

Requer Node.js 22+. Rode backend e frontend em terminais separados:

```bash
# Backend — API em http://localhost:3000 (Swagger em /docs)
cd tny-backend
cp .env.example .env      # defina JWT_SECRET e DATABASE_URL
npm install
npm run db:migrate
npm run db:seed           # opcional: dados de exemplo
npm run dev

# Frontend — SPA em http://localhost:5173
cd tny-frontend
cp .env.example .env      # VITE_API_URL aponta para o backend
npm install
npm run dev
```

Para o ambiente de produção com Docker (PostgreSQL + API) e detalhes de configuração,
veja a documentação abaixo.

## Documentação

- **[Documentação técnica detalhada](documentacao-tecnica-detalhada.md)** — arquitetura,
  banco de dados, referência de rotas da API, testes, containerização e deploy.
- **[Guia de instalação](instalacao.md)** — passo a passo de pré-requisitos, instalação e
  variáveis de ambiente.
- **[Design no Figma](https://www.figma.com/design/JV3aZKjoB5WZ6nLo2tatJR/PROJETO-TNY---MARCO-02?node-id=0-1&t=FXbnAJC6b4N1AEd7-1)** — protótipos e identidade visual do projeto.

## Stack

- **Frontend:** React 19, Vite 8, Tailwind CSS 4, React Router 7, TanStack Query, Axios, Zod.
- **Backend:** Fastify 5, Prisma 7, Zod 4, `@fastify/jwt`, Swagger UI, Vitest.
- **Banco:** SQLite (dev/testes) · PostgreSQL 16 (produção/Docker).

## Licença

[MIT](LICENSE)
