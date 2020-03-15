# JVM

## 一、字节码文件一

cmd javap指令：

![javap_help](C:\Users\TKW\Desktop\note\jvm_pic\javap_help.png)

## 二、JAVA反射

```java
        //获取Class字节码对象方式
        //方式一、类名.class
        Class<StudentBean> studentBeanClass = StudentBean.class;
//        System.out.println(studentBeanClass);
        //方式二、对象.getClass();
        StudentBean studentBean = new StudentBean();
        Class<? extends StudentBean> aClass = studentBean.getClass();
//        System.out.println(aClass);
        //方式三、Class.forName("类reference");
        Class<?> aClass1 = Class.forName("jvm.reflection.StudentBean");
//        System.out.println(aClass1);

        //创建类
        Constructor<?> constructor = aClass1.getConstructor();
        Object o = constructor.newInstance();
//        System.out.println(o.toString());

        //获得所有public方法，该方法可以获得所有的public方法，包括从父类继承来的
        Method[] methods = aClass1.getMethods();
//        for (Method m : methods) {
//            System.out.println(m.toString());
//        }

        //获得当前类的所有方法，public,default,protected,private均可获得，但父类的方法不可
        Method[] declaredMethods = aClass1.getDeclaredMethods();
//        for (Method m : declaredMethods) {
//            System.out.println(m.toString());
//        }

        //获得单个方法，getMethod()方法第一个参数为方法名，后面为所需参数类型的字节码对象
        Method toString = aClass1.getMethod("toString");
//        System.out.println(toString);
        Method setAge = aClass1.getMethod("setAge", int.class);
//        System.out.println(setAge);
        //注意，该方法只能获得public方法的Method对象
//        Method setStudent = aClass1.getMethod("setStudent", boolean.class);
//        System.out.println(setStudent);

        //获得任意单个方法对象,
        Method setStudent = aClass1.getDeclaredMethod("setStudent", boolean.class);
//        System.out.println(setStudent);

        //方法调用,调用invoke()方法，第一个参数为类字节码对象，后面的为实参参数
        setAge.invoke(o, 1);
//        System.out.println(o);
        //但注意，非public方法是不能直接进行调用的,需要调用setAccessible()方法
        setStudent.setAccessible(true);
        setStudent.invoke(o, true);
//        System.out.println(o);

        //获得所有public属性，包括父类的public属性，但其他修饰的不可
        Field[] fields = aClass1.getFields();
//        for (Field f : fields) {
//            System.out.println(f.toString());
//        }

        //获得当前类的所有属性，不包括父类的属性
        Field[] declaredFields = aClass1.getDeclaredFields();
//        for (Field f : declaredFields) {
//            System.out.println(f.toString());
//        }

        //获得单个字段，可获得所有public修饰的字段
        Field age = aClass1.getField("age");
        //get()传入对象，获得字段的值
//        System.out.println(age.get(o));

        //getField()不可直接调用，会提示没有当前字段,可使用getDeclaredFiled()
//        aClass1.getField("name");
        Field isStudent = aClass1.getDeclaredField("isStudent");
        isStudent.setAccessible(true);
        System.out.println(isStudent.get(o));
        isStudent.set(o, false);
        System.out.println(o);

        Scanner sc = new Scanner(System.in);
        if (sc.hasNextLine()){//判断还有没
//            String[] strings = sc.nextLine().trim().split(" ");
        }


        ArrayList<String[]> strings = new ArrayList<>();
        while (sc.hasNextLine()){
            String[] st = sc.nextLine().trim().split(" ");
            strings.add(st);
        }
```

## 三、ClassLoader

ClassLoader：类加载器，顶层为BootstracpClassLoader，是C写的，

- BootstracpClassLoader：是负责加载$JAVA_ HOME中jre/ib/rt.jar里所有的class ,由C++实现,不是ClassL oader子类。它的类加载路径为：

  ```java
  private static String bootClassPath = System.getProperty("sun.boot.class.path");
  ```

- ExtensionClassLoader(ExtClassLoader)：负责加载java平台中扩展功能的一些jar包,包括\$JAVA_ HOME中jre/ib/*.jar或-Djava.ext.dirs指定目录下的jar包。

- AppClassLoader：负责加载classpath中指定的jar包及目录中class。

  Launcher.java中Launcher类会在其构造方法中调用获取ExtClassLoader和AppClassLoader。

  ```java
  ![ClassLoader_extends](C:\Users\TKW\Desktop\note\jvm_pic\ClassLoader_extends.png)public Launcher() {
          // Create the extension class loader
          ClassLoader extcl;
          try {
              extcl = ExtClassLoader.getExtClassLoader();
          } catch (IOException e) {
              throw new InternalError(
                  "Could not create extension class loader", e);
          }
  
          // Now create the class loader to use to launch the application
          try {
              loader = AppClassLoader.getAppClassLoader(extcl);
          } catch (IOException e) {
              throw new InternalError(
                  "Could not create application class loader", e);
          }
  		
          // Also set the context class loader for the primordial thread.
          Thread.currentThread().setContextClassLoader(loader);
  
          // Finally, install a security manager if requested
          String s = System.getProperty("java.security.manager");
          if (s != null) {
              SecurityManager sm = null;
              if ("".equals(s) || "default".equals(s)) {
                  sm = new java.lang.SecurityManager();
              } else {
                  try {
                      sm = (SecurityManager)loader.loadClass(s).newInstance();
                  } catch (IllegalAccessException e) {
                  } catch (InstantiationException e) {
                  } catch (ClassNotFoundException e) {
                  } catch (ClassCastException e) {
                  }
              }
              if (sm != null) {
                  System.setSecurityManager(sm);
              } else {
                  throw new InternalError(
                      "Could not create SecurityManager: " + s);
              }
          }
      }
  ```

  

- CustomClassLoaader：应用程序根据自身需要自定义的ClassL oader ,如tomcat. jboss都会根据j2ee规范自行实现ClassL oader。

类继承关系如下：

![ClassLoader_extends](C:\Users\TKW\Desktop\note\jvm_pic\ClassLoader_extends.png)

而所谓双亲委派：即通过如下流程：

![classLoader_double_parents](C:\Users\TKW\Desktop\note\jvm_pic\classLoader_double_parents.png)

当加载一个Class时，首先通过自定义的ClassLoader进行查找，若找到，则返回，否则交由上层ClassLoade 再重复该步骤，直至找到或者到达BootstrapClassLoader，若还是没有找到缓存，则往相反的顺序从BootstrapClassLoade开始往下逐个往下去查找各自分管的区域中是否有该类，如果找到，则进行加载，否则就交给下层ClassLoader，继续查找，如果到最下层的AppClassLoader或者CustomClassLoader都没找到，则抛出ClassNotFoundExecption，采用双亲委派策略，保证了语言内部的类不会被重写，保证了安全。并且通过这样的调用方式，避免了类的重复加载。





