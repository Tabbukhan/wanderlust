# Stage 1: Build
FROM node:20-alpine AS build

# Optional: build args to override TS module
ARG TS_MODULE=Node16

WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install --no-audit --no-fund

# Copy all source code
COPY . .

# Build the project with overridden module setting
RUN npx tsc -p tsconfig.prod.json --module $TS_MODULE \
    && npm run vite build

# Stage 2: Production image
FROM nginx:alpine

# Copy built files from build stage
COPY --from=build /app/dist /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start NGINX
CMD ["nginx", "-g", "daemon off;"]
