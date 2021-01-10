设计一个类, 我们只能生成该类的一个实例



## 1. 饿汉式

```java
public class Hungry {
    //饿汉式浪费空间, 一上来就把对象加载到内存中
    private AtomicLong atomicLong = new AtomicLong(); //假设单例模式中的资源
    private Hungry(){
        
    }
    private final static Hungry HUNGRY = new Hungry();
    public static Hungry getInstance(){
        return HUNGRY;
    }
    public long getId(){
        return atomicLong.incrementAndGet();
    }
}
```

能防住多线程, 不能防住反射.

## 2. 懒汉式

````java
class Lazy{
    //单线程下没问题, 多线程不行
    private AtomicLong atomicLong = new AtomicLong();
    private Lazy(){
        System.out.println("one lazy singleton has been created");
    }
    private static Lazy LAZY;

    public static Lazy getInstance(){
        if(LAZY == null){
            LAZY = new Lazy();
        }
        return LAZY;
    }
    public long getId(){
        return atomicLong.incrementAndGet();
    }
````

不能防住多线程, 不能防住反射.

## 3. 加锁懒汉式

```java
//加锁之后的懒汉式单例模式, DCL
class LockLazy{
    private AtomicLong atomicLong = new AtomicLong();
    private LockLazy(){
        System.out.println("one locklazy singleton has been created");
    }
    private static LockLazy LOCKLAZY;
    //双重检查, 如果单例已经创建了, 就不加锁. 单例未创建才加锁.
    public static LockLazy getInstance(){
        if(LOCKLAZY == null){
            synchronized (LockLazy.class){
                if(LOCKLAZY == null){
                    LOCKLAZY = new LockLazy();         
                }
            }
        }
        return LOCKLAZY;
    }
    public long getId(){
        return atomicLong.incrementAndGet();
    }
}
//但是这样也还是不安全, 如果一个线程进行new LockLazy();操作,这个操作不是原子性的, 会被jvm大体分为3个阶段. 
//1. 分配内存空间 2. 初始化对象, 进行属性赋值. 3. 将对象指向这个空间
//如果没有发生指令重排, 是安全的. 如果指令重排为 132这样的顺序, 当一个线程执行完第3步, 还没有执行第2步, 这时时间片用尽, 切换到另一个线程, 另外一个线程执行getInstance()方法, 一看LOCKLAZY非空, 就直接返回了这个对象了, 但是这个对象内还没有初始化. 自然就会报错, 所以为了安全一定要将单例的引用加volatile
```

## 4. 加锁加volatile懒汉式

```java
class SafeLazy{
    private AtomicLong atomicLong = new AtomicLong();
    private volatile static SafeLazy SAFELAZY;
    private SafeLazy(){
        
    }

    public static SafeLazy getInstance(){
        if(SAFELAZY == null)
        {
            synchronized (SafeLazy.class)
            {
                if(SAFELAZY == null)
                {
                    SAFELAZY = new SafeLazy();
                }
                return SAFELAZY;
            }
        }
        return SAFELAZY;
    }
    
    public long getId(){
        return atomicLong.incrementAndGet();
    }
}
```

多线程足够安全, 仍然防不住反射

## 5. 静态内部类实现单例模式

```java
public class Holder {
    private Holder(){

    }
    private AtomicLong atomicLong = new AtomicLong();
    private static class InnerClass
    {
        private static final Holder HOLDER = new Holder();
    }
    public static Holder getInstance(){
        return InnerClass.HOLDER;
    }
    public long getId(){
        return atomicLong.incrementAndGet();
    }
}
//静态内部类单例模式也称单例持有者模式，实例由内部类创建，由于 JVM 在加载外部类的过程中,
// 是不会加载静态内部类的, 只有内部类的属性/方法被调用时才会被加载, 并初始化其静态属性
```

就是把饿汉式的new 单例模式的过程`private final static Hungry HUNGRY = new Hungry();`放到个静态内部类中, 通过静态内部类的加载机制实现懒惰加载. 仍然防不住反射

## 6. 枚举类实现单例模式

```java
enum Singleton
{
    SINGLETON;
    private AtomicLong atomicLong = new AtomicLong();
    public long getId(){
        return atomicLong.incrementAndGet();
    }
    public static Singleton getInstance()
    {
        return SINGLETON;
    }
}
```

这个最安全, 多线程和反射都破坏不了, 反序列化的方法也破解不了. 也最简洁, 核心代码只需要3行. 加载模式和饿汉式一样, 都是提前加载. 怪不得都推荐使用这种方法