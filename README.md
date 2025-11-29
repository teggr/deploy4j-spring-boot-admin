# deploy4j-spring-boot-admin

A Spring Boot Admin Server distribution for the [deploy4j](https://github.com/teggr/deploy4j) project.

Spring Boot Admin provides a web UI for monitoring and managing Spring Boot applications. This project packages it as a standalone application that can be deployed using deploy4j.

## Prerequisites

- Java 17 or higher
- Maven 3.9+ (or use the included Maven wrapper)
- Docker (for building container images)

## Building

### Build the application

```bash
./mvnw clean package
```

This will compile the application and create an executable JAR file in the `target/` directory.

### Run locally

```bash
./mvnw spring-boot:run
```

Or run the JAR directly:

```bash
java -jar target/deploy4j-spring-boot-admin-0.0.1-SNAPSHOT.jar
```

The Spring Boot Admin dashboard will be available at http://localhost:8080

### Run tests

```bash
./mvnw test
```

## Building Docker Image

This project uses the Spring Boot Maven plugin to build Docker images using [Cloud Native Buildpacks](https://docs.spring.io/spring-boot/maven-plugin/build-image.html).

### Build the Docker image

```bash
./mvnw spring-boot:build-image
```

This will create a Docker image named `teggr/deploy4j-spring-boot-admin:0.0.1-SNAPSHOT`.

To customize the Docker image prefix (e.g., for your own registry):

```bash
./mvnw spring-boot:build-image -Ddocker.image.prefix=myregistry
```

### Run the Docker container

```bash
docker run -p 8080:8080 teggr/deploy4j-spring-boot-admin:0.0.1-SNAPSHOT
```

## Release

### Push to Docker Hub

To push the Docker image to Docker Hub, you need to be logged in:

```bash
docker login
```

Then push the image:

```bash
docker push teggr/deploy4j-spring-boot-admin:0.0.1-SNAPSHOT
```

### Deploying with deploy4j

Once the Docker image is published, you can deploy it using [deploy4j](https://teggr.github.io/deploy4j):

```bash
deploy4j setup --version 0.0.1-SNAPSHOT
```

For subsequent deployments:

```bash
deploy4j deploy --version 0.0.1-SNAPSHOT
```

See the [deploy4j documentation](https://teggr.github.io/deploy4j) for more details on deployment configuration.

## Configuration

The Spring Boot Admin server can be configured via environment variables or `application.properties`. Common configuration options:

| Property | Default | Description |
|----------|---------|-------------|
| `server.port` | `8080` | HTTP port for the server |
| `spring.application.name` | `deploy4j-spring-boot-admin` | Application name |

## Registering Client Applications

Client applications can register with this Spring Boot Admin server by adding the following dependency and configuration:

**Add dependency (Maven):**
```xml
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-starter-client</artifactId>
    <version>3.4.7</version>
</dependency>
```

**Configure the client (application.properties):**
```properties
spring.boot.admin.client.url=http://localhost:8080
management.endpoints.web.exposure.include=*
```

See the [Spring Boot Admin documentation](https://docs.spring-boot-admin.com/) for more configuration options.

## Related Projects

- [deploy4j](https://github.com/teggr/deploy4j) - Deploy Java web applications to self-hosted VMs using Docker
- [deploy4j-demo](https://github.com/teggr/deploy4j-demo) - Example Spring Boot application for deploy4j

## License

MIT License - see [LICENSE](LICENSE) for details.
