# Build stage
# Image jdk pour compiler
FROM eclipse-temurin:21-jdk-alpine AS myapp-build
ENV MYAPP_HOME=/opt/myapp
# Tous les fichiers seront copiés et les commandes exécutées sur ce répertoire
WORKDIR $MYAPP_HOME

# installe Maven
RUN apk add --no-cache maven

COPY pom.xml .
COPY src ./src
# compiler le projet et générer le .jar et -DskipTests pour éviter de faire les tests pendant le build
RUN mvn package -DskipTests

# Run stage
FROM eclipse-temurin:21-jre-alpine
ENV MYAPP_HOME=/opt/myapp
WORKDIR $MYAPP_HOME
# Copier le .jar généré précédemment et le copier dans le conteneur myapp.jar
COPY --from=myapp-build $MYAPP_HOME/target/*.jar $MYAPP_HOME/myapp.jar

# Démarrer le projet lorsque le conteneur est lancé
ENTRYPOINT ["java", "-jar", "myapp.jar"]
