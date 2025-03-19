# ---- build stage ----
FROM node:20-alpine AS builder
WORKDIR /app

# install dependencies
COPY package*.json ./
RUN npm ci --omit=dev

# copy source code
COPY . .

# ---- runtime stage ----
FROM node:20-alpine AS runtime
ENV NODE_ENV=production
WORKDIR /app

# non-root user
RUN addgroup -S nodegrp && adduser -S nodeusr -G nodegrp

# copy runtime files
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package*.json ./
COPY --from=builder /app ./
# 👆 brings in config/, controllers/, middleware/, models/, routes/, utils/, server.js, etc.

ARG PORT=3000
ENV PORT=${PORT}
EXPOSE ${PORT}

# healthcheck (adjust path if your app uses /api/health)
HEALTHCHECK --interval=30s --timeout=5s --start-period=20s --retries=3 \
  CMD wget -qO- "http://localhost:${PORT}/health" || exit 1

USER nodeusr
CMD ["node", "server.js"]

