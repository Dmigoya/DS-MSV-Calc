# Etapa 1: Construcción de la aplicación
FROM maven:3.9-eclipse-temurin-17 AS build
WORKDIR /app

# Copiar archivos de configuración de Maven
COPY pom.xml .
# Descargar dependencias (caché de capas)
RUN mvn dependency:go-offline -B

# Copiar el código fuente
COPY src ./src

# Construir la aplicación
RUN mvn clean package -DskipTests

# Etapa 2: Imagen de ejecución (liviana y multi-arquitectura)
FROM eclipse-temurin:17-jre
WORKDIR /app

# Crear usuario no root para seguridad
RUN groupadd -r spring && useradd -r -g spring spring
USER spring:spring

# Copiar el JAR desde la etapa de construcción
COPY --from=build /app/target/*.jar app.jar

# Exponer el puerto (ajusta según tu aplicación)
EXPOSE 8080

# Punto de entrada
ENTRYPOINT ["java", "-jar", "app.jar"]

