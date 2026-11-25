FROM node:20-alpine AS build
WORKDIR /app

COPY package.json ./
COPY package-lock.json* ./
RUN npm install

COPY . .

# Baked into the JS bundle at build time. /api is proxied by nginx
# (see nginx.conf) to the backend, so this works in Docker and locally.
ARG VITE_API_BASE_URL=/api
# Empty = socket.io-client uses the same origin (nginx proxies it).
ARG VITE_SOCKET_URL=
ENV VITE_API_BASE_URL=$VITE_API_BASE_URL
ENV VITE_SOCKET_URL=$VITE_SOCKET_URL

RUN npm run build

# ---- serve stage ----
FROM nginx:1.27-alpine
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
