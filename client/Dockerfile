# =============================================================================
# client/Dockerfile — Production Vite SPA Runner for Railway
# =============================================================================
FROM node:20-alpine AS builder

WORKDIR /app

# Baked build-time arguments for Vite
ARG VITE_API_URL=https://server-production-3223ebce.up.railway.app
ENV VITE_API_URL=$VITE_API_URL

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

# ── Production Runner ────────────────────────────────────────────────────────
FROM node:20-alpine AS runner
WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY --from=builder /app/dist ./dist

ENV NODE_ENV=production
ENV PORT=3000
EXPOSE 3000

CMD ["npx", "serve", "-s", "dist", "-l", "3000"]
