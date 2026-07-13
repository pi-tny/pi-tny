## 1. Pré-instalção

Antes de iniciar, garanta que os seguintes requisitos de software estão instalados:

| Software | Versão mínima | Verificação |
|---|---|---|
| Node.js | 22.x | `node --version` |
| npm | 10.x | `npm --version` |
| Git | qualquer | `git --version` |
| Docker | 24.x (opcional) | `docker --version` |
| Docker Compose | 2.x (opcional) | `docker compose version` |

> **Nota:** Docker é necessário apenas para execução em modo de produção via containers. O modo de desenvolvimento funciona sem Docker.

---

## 2. Instalação e Configuração

### 2.1 Clonar o Repositório

```bash
# Clone os repositórios (ou o monorepo, conforme estrutura do projeto)
git clone https://github.com/pi-tny/tny-backend
git clone https://github.com/pi-tny/tny-frontend
```

### 2.2 Configurar e Instalar o Backend

```bash
cd tny-backend

# 1. Copiar arquivo de variáveis de ambiente
cp .env.example .env

# 2. Editar o .env com suas configurações (ver seção 2.4)
# No mínimo, defina JWT_SECRET e confirme DATABASE_URL

# 3. Instalar dependências
npm install

# 4. Aplicar as migrations do banco de dados
npm run db:migrate

# 5. (Opcional) Popular com dados de exemplo
npm run db:seed
```

### 2.3 Configurar e Instalar o Frontend

```bash
cd tny-frontend

# 1. Copiar arquivo de variáveis de ambiente
cp .env.example .env

# 2. Instalar dependências
npm install
```

> O frontend usa `VITE_API_URL` (padrão `http://localhost:3000`) para localizar a API do
> backend. Ela é lida em **tempo de build** pelo Vite — em dev basta o `.env`; para
> Docker/produção, passe-a no `docker build` (`--build-arg VITE_API_URL=...`).

### 2.4 Variáveis de Ambiente do Backend

O arquivo `.env` (criado a partir de `.env.example`) controla o comportamento da API, logo abaixo pode observar as opções de customização, utilize-as para configuração do ''.env'':

| Variável | Obrigatória | Padrão | Descrição |
|---|---|---|---|
| `DATABASE_URL` | **Sim** | — | URL de conexão com o banco. Ex: `file:./prisma/dev.db` (SQLite) ou `postgresql://user:pass@host:5432/db` (Postgres) |
| `JWT_SECRET` | **Sim** | — | Chave secreta para assinar e verificar tokens JWT. Use uma string longa e aleatória em produção |
| `DATABASE_PROVIDER` | Não | `sqlite` | Provedor do banco: `sqlite` ou `postgres`. Controla qual migration e adapter Prisma usa |
| `PORT` | Não | `3000` | Porta em que a API vai escutar |
| `NODE_ENV` | Não | `dev` | Ambiente: `dev`, `test` ou `production` |
| `CORS_ORIGIN` | Não | `*` | Origem permitida para CORS. Em produção, defina a URL do frontend |
| `WHATSAPP_NUMBER` | Não | `""` | Número do WhatsApp da loja (formato: `5585981000000`). Usado na geração do link de checkout |

**Exemplo de `.env` para desenvolvimento:**

```dotenv
DATABASE_PROVIDER="sqlite"
DATABASE_URL="file:./prisma/dev.db"
PORT=3000
NODE_ENV=dev
JWT_SECRET="minha-chave-secreta-muito-longa-e-segura"
CORS_ORIGIN="http://localhost:5173"
WHATSAPP_NUMBER="5585981025616"
```

---