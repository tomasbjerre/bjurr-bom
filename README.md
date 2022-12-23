# Bjurr BOM

[![Maven Central](https://maven-badges.herokuapp.com/maven-central/se.bjurr.bom/bjurr-bom/badge.svg)](https://maven-badges.herokuapp.com/maven-central/se.bjurr.bom/bjurr-bom)

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
                <groupId>se.bjurr.bom</groupId>
                <artifactId>bjurr-bom</artifactId>
                <version>X</version>
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
        <groupId>se.bjurr.bom</groupId>
        <artifactId>bjurr-bom</artifactId>
        <version>X</version>
    </parent>
    <!--- ... //-->
</project>
```


## Releasing

Release and sign with:

```sh
./mvnw se.bjurr.gitchangelog:git-changelog-maven-plugin:semantic-version \
 release:prepare \
 se.bjurr.gitchangelog:git-changelog-maven-plugin:git-changelog \
 release:perform -B \
  && git commit -a -m "chore: updating changelog" \
  && git push \
  || git clean -f
```

To get signing working, there is a bug, you may need to do this first:

```sh
gpg -o /tmp/dummy --sign .gitignore
```