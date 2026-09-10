# ================================
# Stage 1: Build
# ================================
FROM node:22-alpine3.22 AS build

WORKDIR /app

# Upgrade Alpine packages
RUN apk update && apk upgrade

# Copy dependency files first
COPY package*.json ./

# Install exact dependencies
RUN npm ci

# Copy application source
COPY . .

# Vite build-time variable
ARG VITE_API_URL
ENV VITE_API_URL=${VITE_API_URL}

# Build production frontend
RUN npm run build


# ================================
# Stage 2: Production Runtime
# ================================
FROM nginx:alpine AS runtime

# Upgrade Alpine packages
RUN apk update && apk upgrade

# Nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy production frontend
COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]