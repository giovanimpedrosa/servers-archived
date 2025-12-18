FROM node:22-alpine

WORKDIR /app

# Copia apenas os manifests primeiro (cache eficiente)
COPY package.json package-lock.json ./

RUN npm ci --omit-dev

# Copia o código-fonte
COPY src ./src

ENV NODE_ENV=production

# Log inicial para debug
CMD ["sh", "-c", "echo 'Starting MCP Postgres' && echo \"DATABASE_URL=$DATABASE_URL\" && node src/index.js \"$DATABASE_URL\""]
