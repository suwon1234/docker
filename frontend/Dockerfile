# 스테이지 1 -> 빌드, node 이미지를 사용
FROM node:22 AS builder

# 작업 디렉토리
WORKDIR /app

# node_modules 제외 -> .dockerignore 파일 체크
COPY . .

# 의존성 설치
RUN npm install || (echo "npm install failed" && tail -n 50 /app/npm-debug.log && exit 1)

# 빌드 -> /app/build
RUN npm run build || (echo "npm run build failed" && tail -n 50 /app/npm-debug.log && exit 1)

# 스테이지 2 -> 서비스, nginx 이미지를 사용, 빌드 결과를 넣어서 구동
FROM nginx:alpine

# 빌드된 결과물 카피(html, css, js, 리소스(이미지 등))
COPY --from=builder /app/build /usr/share/nginx/html

# 포트
EXPOSE 80

# 컨테이너 가동
CMD ["nginx", "-g", "daemon off;"]
