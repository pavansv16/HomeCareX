# Stage 1: Build the backend application
FROM node:20-alpine AS build

WORKDIR /app

# Copy package configuration
COPY package.json ./

# Install dependencies
RUN npm install

# Copy TypeScript configuration
COPY tsconfig.json ./

# Copy backend source code
COPY src ./src

# Compile TypeScript into JavaScript
RUN npm run build


# Stage 2: Run the backend application
FROM node:20-alpine AS runtime

WORKDIR /app

ENV NODE_ENV=production

# Copy package configuration
COPY package.json ./

# Install only production dependencies
RUN npm install --omit=dev && npm cache clean --force

# Copy compiled application
COPY --from=build /app/dist ./dist

# Create storage directories
RUN mkdir -p storage/job-media storage/job-proof storage/kyc \
    && chown -R node:node /app

# Run as non-root user
USER node

# Backend port
EXPOSE 5000

# Start backend server
CMD ["node", "dist/server.js"]
