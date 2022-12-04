# Bjurr BOM

Some Maven stuff for my projects.

## Get Maven

You may want to use the [Maven wrapper](https://maven.apache.org/wrapper):

```sh
mvn wrapper:wrapper
```

## Update dependencies

```sh
./mvnw versions:update-properties
```

## Import the BOM

You can add the BOM as a dependency:

```xml
<project>
    <!--- ... //-->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>reflectoring</groupId>
                <artifactId>reflectoring-bom</artifactId>
                <version>1.0</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
```

Or as a parent:

```xml
<project>
    <parent>
        <groupId>reflectoring</groupId>
        <artifactId>reflectoring-bom</artifactId>
        <version>1.0</version>
    </parent>
    <!--- ... //-->
</project>
```


## Releasing

Release and sign with:

```sh
./mvnw \
  release:prepare \
  release:perform \
  -B
```