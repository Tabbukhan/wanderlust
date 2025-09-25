# Stage 1: Build the frontend
FROM node:20-alpine AS build

WORKDIR /app

# Copy package.json and package-lock.json first (better caching)
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the frontend code
COPY . .

# Build the app (will create /app/build for React or /app/dist for Vue/Angular)
RUN npm run build

# Stage 2: Serve with Nginx
FROM nginx:alpine

# Copy built frontend files from build stage to nginx web root
COPY --from=build /app/build /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
