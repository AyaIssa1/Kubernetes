# Utiliser une image de base pour le build Maven
FROM maven:3.9.11-eclipse-temurin-17 AS build

# Répertoire de travail pour le build
WORKDIR /build

# Copier le fichier pom.xml pour résoudre les dépendances Maven
COPY src/api/pom.xml .
RUN ["mvn", "dependency:resolve", "-U"]

# Copier le code source et construire l'application
COPY src/api/src ./src
RUN ["mvn", "clean", "package", "-DskipTests"]

# Image finale pour l'exécution de l'application
FROM eclipse-temurin:17-jre-alpine

# Répertoire de travail dans l'image finale
WORKDIR /app

# Exposer le port pour l'application
EXPOSE 8080

# Copier le fichier JAR depuis le build Maven
COPY --from=build /build/target/*.jar /app/*.jar

# Exécution de l'application Java
ENTRYPOINT ["java", "-jar", "/app/*.jar"]
