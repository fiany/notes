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

