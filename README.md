
<!---
Moualek/Moualek is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
<execution>
<phase>package</phase>
<goals>
<goal>shade</goal>
</goals>
<configuration>
<transformers>
<transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
<mainClass>com.googlecode.aviator.Main</mainClass>
</transformer>
</transformers>
<outputFile>aviator.jar</outputFile>
</configuration>
</execution>
</executions>
</plugin>
</plugins>
</build>
</profile>
<profile>
<id>release</id>
<activation>
<activeByDefault>false</activeByDefault>
<property>
<!--  where property `performRelease` comes from? - maven-release-plugin
	       Whether to use the release profile that adds sources and javadocs to the
	       released artifact, if appropriate. If set to true, the release plugin sets
	       the property "performRelease" to true, which activates the profile "release-profile",
	       which is inherited from the super pom. Default value is: true. https://maven.apache.org/plugins-archives/maven-release-plugin-2.2.2/perform-mojo.html
	       - All models implicitly inherit from a super-POM which added maven-source-plugin
	       and maven-javadoc-plugin https://maven.apache.org/ref/3.0.4/maven-model-builder/super-pom.html  -->
<name>performRelease</name>
<value>true</value>
</property>
</activation>
<build>
<plugins>
<plugin>
<artifactId>maven-gpg-plugin</artifactId>
<version>1.6</version>
<executions>
<execution>
<id>sign-artifacts</id>
<phase>verify</phase>
<goals>
<goal>sign</goal>
</goals>
</execution>
</executions>
</plugin>
<!--  Maven plugin which includes build-time git repository information
	       into an POJO / *.properties). Make your apps tell you which version exactly
	       they were built from! Priceless in large distributed deployments. https://github.com/ktoso/maven-git-commit-id-plugin  -->
<plugin>
<groupId>pl.project13.maven</groupId>
<artifactId>git-commit-id-plugin</artifactId>
<version>3.0.0</version>
<executions>
<execution>
<id>get-the-git-infos</id>
<goals>
<goal>revision</goal>
</goals>
</execution>
<execution>
<id>validate-the-git-infos</id>
<goals>
<goal>validateRevision</goal>
</goals>
</execution>
</executions>
<configuration>
<validationProperties>
<!--  verify that the current repository is not dirty  -->
<validationProperty>
<name>validating git dirty</name>
<value>${git.dirty}</value>
<shouldMatchTo>false</shouldMatchTo>
</validationProperty>
</validationProperties>
<generateGitPropertiesFile>true</generateGitPropertiesFile>
<generateGitPropertiesFilename>${project.build.outputDirectory}/META-INF/scm/${project.groupId}/${project.artifactId}/git.properties</generateGitPropertiesFilename>
</configuration>
</plugin>
</plugins>
</build>
</profile>
</profiles>
<reporting>
<plugins>
<plugin>
<artifactId>maven-javadoc-plugin</artifactId>
<configuration>
<encoding>utf-8</encoding>
<charset>utf-8</charset>
</configuration>
</plugin>
</plugins>
</reporting>
</project>
