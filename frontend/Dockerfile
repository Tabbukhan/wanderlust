# Stage 1: Build the frontend
FROM node:20-alpine AS build

WORKDIR /app

# Copy package.json and package-lock.json first (caching)
COPY package*.json ./

# Install dependencies (skip audit/funding for CI speed)
RUN npm install --no-audit --no-fund

# Copy all frontend code
COPY . .

# Build frontend using Vite (handles TS + aliases)
RUN npm run build

# Stage 2: Serve with Nginx
FROM nginx:alpine

# Remove default Nginx static files
RUN rm -rf /usr/share/nginx/html/*

# Copy build output (Vite outputs to dist/)
COPY --from=build /app/dist /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
