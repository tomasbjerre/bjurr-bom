# Bjurr BOM

[![Maven Central](https://maven-badges.herokuapp.com/maven-central/se.bjurr.bom/bjurr-bom/badge.svg)](https://maven-badges.herokuapp.com/maven-central/se.bjurr.bom/bjurr-bom)

Common Maven settings for my projects. Example usage [here](https://github.com/tomasbjerre/assertj-snapshot).

## Get Maven

You may want to use the [Maven wrapper](https://maven.apache.org/wrapper):

```sh
mvn wrapper:wrapper -Dmaven=3.8.1
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

## Static code analysis

Spotbugs is bundled. Violations can be ignored with `@SuppressFBWarnings`. Also [Violations Maven Plugin](https://github.com/tomasbjerre/violations-maven-plugin) is used to present, and fail build based on, violations.

```
mvn verify 
```

May fail the build with:

```
[ERROR] org/assertj/snapshot/internal/utils/FileUtilsImpl.java
|          |               |          |      |                              |
| Reporter | Rule          | Severity | Line | Message                      |
|          |               |          |      |                              |
+----------+---------------+----------+------+------------------------------+
|          |               |          |      |                              |
| Spotbugs | MS_EXPOSE_REP | INFO     | 30   | Public  static  method   may |
|          |               |          |      | expose              internal |
|          |               |          |      | representation by  returning |
|          |               |          |      | array <p>  A  public  static |
|          |               |          |      | method returns  a  reference |
|          |               |          |      | to an array that is part  of |
|          |               |          |      | the  static  state  of   the |
|          |               |          |      | class. Any code  that  calls |
|          |               |          |      | this   method   can   freely |
|          |               |          |      | modify the underlying array. |
|          |               |          |      | One fix is to return a  copy |
|          |               |          |      | of the array.</p>            |
|          |               |          |      |                              |
+----------+---------------+----------+------+------------------------------+
Summary of org/assertj/snapshot/internal/utils/FileUtilsImpl.java
|          |      |      |       |       |
| Reporter | INFO | WARN | ERROR | Total |
|          |      |      |       |       |
+----------+------+------+-------+-------+
|          |      |      |       |       |
| Spotbugs | 1    | 0    | 0     | 1     |
|          |      |      |       |       |
+----------+------+------+-------+-------+
|          |      |      |       |       |
|          | 1    | 0    | 0     | 1     |
|          |      |      |       |       |
+----------+------+------+-------+-------+


Summary
|          |      |      |       |       |
| Reporter | INFO | WARN | ERROR | Total |
|          |      |      |       |       |
+----------+------+------+-------+-------+
|          |      |      |       |       |
| Spotbugs | 1    | 0    | 0     | 1     |
|          |      |      |       |       |
+----------+------+------+-------+-------+
|          |      |      |       |       |
|          | 1    | 0    | 0     | 1     |
|          |      |      |       |       |
+----------+------+------+-------+-------+

[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  12.007 s
[INFO] Finished at: 2023-06-26T10:40:41+02:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal se.bjurr.violations:violations-maven-plugin:1.50.4:violations (default) on project assertj-snapshot: Too many violations found, max is 0 but found 1. You can adjust this with the 'maxViolations' configuration parameter. -> [Help 1]
```

## Releasing

Release and sign with:

```sh
./mvnw se.bjurr.gitchangelog:git-changelog-maven-plugin:semantic-version \
 release:prepare release:perform -B \
 se.bjurr.gitchangelog:git-changelog-maven-plugin:git-changelog \
  && git commit -a -m "chore: updating changelog" \
  && git push \
  || git clean -f
```

To get signing working, there is a bug, you may need to do this first:

```sh
gpg -o /tmp/dummy --sign .gitignore
```
