<h1 align="center">Deepmatrix OpenAPI Generator</h1>

---

### Modify YAML

To mark a particular property as ID in a schema add annotations in the form of list under x-setter-annotation

```yaml
x-setter-annotation: 
            - Id
            - GeneratedValue(generator="UUID")
            - GenericGenerator(name="UUID", strategy="org.hibernate.id.UUIDGenerator")
            - Column(name="orgId", updatable=false, nullable=false)
```

---

### Build Projects

To build from source, you need the following installed and available in your `$PATH:`

* [Java 8](https://www.oracle.com/technetwork/java/index.html)

* [Apache Maven 3.3.4 or greater](https://maven.apache.org/)

After cloning the project, you can build it from source with this command:
```sh
mvn clean install -DskipTests
```
It will create a jar file in a folder
`openapi-generator\modules\openapi-generator-cli\target\openapi-generator-cli.jar`

---

### Generating apis and models

```sh
java -jar C:\deepmatrix\codegen\openapi-generator\modules\openapi-generator-cli\target\openapi-generator-cli.jar generate -i C:\deepmatrix\data-oas\data-service.yaml -g  jaxrs-spec --library=quarkus --additional-properties=interfaceOnly=true,useSwaggerAnnotations=false,dateLibrary=java8,apiPackage=io.deepmatrix.service.api,modelPackage=io.deepmatrix.service.model  -o C:\deepmatrix\codegen\codegeneg1
```

---

### Jakarta EE vs Java EE namespace

The `jaxrs-spec` generator emits **`jakarta.*`** imports by default (Hibernate 6 /
Jakarta Persistence 3, Quarkus 3, Jakarta Bean Validation 3, JAX-RS 3). This affects:

| Generated code | Package |
| --- | --- |
| JPA annotations (`@Id`, `@Column`, `@Transient`, `@MappedSuperclass`, `@GeneratedValue`) | `jakarta.persistence.*` |
| Bean validation (`@NotNull`, `@Size`, `@Valid`, ...) | `jakarta.validation.*` |
| JAX-RS (`@Path`, `@GET`, `Response`, ...) | `jakarta.ws.rs.*` |
| `@Generated` | `jakarta.annotation.Generated` |

To go back to the legacy `javax.*` namespace (Hibernate 5 / Quarkus 2), add
`useJakartaEe=false` to `--additional-properties`:

```sh
--additional-properties=interfaceOnly=true,useSwaggerAnnotations=false,dateLibrary=java8,useJakartaEe=false,apiPackage=io.deepmatrix.service.api,modelPackage=io.deepmatrix.service.model
```

Note: `org.hibernate.annotations.GenericGenerator` stays on `org.hibernate.*` in both
modes — only the Java/Jakarta EE specification packages move.
