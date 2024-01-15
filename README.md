# SPDX

## Gradle

Add

```
spdxSbom {
    targets {
        create("application") {
        }
    }
}
```

to build file. Then run `gradle spdxSbom` to generate the JSON. Looks like [this](release.spdx.json).

To automatically include it in the JAR file:

```
tasks.named("bootJar") {
    dependsOn("spdxSbom")
    from("build/spdx") {
        include "*.spdx.json" into "BOOT-INF/classes"
    }
}
```

## Maven

Add

```
<plugin>
    <groupId>org.spdx</groupId>
    <artifactId>spdx-maven-plugin</artifactId>
    <version>0.7.2</version>
    <executions>
        <execution>
            <id>build-spdx</id>
            <goals>
                <goal>createSPDX</goal>
            </goals>
            <phase>compile</phase>
        </execution>
    </executions>
    <configuration>
        <spdxFile>${project.build.outputDirectory}/application.spdx.json</spdxFile>
    </configuration>
</plugin>
```

It will be automatically included in the Boot JAR as it runs in the compile phase and the file is written in the classes output directory.

## Mime Type

https://www.iana.org/assignments/media-types/application/spdx+json

# CycloneDX

## Gradle

Add

```
cyclonedxBom {
    includeLicenseText = false
    destination = file("build/cyclonedx")
    outputFormat = "json"
}
```

then run `gradle cyclonedxBom`.

To automatically include it in the JAR file:

```
tasks.named("bootJar") {
    dependsOn("cyclonedxBom")
    from("build/cyclonedx") {
        include "bom.json" into "BOOT-INF/classes"
    }
}
```

## Maven

```
<plugin>
    <groupId>org.cyclonedx</groupId>
    <artifactId>cyclonedx-maven-plugin</artifactId>
    <version>2.7.10</version>
    <executions>
        <execution>
            <goals>
                <goal>makeAggregateBom</goal>
            </goals>
            <phase>compile</phase>
        </execution>
    </executions>
    <configuration>
        <outputDirectory>${project.build.outputDirectory}</outputDirectory>
        <outputFormat>json</outputFormat>
    </configuration>
</plugin>
```

It will be automatically included in the Boot JAR as it runs in the compile phase and the file is written in the classes output directory.

## Mime type

https://cyclonedx.org/specification/overview/#registered-media-types
