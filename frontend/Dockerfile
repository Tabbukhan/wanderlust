# Stage 1: Build the frontend
FROM node:20-alpine AS build

WORKDIR /app

# Copy package.json and package-lock.json first (for caching)
COPY package*.json ./

# Install dependencies without running audit/fund
RUN npm install --no-audit --no-fund

# Copy the rest of the frontend code
COPY . .

# Build the frontend
RUN npm run build

# Stage 2: Serve with Nginx
FROM nginx:alpine

# Remove default Nginx static files
RUN rm -rf /usr/share/nginx/html/*

# Copy build output from previous stage
# If Vite outputs to 'dist' (default) or 'build' (React), copy whichever exists
COPY --from=build /app/dist /usr/share/nginx/html
COPY --from=build /app/build /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
