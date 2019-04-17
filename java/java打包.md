## java打包

### Gradle打包

#### 不包含依赖包进行打包

```groovy
bootJar {
	classifier = 'boot'
	excludes = ["*.jar"]
	}

```

#### 包含依赖包

```groovy

bootJar {
    // 修改打包名称
    archiveName = 'whatever.jar' 
	launchScript()
}
```

### Maven打包

#### 将依赖jar包单独出来

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-dependency-plugin</artifactId>
    <executions>
        <execution>
            <id>copy-dependencies</id>
            <phase>prepare-package</phase>
            <goals>
                <goal>copy-dependencies</goal>
            </goals>
            <configuration>
                <outputDirectory>${project.build.directory}/lib</outputDirectory>
                <!-- 需要排除的jar的 groupId -->
                <excludeGroupIds>
                    com.wteam
                </excludeGroupIds>
            </configuration>
        </execution>
    </executions>
</plugin>
```

#### 不包含依赖包进行打包

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
        <!-- main 入口 -->
        <mainClass>com.wteam.medical.MedicalApplication</mainClass>
        <!-- 设置为ZIP，此模式下spring-boot-maven-plugin会将Manifest.MF文件中的Main-Class设置为org.springframework.boot.loader.PropertiesLauncher -->
        <layout>ZIP</layout>
        <!-- 需要包含的jar包 -->
        <includes>
            <include>
                <groupId>nothing</groupId>
                <artifactId>nothing</artifactId>
            </include>
        </includes>
    </configuration>
    <executions>
        <execution>
            <goals>
                <goal>repackage</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```



