# OpenAPI Generator Templates For JBoss EAP

## Usage

### CLI
```bash
openapi-generator generate -g jaxrs-resteasy-eap -i openapi.yml -t path/to/these/templates -o output/project
```

### Maven
```xml
<build>
    <plugins>
        <!-- Add the path to the generated sources to maven -->
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>build-helper-maven-plugin</artifactId>
            <version>3.1.0</version>
            <executions>
                <execution>
                    <phase>generate-sources</phase>
                    <goals>
                        <goal>add-source</goal>
                    </goals>
                    <configuration>
                        <sources>
                            <source>src/gen/java</source>
                        </sources>
                    </configuration>
                </execution>
            </executions>
        </plugin>
        <!-- Clone the customized template which has support for JPA annotations -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-scm-plugin</artifactId>
            <version>1.11.2</version>
            <executions>
                <execution>
                    <phase>initialize</phase>
                    <goals>
                        <goal>checkout</goal>
                    </goals>
                    <configuration>
                        <connectionUrl>scm:git:https://github.com/redhat-appdev-practice/openapi-jboss-jpa-templates.git</connectionUrl>
                        <checkoutDirectory>templates</checkoutDirectory>
                        <connectionType>connection</connectionType>
                    </configuration>
                </execution>
            </executions>
        </plugin>
        <!-- Run the OpenAPI Generator -->
        <plugin>
            <groupId>org.openapitools</groupId>
            <artifactId>openapi-generator-maven-plugin</artifactId>
            <version>4.3.1</version>
            <executions>
                <execution>
                    <phase>generate-sources</phase>
                    <goals>
                        <goal>generate</goal>
                    </goals>
                    <configuration>
                        <generatorName>jaxrs-resteasy-eap</generatorName>
                        <inputSpec>${project.basedir}/openapi.yaml</inputSpec>
                        <templateDirectory>${project.basedir}/templates</templateDirectory>
                        <output>${project.basedir}</output>
                        <configOptions>
                            <serializableModel>true</serializableModel>
                            <artifactId>${project.artifactId}</artifactId>
                            <groupId>${project.groupId}</groupId>
                            <version>${project.version}</version>
                            <dateLibrary>java8-localdatetime</dateLibrary>
                            <sourceFolder>src/gen/java</sourceFolder>
                            <basePackage>com.redhat</basePackage>
                            <invokerPackage>com.redhat</invokerPackage>
                            <configPackage>com.redhat.config</configPackage>
                            <modelPackage>com.redhat.models</modelPackage>
                            <apiPackage>com.redhat.api</apiPackage>
                        </configOptions>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```


### Gradle

```groovy
buildscript {
  repositories {
    mavenLocal()
    maven { url "https://repo1.maven.org/maven2" }
  }
  dependencies {
    classpath "org.openapitools:openapi-generator-gradle-plugin:5.0.0"
  }
}
apply plugin: 'org.openapi.generator'

openApiGenerate {
    generatorName = "jaxrs-resteasy-eap"
    inputSpec = "$rootDir/openapi.yaml".toString()
    outputDir = "$buildDir/generated".toString()
    apiPackage = "org.openapi.example.api"
    invokerPackage = "org.openapi.example.invoker"
    modelPackage = "org.openapi.example.model"
    templateDirectory = "$rootDir/templates"
    configOptions = [
        dateLibrary: "java8"
    ]
}
```