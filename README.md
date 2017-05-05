
# Static Code Analysis

## ABOUT THIS PROJECT

---

This project is setup to consolidate the code check rules into a single place. This project wraps rules for -

* FindBugs
* CheckStyle
* PMD

## BUILDING THIS PROJECT

This is a simple maven jar project and can be build and deployed to code artifact repository [nexus, artifactory etc..]

For local builds - 
`mvn clean install`

For CI builds -
`mvn clean deploy`

## CONFIGURING THE PROJECT TO USE THESE RULESETS
Add following to the project pom or merge with existing plugins tag - 

```
<plugins>
   <!-- Findbug plugin -->
   <plugin>
      <groupId>org.codehaus.mojo</groupId>
      <artifactId>findbugs-maven-plugin</artifactId>
      <version>3.0.3</version>
      <dependencies>
         <dependency>
            <groupId>net.ameesh</groupId>
            <artifactId>static-code-analysis</artifactId>
            <version>1.0.0</version>
         </dependency>
      </dependencies>
      <executions>
         <execution>
            <id>findbug</id>
            <phase>verify</phase>
            <goals>
               <goal>check</goal>
            </goals>
         </execution>
      </executions>
      <configuration>
         <findbugsXmlOutputDirectory>${project.build.directory}/findbugs</findbugsXmlOutputDirectory>
         <failOnError>true</failOnError>
         <skip>${skipFindBug}</skip>
         <xmlOutput>true</xmlOutput>
         <effort>Max</effort>
         <threshold>Low</threshold>
         <excludeFilterFile>findbugs-exclude.xml</excludeFilterFile>
      </configuration>
   </plugin>
   <!-- PMD check  -->
   <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-pmd-plugin</artifactId>
      <version>3.6</version>
      <dependencies>
         <dependency>
            <groupId>net.ameesh</groupId>
            <artifactId>static-code-analysis</artifactId>
            <version>1.0.0</version>
         </dependency>
      </dependencies>
      <executions>
         <execution>
            <goals>
               <goal>check</goal>
               <goal>cpd-check</goal>
            </goals>
            <configuration>
               <skip>${skipPMD}</skip>
            </configuration>
         </execution>
      </executions>
      <configuration>
         <linkXref>true</linkXref>
         <sourceEncoding>${project.build.sourceEncoding}</sourceEncoding>
         <minimumTokens>100</minimumTokens>
         <targetJdk>${version.jdk}</targetJdk>
         <rulesets>
            <ruleset>pmd-rules.xml</ruleset>
         </rulesets>
      </configuration>
   </plugin>
   <!-- Checkstyle -->
   <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-checkstyle-plugin</artifactId>
      <version>2.17</version>
      <dependencies>
         <dependency>
            <groupId>net.ameesh</groupId>
            <artifactId>static-code-analysis</artifactId>
            <version>1.0.0</version>
         </dependency>
      </dependencies>
      <executions>
         <execution>
            <id>validate</id>
            <phase>validate</phase>
            <configuration>
               <!-- <configLocation>src/main/resources/checkstyle.xml</configLocation>  -->
               <encoding>UTF-8</encoding>
               <consoleOutput>true</consoleOutput>
               <failsOnError>true</failsOnError>
               <linkXRef>false</linkXRef>
               <skip>${skipCheckstyle}</skip>
            </configuration>
            <goals>
               <goal>check</goal>
            </goals>
         </execution>
      </executions>
   </plugin>
</plugins>
```


