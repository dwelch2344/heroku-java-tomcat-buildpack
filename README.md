Heroku Java + Tomcat BuildPack
------------------------------
A simple modification to https://github.com/heroku/heroku-buildpack-java to download and include Tomcat 7 in a your build directory at /tomcat. You can then use the Maven AntRun plugin to overwrite any files configuration files and launch the project as necessary. See https://gist.github.com/4575349 or below for an example build configuration.



```xml
<build>
  <plugins>
    <!-- copy configurations for Heroku Tomcat -->
    <plugin>
    	<artifactId>maven-antrun-plugin</artifactId>
    	<executions>
    		<execution>
    			<id>copy-conf</id>
    			<phase>install</phase>
    			<configuration>
    				<tasks>
    					<copy todir="${project.build.directory}/../tomcat/conf">
    						<fileset dir="deploy/conf" />
    					</copy>
    				</tasks>
    			</configuration>
    			<goals>
    				<goal>run</goal>
    			</goals>
    		</execution>
    		<execution>
    			<id>copy-project</id>
    			<phase>install</phase>
    			<configuration>
    				<tasks>
    					<copy todir="${project.build.directory}/../tomcat/webapps/ROOT">
    						<fileset dir="target/${project.build.finalName}" />
    					</copy>
    				</tasks>
    			</configuration>
    			<goals>
    				<goal>run</goal>
    			</goals>
    		</execution>
    	</executions>
    </plugin>
  </plugins>
</build>

```