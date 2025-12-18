# =========================
# Build (TypeScript)
# =========================
FROM node:22-alpine AS builder

WORKDIR /app

# Copia arquivos base
COPY package.json package-lock.json tsconfig.json ./

# Copia TODO o src (obrigatório)
COPY src ./src

# Instala dependências (inclui devDependencies)
RUN npm ci

# Compila TypeScript -> dist/
RUN npx tsc


# =========================
# Runtime
# =========================
FROM node:22-alpine AS runtime

WORKDIR /app

ENV NODE_ENV=production

# Copia JS compilado
COPY --from=builder /app/dist ./dist

# Copia package.json para runtime
COPY --from=builder /app/package.json ./
COPY --from=builder /app/package-lock.json ./

# Instala somente dependências de produção
RUN npm ci --omit-dev

# MCP Postgres é gerado em: dist/postgres/index.js
CMD ["sh", "-c", "echo \"Starting MCP Postgres\" && echo \"DATABASE_URL=$DATABASE_URL\" && node dist/postgres/index.js \"$DATABASE_URL\""]
