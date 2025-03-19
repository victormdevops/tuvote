# ---- build stage ----
FROM node:20-alpine AS builder
WORKDIR /app

# install deps
COPY package*.json ./
RUN npm ci


# copy source code
COPY . .

# build the frontend
RUN npm run build

# ---- runtime stage ----
FROM nginx:alpine

# copy built files into nginx html dir
COPY --from=builder /app/dist /usr/share/nginx/html

# replace default nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

HEALTHCHECK --interval=30s --timeout=5s --start-period=20s --retries=3 \
  CMD wget -qO- http://localhost || exit 1


