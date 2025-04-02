# ------------------ 빌드 단계 (Build Stage) ------------------
# Node.js와 npm이 포함된 이미지를 사용해서 애플리케이션 코드를 빌드합니다.
FROM node:18-alpine AS build
# 작업 디렉토리 설정
WORKDIR /app
# package.json, package-lock.json 복사
COPY package*.json ./
# 의존성 설치
RUN npm install
# 소스 코드 전체 복사
COPY . .
# 빌드 명령어 실행 (React의 경우 npm run build)
RUN npm run build

# ------------------ 런타임 단계 (Runtime Stage) ------------------
# nginx 이미지를 사용해 정적 웹 파일 제공
FROM nginx:alpine
# Nginx 설정파일 복사 (커스텀이 필요한 경우만)
# COPY nginx.conf /etc/nginx/conf.d/default.conf

# 빌드 단계에서 생성된 build 폴더를 nginx 기본 경로 (/usr/share/nginx/html)에 복사
COPY --from=build /app/build /usr/share/nginx/html
# 컨테이너 포트 공개 (기본 80)
EXPOSE 80
# Nginx 실행
CMD ["nginx", "-g", "daemon off;"] 