## 一、Callable与Runnable
先说一下java.lang.Runnable吧，它是一个接口，在它里面只声明了一个run()方法：
```java
public interface Runnable {
    public abstract void run();
}
```
由于run()方法返回值为void类型，所以在执行完任务之后无法返回任何结果。
Callable位于java.util.concurrent包下，它也是一个接口，在它里面也只声明了一个方法，只不过这个方法叫做call()：
```java
public interface Callable<V> {
    /**
     * Computes a result, or throws an exception if unable to do so.
     *
     * @return computed result
     * @throws Exception if unable to compute a result
     */
    V call() throws Exception;
}
```
可以看到，这是一个泛型接口，call()函数返回的类型就是传递进来的V类型。
那么怎么使用Callable呢？一般情况下是配合ExecutorService来使用的，在ExecutorService接口中声明了若干个submit方法的重载版本：
```java
<T> Future<T> submit(Callable<T> task);
<T> Future<T> submit(Runnable task, T result);
Future<?> submit(Runnable task);
```
第一个submit方法里面的参数类型就是Callable。
暂时只需要知道Callable一般是和ExecutorService配合来使用的，具体的使用方法讲在后面讲述。
一般情况下我们使用第一个submit方法和第三个submit方法，第二个submit方法很少使用。

## 二、Future
Future就是对于具体的Runnable或者Callable任务的执行结果进行取消、查询是否完成、获取结果。必要时可以通过get方法获取执行结果，该方法会阻塞直到任务返回结果。
Future类位于java.util.concurrent包下，它是一个接口：
```java
public interface Future<V> {
    boolean cancel(boolean mayInterruptIfRunning);
    boolean isCancelled();
    boolean isDone();
    V get() throws InterruptedException, ExecutionException;
    V get(long timeout, TimeUnit unit)
        throws InterruptedException, ExecutionException, TimeoutException;
}
```
在Future接口中声明了5个方法，下面依次解释每个方法的作用：
* cancel方法用来取消任务，如果取消任务成功则返回true，如果取消任务失败则返回false。参数mayInterruptIfRunning表示是否允许取消正在执行却没有执行完毕的任务，如果设置true，则表示可以取消正在执行过程中的任务。如果任务已经完成，则无论mayInterruptIfRunning为true还是false，此方法肯定返回false，即如果取消已经完成的任务会返回false；如果任务正在执行，若mayInterruptIfRunning设置为true，则返回true，若mayInterruptIfRunning设置为false，则返回false；如果任务还没有执行，则无论mayInterruptIfRunning为true还是false，肯定返回true。
* isCancelled方法表示任务是否被取消成功，如果在任务正常完成前被取消成功，则返回 true。
* isDone方法表示任务是否已经完成，若任务完成，则返回true；
* get()方法用来获取执行结果，这个方法会产生阻塞，会一直等到任务执行完毕才返回；
* get(long timeout, TimeUnit unit)用来获取执行结果，如果在指定时间内，还没获取到结果，就直接返回null。

也就是说Future提供了三种功能：
  1. 判断任务是否完成；
  2. 能够中断任务；
  3. 能够获取任务执行结果。

因为Future只是一个接口，所以是无法直接用来创建对象使用的，因此就有了下面的FutureTask。

## 三、FutureTask
我们先来看一下FutureTask的实现：
```java
public class FutureTask<V> implements RunnableFuture<V>
```
FutureTask类实现了RunnableFuture接口，我们看一下RunnableFuture接口的实现：
```java
public interface RunnableFuture<V> extends Runnable, Future<V> {
    void run();
}
```
可以看出RunnableFuture继承了Runnable接口和Future接口，而FutureTask实现了RunnableFuture接口。所以它既可以作为Runnable被线程执行，又可以作为Future得到Callable的返回值。
FutureTask提供了2个构造器：
```java
public FutureTask(Callable<V> callable) {}

public FutureTask(Runnable runnable, V result) {}
```

## 四、使用示例
  1. ### 使用Callable+Future获取执行结果
      ```java
      public class Test {
        public static void main(String[] args) {
            ExecutorService executor = Executors.newCachedThreadPool();
            Task task = new Task();
            Future<Integer> result = executor.submit(task);
            executor.shutdown();
             
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e1) {
                e1.printStackTrace();
            }
             
            System.out.println("主线程在执行任务");
             
            try {
                System.out.println("task运行结果"+result.get());
            } catch (InterruptedException e) {
                e.printStackTrace();
            } catch (ExecutionException e) {
                e.printStackTrace();
            }
             
            System.out.println("所有任务执行完毕");
          }
      }
      class Task implements Callable<Integer>{
          @Override
          public Integer call() throws Exception {
              System.out.println("子线程在进行计算");
              Thread.sleep(3000);
              int sum = 0;
              for(int i=0;i<100;i++)
                  sum += i;
              return sum;
          }
      }
      ```
  2. ### 使用Callable+FutureTask获取执行结果
      ```java
      public class Test {
      public static void main(String[] args) {
          //第一种方式
          ExecutorService executor = Executors.newCachedThreadPool();
          Task task = new Task();
          FutureTask<Integer> futureTask = new FutureTask<Integer>(task);
          executor.submit(futureTask);
          executor.shutdown();
           
          //第二种方式，注意这种方式和第一种方式效果是类似的，只不过一个使用的是ExecutorService，一个使用的是Thread
          /*Task task = new Task();
          FutureTask<Integer> futureTask = new FutureTask<Integer>(task);
          Thread thread = new Thread(futureTask);
          thread.start();*/
           
          try {
              Thread.sleep(1000);
          } catch (InterruptedException e1) {
              e1.printStackTrace();
          }
           
          System.out.println("主线程在执行任务");
           
          try {
              System.out.println("task运行结果"+futureTask.get());
          } catch (InterruptedException e) {
              e.printStackTrace();
          } catch (ExecutionException e) {
              e.printStackTrace();
          }
           
          System.out.println("所有任务执行完毕");
        }
      }
      class Task implements Callable<Integer>{
          @Override
          public Integer call() throws Exception {
              System.out.println("子线程在进行计算");
              Thread.sleep(3000);
              int sum = 0;
              for(int i=0;i<100;i++)
                  sum += i;
              return sum;
          }
      } 
      ```
    如果为了可取消性而使用 Future 但又不提供可用的结果，则可以声明 Future<?> 形式类型、并返回 null 作为底层任务的结果。
    
## 五、Java四种线程池的使用
  1. ### new Thread的弊端
      ```java
      new Thread(new Runnable() {
          @Override
          public void run() {
              // TODO Auto-generated method stub
          }
      }).start();
      ```
      弊端：
        1. 每次new Thread新建对象性能差。
        2. 线程缺乏统一管理，可能无限制新建线程，相互之间竞争，及可能占用过多系统资源导致死机或oom。
        3. 缺乏更多功能，如定时执行、定期执行、线程中断。
        相比new Thread，Java提供的四种线程池的好处在于：
        4. 重用存在的线程，减少对象创建、消亡的开销，性能佳。
        5. 可有效控制最大并发线程数，提高系统资源的使用率，同时避免过多资源竞争，避免堵塞。
        6. 提供定时执行、定期执行、单线程、并发数控制等功能。
  2. ### Executors提供四种线程池
      * `newCachedThreadPool`创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。线程池的规模不存在限制。
      * `newFixedThreadPool` 创建一个固定长度线程池，可控制线程最大并发数，超出的线程会在队列中等待。
      * `newScheduledThreadPool` 创建一个固定长度线程池，支持定时及周期性任务执行。
      * `newSingleThreadExecutor` 创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行。
      > 下面代码说明：
      * #### newCachedThreadPool
          > 创建一个定长线程池，可控制线程最大并发数，超出的线程会在队列中等待。
          ```java
          ExecutorService fixedThreadPool = Executors.newFixedThreadPool(3);
          for (int i = 0; i < 10; i++) {
              final int index = i;
              fixedThreadPool.execute(new Runnable() {
            
                  @Override
                  public void run() {
                      try {
                          System.out.println(index);
                          Thread.sleep(2000);
                      } catch (InterruptedException e) {
                          // TODO Auto-generated catch block
                          e.printStackTrace();
                      }
                  }
              });
          }
          ```
          线程池为无限大，当执行第二个任务时第一个任务已经完成，会复用执行第一个任务的线程，而不用每次新建线程。
          
      * #### newFixedThreadPool
          创建一个定长线程池，可控制线程最大并发数，超出的线程会在队列中等待。
          ```java
          ExecutorService fixedThreadPool = Executors.newFixedThreadPool(3);
          for (int i = 0; i < 10; i++) {
              final int index = i;
              fixedThreadPool.execute(new Runnable() {
            
                  @Override
                  public void run() {
                      try {
                          System.out.println(index);
                          Thread.sleep(2000);
                      } catch (InterruptedException e) {
                          // TODO Auto-generated catch block
                          e.printStackTrace();
                      }
                  }
              });
          }
          ```
          因为线程池大小为3，每个任务输出index后sleep 2秒，所以每两秒打印3个数字。
          定长线程池的大小最好根据系统资源进行设置
      * #### newScheduledThreadPool
          创建一个定长线程池，支持定时及周期性任务执行。
          ```java
          ScheduledExecutorService scheduledThreadPool = Executors.newScheduledThreadPool(5);
          scheduledThreadPool.schedule(new Runnable() {
            
              @Override
              public void run() {
                  System.out.println("delay 3 seconds");
              }
          }, 3, TimeUnit.SECONDS);
          ```
          表示延迟3秒执行。
          定期执行示例代码如下：
          ```java
          scheduledThreadPool.scheduleAtFixedRate(new Runnable() {
              @Override
              public void run() {
                  System.out.println("delay 1 seconds, and excute every 3 seconds");
              }
          }, 1, 3, TimeUnit.SECONDS);
          ```
          表示延迟1秒后每3秒执行一次。
          ScheduledExecutorService比Timer更安全，功能更强大。
      * #### newSingleThreadExecutor
          创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行。
          ```java
          ExecutorService singleThreadExecutor = Executors.newSingleThreadExecutor();
          for (int i = 0; i < 10; i++) {
              final int index = i;
              singleThreadExecutor.execute(new Runnable() {
            
                  @Override
                  public void run() {
                      try {
                          System.out.println(index);
                          Thread.sleep(2000);
                      } catch (InterruptedException e) {
                          // TODO Auto-generated catch block
                          e.printStackTrace();
                      }
                  }
              });
          }
          结果依次输出，相当于顺序执行各个任务。
          
## 六、谈谈new Thread的弊端及Java四种线程池的使用
  > 当我们通过Executor提交一组并发执行的任务，并且希望在每一个任务完成后能立即得到结果，有两种方式可以采取：
  
  * ### 方式一
    > 通过一个list来保存一组future，然后在循环中轮训这组future,直到每个future都已完成。如果我们不希望出现因为排在前面的任务阻塞导致后面先完成的任务的结果没有及时获取的情况，那么在调用get方式时，需要将超时时间设置为0
    ```java
    public class CompletionServiceTest { 
   
        static class Task implements Callable<String>{ 
            private int i; 
               
            public Task(int i){ 
                this.i = i; 
            } 
       
            @Override 
            public String call() throws Exception { 
                Thread.sleep(10000); 
                return Thread.currentThread().getName() + "执行完任务：" + i; 
            }    
        } 
           
        public static void main(String[] args){ 
            testUseFuture(); 
        } 
           
        private static void testUseFuture(){ 
            int numThread = 5; 
            ExecutorService executor = Executors.newFixedThreadPool(numThread); 
            List<Future<String>> futureList = new ArrayList<Future<String>>(); 
            for(int i = 0;i<numThread;i++ ){ 
                Future<String> future = executor.submit(new CompletionServiceTest.Task(i)); 
                futureList.add(future); 
            } 
                       
            while(numThread > 0){ 
                for(Future<String> future : futureList){ 
                    String result = null; 
                    try { 
                        result = future.get(0, TimeUnit.SECONDS); 
                    } catch (InterruptedException e) { 
                        e.printStackTrace(); 
                    } catch (ExecutionException e) { 
                        e.printStackTrace(); 
                    } catch (TimeoutException e) { 
                        //超时异常直接忽略 <br>　　　　　　　　　　　　//future.cancel(true);//超时设置任务取消
                    } 
                    if(null != result){ 
                        futureList.remove(future); 
                        numThread--; 
                        System.out.println(result); 
                        //此处必须break，否则会抛出并发修改异常。（也可以通过将futureList声明为CopyOnWriteArrayList类型解决） 
                        break; 
                    } 
                } 
            } 
        } 
    }
    ```
  * ### 方式二
    > 第一种方式显得比较繁琐，通过使用ExecutorCompletionService，则可以达到代码最简化的效果。
    ```java
    public class CompletionServiceTest { 
      static class Task implements Callable<String>{ 
          private int i; 
             
          public Task(int i){ 
              this.i = i; 
          } 
     
          @Override 
          public String call() throws Exception { 
              Thread.sleep(10000); 
              return Thread.currentThread().getName() + "执行完任务：" + i; 
          }    
      } 
               
      public static void main(String[] args) throws InterruptedException, ExecutionException{ 
          testExecutorCompletionService(); 
      } 
         
      private static void testExecutorCompletionService() throws InterruptedException, ExecutionException{ 
          int numThread = 3; 
          ExecutorService executor = Executors.newFixedThreadPool(numThread); 
          CompletionService<String> completionService = new ExecutorCompletionService<String>(executor); 
          for(int i = 0;i<numThread;i++ ){ 
              completionService.submit(new CompletionServiceTest.Task(i)); 
          } 
        } 
           
        for(int i = 0;i<numThread;i++ ){      
            System.out.println(completionService.take().get());  //获取执行结果
        } 
    } 
    ```
    
    > ExecutorCompletionService分析: CompletionService是Executor和BlockingQueue的结合体。
      ```java
      public ExecutorCompletionService(Executor executor) { 
          if (executor == null) 
              throw new NullPointerException(); 
          this.executor = executor; 
          this.aes = (executor instanceof AbstractExecutorService) ? 
              (AbstractExecutorService) executor : null; 
          this.completionQueue = new LinkedBlockingQueue<Future<V>>(); 
      } 
      ```
      任务的提交和执行都是委托给Executor来完成。在构造函数中创建一个BlockingQueue来保存计算完成的结果，当提交某个任务时，该任务首先将被包装为一个QueueingFuture，
      
      ```java
      public Future<V> submit(Callable<V> task) { 
          if (task == null) throw new NullPointerException(); 
          RunnableFuture<V> f = newTaskFor(task); 
          executor.execute(new QueueingFuture(f)); 
          return f; 
      } 
      ```
      QueueingFuture，这是FutureTask的一个子类，通过改写该子类的done方法，可以实现当任务完成时，将结果放入到BlockingQueue中。
      ```java
      private class QueueingFuture extends FutureTask<Void> {

          QueueingFuture(RunnableFuture<V> task) {
  
              super(task, null);
  
              this.task = task;
  
          }
  
          protected void done() { completionQueue.add(task); }
  
          private final Future<V> task;
  
      }
      ```
      而通过使用BlockingQueue的take(阻塞获取)或poll(非阻塞获取)方法，则可以得到结果。在BlockingQueue不存在元素时，这两个操作会阻塞，一旦有结果加入，则立即返回。

  > 附加知识点：
  
  * take()：取走BlockingQueue里排在首位的对象,若BlockingQueue为空,阻断进入等待状态直到Blocking有新的对象被加入为止;
  * poll(time):取走BlockingQueue里排在首位的对象,若不能立即取出,则可以等time参数规定的时间,取不到时返回nul
  
  ```java
  public Future<V> take() throws InterruptedException { 
      return completionQueue.take(); 
  } 
     
  public Future<V> poll() { 
      return completionQueue.poll(); 
  } 
  ```