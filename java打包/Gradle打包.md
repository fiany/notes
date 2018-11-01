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
	launchScript()
}
```

