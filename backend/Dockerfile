FROM node:20-alpine AS build
WORKDIR /app

COPY package.json ./
# package-lock.json is not in the repo for backend; copy it only if present
COPY package-lock.json* ./
RUN npm install

COPY tsconfig.json ./
COPY src ./src
RUN npm run build

# ---- runtime stage: production deps only ----
FROM node:20-alpine AS runtime
WORKDIR /app
ENV NODE_ENV=production

COPY package.json ./
COPY package-lock.json* ./
RUN npm install --omit=dev && npm cache clean --force

COPY --from=build /app/dist ./dist

# Upload dirs used by the backend (backend/storage/ in the repo)
RUN mkdir -p storage/job-media storage/job-proof storage/kyc \
    && chown -R node:node /app
USER node

EXPOSE 5000
CMD ["node", "dist/server.js"]
