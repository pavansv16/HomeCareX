# Stage 1: Build React application
FROM node:20-alpine AS build

WORKDIR /app

# Copy package configuration
COPY package.json ./

# Install frontend dependencies
RUN npm install

# Copy frontend source code
COPY . .

# Frontend API configuration
ARG VITE_API_BASE_URL=/api
ARG VITE_SOCKET_URL=

ENV VITE_API_BASE_URL=$VITE_API_BASE_URL
ENV VITE_SOCKET_URL=$VITE_SOCKET_URL

# Build React application
RUN npm run build


# Stage 2: Serve frontend using Nginx
FROM nginx:1.27-alpine

# Copy Nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy production frontend files
COPY --from=build /app/dist /usr/share/nginx/html

# Frontend container port
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
