# 注解



##### @[Configuration](https://so.csdn.net/so/search?q=Configuration&spm=1001.2101.3001.7020)注解

标明一个类为配置类，相当于xml文件中的bean标签，是一个bean容器，@Configuration注解标明的类里面如果用@Bean标明一个方法，则作为容器的bean，与与xml中配置的bean意思一样。



##### @Value注解

为了简化从properties里取配置，可以使用@Value, 可以properties文件中的配置值。



##### @Controller, @Service, @Repository,@Component

目前4种注解意思是一样，并没有什么区别，区别只是名字不同。

一般在mvc三层架构里面，不同层次的类用不同的注解，如controller层controller, service层 service



##### @PostConstruct 和 @PreDestory 

[实现初始化和销毁bean之前进行的操作](http://blog.csdn.net/topwqp/article/details/8681497)，只能有一个方法可以用此注释进行注释，方法不能有参数，返回值必需是void,方法需要是非静态的。

~~~java
public class TestService { 

    @PostConstruct  
    public void  init(){  
        System.out.println("初始化");  
    }  

    @PreDestroy  
    public void  dostory(){  
        System.out.println("销毁");  
    }  
}
~~~

@PostConstruct：在构造方法和init方法（如果有的话）之间得到调用，且只会执行一次。

@PreDestory：注解的方法在destory()方法调用后得到执行。

![image-20220117125932393](Spring%E6%B3%A8%E8%A7%A3.assets/image-20220117125932393.png)





引深一点，Spring 容器中的 Bean 是有生命周期的，Spring 允许在 Bean 在初始化完成后以及 Bean 销毁前执行特定的操作，常用的设定方式有以下三种：

1.通过实现 InitializingBean/DisposableBean 接口来定制初始化之后/销毁之前的操作方法；

2.通过 <bean> 元素的 init-method/destroy-method属性指定初始化之后 /销毁之前调用的操作方法；

3.在指定方法上加上@PostConstruct 或@PreDestroy注解来制定该方法是在初始化之后还是销毁之前调用

但他们之前并不等价。即使3个方法都用上了，也有先后顺序.

**Constructor > @PostConstruct >InitializingBean > init-method**



##### @Primary

自动装配时当出现多个Bean候选者时，被注解为@Primary的Bean将作为首选者，否则将抛出异常。

~~~java
@Component  
public class Apple implements Fruit{  

    @Override  
    public String hello() {  
        return "我是苹果";  
    }  
}

@Component  
@Primary
public class Pear implements Fruit{  

    @Override  
    public String hello(String lyrics) {  
        return "梨子";  
    }  
}

public class FruitService { 

  //Fruit有2个实例子类，因为梨子用@Primary，那么会使用Pear注入
    @Autowired  
    private Fruit fruit;  

    public String hello(){  
        return fruit.hello();  
    }  
}
~~~



##### @Lazy(true)

用于指定该Bean是否取消预初始化，用于注解类，延迟初始化。



##### @Autowired

Autowired默认先按byType，如果发现找到多个bean，则，又按照byName方式比对，如果还有多个，则报出异常。

1.可以手动指定按byName方式注入，使用@Qualifier。

//通过此注解完成从spring配置文件中 查找满足Fruit的bean,然后按//@Qualifier指定pean

@Autowired

@Qualifier(“pean”)

public Fruit fruit;

2.如果要允许null 值，可以设置它的required属性为false，如：@Autowired(required=false) 

public Fruit fruit;



##### @Resource

默认按 byName自动注入,如果找不到再按byType找bean,如果还是找不到则抛异常，无论按byName还是byType如果找到多个，则抛异常。

可以手动指定bean,它有2个属性分别是name和type，使用name属性，则使用byName的自动注入，而使用type属性时则使用byType自动注入。

@Resource(name=”bean名字”)

或

@Resource(type=”bean的class”)

这个注解是属于J2EE的，减少了与spring的耦合。



##### @Async

 java里使用线程用3种方法：

\1. 继承Thread，重写run方法

\2. 实现Runnable,重写run方法

\3. 使用Callable和Future接口创建线程，并能得到返回值。



第三种方法：

~~~java
class MyCallable implements Callable<Integer> {
    private int i = 0;
    // 与run()方法不同的是，call()方法具有返回值
    @Override
    public Integer call() {
        int sum = 0;
        for (; i < 100; i++) {
            System.out.println(Thread.currentThread().getName() + " " + i);
            sum += i;
        }
        return sum;
    }
}
~~~

~~~java
public static void main(String[] args) {
        Callable<Integer> myCallable = new MyCallable(); 
    // 创建MyCallable对象
        FutureTask<Integer> ft = new FutureTask<Integer>(myCallable); 
    //使用FutureTask来包装MyCallable对象
    
        for (int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName() + " " + i);
            if (i == 30) {
                Thread thread = new Thread(ft);  
                //FutureTask对象作为Thread对象的target创建新的线程
                thread.start();                     
                //线程进入到就绪状态
            }
        }
        System.out.println("主线程for循环执行完毕..");
        try {
            int sum = ft.get();            //取得新创建的新线程中的call()方法返回的结果
            System.out.println("sum = " + sum);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
}
~~~



而使用@Async可视为第4种方法。基于@Async标注的方法，称之为异步方法,这个注解用于标注某个方法或某个类里面的所有方法都是需要异步处理的。被注解的方法被调用的时候，会在新线程中执行，而调用它的方法会在原来的线程中执行。









































# 来源，链接

https://blog.csdn.net/weixin_39805338/article/details/80770472