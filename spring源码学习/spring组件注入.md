## spring组件注入
### 注入方式
- @componentScan + 组件标注注解（Controller、Service、Repository、Component）
- @Bean注入
- @Import注入
- spring提供的FactoryBean注入

### 常用注解

- @DependOn 用于控制组件之间以来，控制容器启动时组件加载顺序