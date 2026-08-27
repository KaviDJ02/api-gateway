# api-gateway — Phase 3

Minimal Spring Cloud Gateway configured to use Eureka for service discovery.

Files added:

- `pom.xml` — Maven build
- `src/main/java/.../ApiGatewayApplication.java` — main class
- `src/main/resources/application.yml` — gateway + eureka configuration

Build and run:

```bash
cd blood-donation-platform/api-gateway
mvn -DskipTests package
java -jar target/api-gateway-0.0.1-SNAPSHOT.jar
```

Pre-conditions: Eureka server must be running on `http://localhost:8761/`.

Smoke tests (after gateway is running):

```bash
# gateway health
curl http://localhost:8080/actuator/health

# list discovered routes (gateway will create routes for services registered in Eureka)
curl http://localhost:8080/actuator/routes

# If a service `donor-service` is registered in Eureka, requests can be proxied:
curl -i http://localhost:8080/donor-service/actuator/health
```
# api-gateway

Spring Cloud Gateway on port `8080`. It uses Eureka service discovery and exposes the business services through stable prefixes.

| Gateway path | Service |
|---|---|
| `/donor-service/**` | donor-service |
| `/blood-request-service/**` | blood-request-service |
| `/donation-service/**` | donation-service |

```bash
mvn -DskipTests package
java -jar target/api-gateway-0.0.1-SNAPSHOT.jar
```

Set `EUREKA_DEFAULT_ZONE` when Eureka is not running on `localhost:8761`. Verify the gateway with:

```bash
curl http://localhost:8080/actuator/health
curl http://localhost:8080/actuator/routes
```
