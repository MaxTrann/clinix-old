# Yêu cầu: apps/web là project React + Vite (npm), có script `dev` và `build`.
FROM node:22-alpine AS base
WORKDIR /app
COPY package*.json ./

# --- dev: Vite dev server, dùng với docker-compose.yml ---
FROM base AS dev
RUN npm ci
COPY . .
EXPOSE 5173
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]

# --- build: VITE_API_URL được nhúng lúc build ---
FROM base AS build
ARG VITE_API_URL
ENV VITE_API_URL=$VITE_API_URL
RUN npm ci
COPY . .
RUN npm run build

# --- prod: nginx phục vụ file tĩnh, fallback về index.html cho SPA ---
FROM nginx:1.27-alpine AS prod
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
  CMD wget -qO- http://127.0.0.1/ || exit 1
