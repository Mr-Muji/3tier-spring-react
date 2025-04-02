# ------------------ 빌드 단계 (Build Stage) ------------------
# 여기서는 JDK와 Maven이 포함된 이미지를 사용하여 애플리케이션 코드를 빌드합니다.
FROM maven:3.9-eclipse-temurin-21 AS build
# 작업 디렉토리 설정
WORKDIR /app
# Maven 의존성 캐싱을 위해 먼저 pom.xml만 복사
COPY pom.xml .
RUN mvn dependency:go-offline
# 소스 코드 전체 복사 (pom.xml까지 그대로 복사해도 문제 없습니다.)
COPY . .
# Maven으로 패키징 (테스트 스킵 옵션 사용 가능)
RUN mvn package -DskipTests

# ------------------ 런타임 단계 (Runtime Stage) ------------------
# Eclipse Temurin 21 (JRE) 이미지를 사용하여 실행
FROM eclipse-temurin:21-jre
# 작업 디렉토리 설정
WORKDIR /app
# 빌드 단계에서 생성한 JAR 파일을 복사
COPY --from=build /app/target/*.jar app.jar
# 컨테이너가 호출될 때 열어줄 포트 (예: 8080)
EXPOSE 8080
# 컨테이너 시작 시 실행할 명령어
ENTRYPOINT ["java", "-jar", "app.jar"] 