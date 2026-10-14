FROM node:22-alpine

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .
RUN npm run build

ENV NODE_ENV=production

EXPOSE 8443

# O vite.config.ts lê PORT e API_PROXY_TARGET em runtime. O proxy mantém
# /api no mesmo domínio público do frontend e alcança o backend pela rede privada.
CMD ["npm", "run", "preview"]
