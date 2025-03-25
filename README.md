# **API-First Development with OpenAPI & Spring Boot**  
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.2.0-green)  
![OpenAPI](https://img.shields.io/badge/OpenAPI-3.0.3-blue)  

A step-by-step guide to building an API-First application using **OpenAPI 3.0** and **Spring Boot 3**.  

---

## **📌 Table of Contents**  
1. [Prerequisites](#-prerequisites)  
2. [Step 1: Create a Spring Boot Project](#-step-1-create-a-spring-boot-project)  
3. [Step 2: Define Your API (OpenAPI Spec)](#-step-2-define-your-api-openapi-spec)  
4. [Step 3: Generate Spring Boot Code](#-step-3-generate-spring-boot-code)  
5. [Step 4: Implement the API](#-step-4-implement-the-api)  
6. [Step 5: Run & Test](#-step-5-run--test)  
7. [Best Practices](#-best-practices)  
8. [Next Steps](#-next-steps)  

---

## **✅ Prerequisites**  
- Java 17+  
- Maven 3.8+  
- IDE (IntelliJ, VSCode, etc.)  

---

## **🚀 Step 1: Create a Spring Boot Project**  
1. **Option 1**: Use [Spring Initializr](https://start.spring.io/) with:  
   - **Spring Web**  
   - **Lombok** (optional)  

2. **Option 2**: Use this `pom.xml`:  
   ```xml
   <dependencies>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springdoc</groupId>
           <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
           <version>2.3.0</version>
       </dependency>
   </dependencies>
   ```

---

## **📝 Step 2: Define Your API (OpenAPI Spec)**  
Create `src/main/resources/api.yaml`:  
```yaml
openapi: 3.0.3
info:
  title: User API
  version: 1.0.0
paths:
  /users:
    get:
      summary: Get all users
      responses:
        200:
          description: List of users
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/User"
components:
  schemas:
    User:
      type: object
      properties:
        id: { type: integer }
        name: { type: string }
```

---

## **⚙️ Step 3: Generate Spring Boot Code**  
1. Add the **OpenAPI Generator** plugin to `pom.xml`:  
   ```xml
   <plugin>
       <groupId>org.openapitools</groupId>
       <artifactId>openapi-generator-maven-plugin</artifactId>
       <version>7.3.0</version>
       <executions>
           <execution>
               <goals>
                   <goal>generate</goal>
               </goals>
               <configuration>
                   <inputSpec>${project.basedir}/src/main/resources/api.yaml</inputSpec>
                   <generatorName>spring</generatorName>
                   <apiPackage>com.example.api</apiPackage>
                   <modelPackage>com.example.model</modelPackage>
                   <configOptions>
                       <interfaceOnly>true</interfaceOnly>
                       <useSpringBoot3>true</useSpringBoot3>
                   </configOptions>
               </configuration>
           </execution>
       </executions>
   </plugin>
   ```
2. Generate code:  
   ```bash
   mvn clean compile
   ```

---

## **👨‍💻 Step 4: Implement the API**  
Create `UserController.java`:  
```java
@RestController
public class UserController implements UsersApi {
    @Override
    public ResponseEntity<List<User>> getUsers() {
        return ResponseEntity.ok(List.of(
            new User().id(1).name("Alice"),
            new User().id(2).name("Bob")
        ));
    }
}
```

---

## **🔍 Step 5: Run & Test**  
1. Start the app:  
   ```bash
   mvn spring-boot:run
   ```
2. Access **Swagger UI**:  
   [http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html)  
3. Test with `curl`:  
   ```bash
   curl http://localhost:8080/users
   ```

---

## **🏆 Best Practices**  
✔ **Keep generated code in `target/`** (don’t commit it).  
✔ **Use `interfaceOnly=true`** to avoid overwriting your logic.  
✔ **Validate API compliance** with automated tests.  

---

## **📈 Next Steps**  
- Add **database integration** (JPA/Hibernate).  
- Secure the API with **Spring Security**.  
- Deploy with **Docker**.  


