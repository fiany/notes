# java特点

- 平台无关性
- GC
- 语言特性
- 面向对象
- 类库
- 异常处理

## 平台无关性

- javap查看字节码

### JVM如何加载.class文件

#### java虚拟机

![](D:\project\notes\assets\2019-04-17 134816.png)

- Class Loader:依据特定格斯，加载class文件到内存
- Execution Engine ：对命令进行解析
- Native Interface：融合不同开发语言的原生库为Java所用
- Runtime Data Area：JVM内存空间结构模型

### java反射

java反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意方法和属性；这种动态获取信息以及动态调用对象防范的功能成为java语言的反射机制。

#### 反射示例

```java
public static void main(Stirng[] args ){
    Class clazz = Class.forName("com.Robot");
    Robot r = (Robot)clazz.newInstance();
    // 获取该类所有方法（不能获取所继承，或者实现的接口方法）
    Method method = clazz.getDeclaredMethod("throwHello",String.class);
    // 设置私有方法可以访问
    method.setAccessible(true);
    Object str = method.invoke(r,"ni");
    // 获取该类所有public方法
    Method methodHello = clazz.getMethod("hello",String.class);
    Field name = clazz.getDeclaredField("name");
    name.setAccessible(true);
}
```

### ClassLoader

ClassLoader在java中有着非常重要的作用，它主要工作在Class转载的加载阶段，其主要作用是从系统外部获取Class二进制数据流。它是java的核心组件，所有的Class都是由ClassLoader进行加载的，ClassLoader负责通过将Class文件里的二进制数据流装载进系统，然后交给java虚拟机进行连接，初始化等操作。

#### ClassLoader的种类

- BootStrapClassLoader：C++编写，加载核心库java.*
- ExtCalssLoader：java编写，加载扩展库javax.*
- AppClassLoader：java编写，加载程序所在目录
- 自定义ClassLoader：java编写，定制化加载

#### 自定义ClassLoader

  ```java
public class MyClassLoader extends ClassLoader{
    private String path;
    private String classLoaderName;
    public MyClassLoader(String path, String classLoaderName){
        this.path =path;
        this.classLoaderName = classLoaderName;
    }
    // 用于寻找类文件
    @Override
    public Class findClass(String name){
        byte[] b = loadClassData(name);
        return defineClass(name,b,0,b.length);
    }
    // 用于加载类文件
    private byte[] loadClassData(String name){
        name = path + name + ".calss";
        InputStream in = null;
        ByteArrayOutputStream out = null;
        try {
            in = new FileInputStream（new File(name));
            out = new ByteArrayOutputStream();
            int i = 0;
            while((i = in.read()) != -1){
                out.write(i);
            }catch() {
                // 异常处理
            } finally {
                // 关闭流
            }
        }
    }
}
  ```

#### 类加载器的双亲委派机制

![](D:\project\notes\assets\2019-04-17150748.png)

### 类的加载方式

- 隐式加载：new
- 显式加载：loadClass ，forName等

#### loadclass 与 forName的区别

- Class.forName得到额class时已经初始化完成的
  - 应用：Class.forName("com.mysql.jdbc.Driver");
- Classloader.loadClass得到的class是还没有链接的
  - 应用：spring ioc中大量使用，让类在初次使用的时候才完成初始化，提高容器启动速度



 