# Stage 1: Build
# ================================
FROM node:22-alpine3.22 AS build

WORKDIR /app

# Upgrade Alpine packages to latest available security patches
RUN apk update && apk upgrade

# Copy dependency files first for Docker layer caching
COPY package*.json ./

# Install exact dependencies from package-lock.json
RUN npm ci

# Copy application source
COPY . .

# ================================
# Stage 2: Runtime
# ================================
FROM node:22-alpine3.22 AS runtime

WORKDIR /app

# Upgrade Alpine packages
RUN apk update && apk upgrade

# Run application as non-root user
USER node

# Copy only required application files
COPY --chown=node:node --from=build /app/package*.json ./
COPY --chown=node:node --from=build /app/node_modules ./node_modules
COPY --chown=node:node --from=build /app ./

EXPOSE 3000

CMD ["npm", "start"]