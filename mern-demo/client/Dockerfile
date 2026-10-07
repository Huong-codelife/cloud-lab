# Stage 1: Build ứng dụng React với Node.js
FROM node:20-alpine AS build

WORKDIR /app

# Sao chép package.json và cài đặt dependencies
COPY package*.json ./
RUN npm install

# Sao chép source code và build
COPY . .
RUN npm run build

# Stage 2: Chạy với Nginx (nhẹ, nhanh, production-ready)
FROM nginx:alpine

# Xóa config mặc định của Nginx
RUN rm /etc/nginx/conf.d/default.conf

# Sao chép template config Nginx (dùng biến $PORT)
COPY nginx.conf.template /etc/nginx/templates/nginx.conf.template

# Sao chép file build từ stage 1 vào Nginx
COPY --from=build /app/dist /usr/share/nginx/html

# Port mặc định (Render sẽ override bằng biến PORT)
ENV PORT=3000
EXPOSE 3000

# Dùng envsubst để thay $PORT trong template, rồi khởi chạy Nginx
CMD ["/bin/sh", "-c", "envsubst '${PORT}' < /etc/nginx/templates/nginx.conf.template > /etc/nginx/conf.d/default.conf && nginx -g 'daemon off;'"]