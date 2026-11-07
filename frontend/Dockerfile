# Stage 1: Build React application
FROM node:18-alpine AS build

WORKDIR /app

COPY frontend/package*.json ./

RUN npm ci --legacy-peer-deps

COPY frontend/ ./

RUN npm run build


# Stage 2: Serve React using Nginx
FROM nginx:stable-alpine

COPY --from=build /app/build /usr/share/nginx/html

# Copy product images
COPY static/images/ /usr/share/nginx/html/images/

COPY frontend/nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]