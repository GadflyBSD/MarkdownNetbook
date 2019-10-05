# 简介
> DelayQueue是一个支持延时获取元素的无界阻塞队列。队列使用PriorityQueue来实现。队列中的元素必须实现Delayed接口，在创建元素时可以指定多久才能从队列中获取当前元素。只有在延迟期满时才能从队列中提取元素。

* ### DelayQueue非常有用，可以运用在以下两个应用场景： 
  * #### 缓存系统的设计：使用DelayQueue保存缓存元素的有效期，使用一个线程循环查询DelayQueue，一旦能从DelayQueue中获取元素时，就表示有缓存到期了。
  * #### 定时任务调度：使用DelayQueue保存当天要执行的任务和执行时间，一旦从DelayQueue中获取到任务就开始执行，比如Timer就是使用DelayQueue实现的。

* ### DelayQueue只有两个很简单的构造方法：
  ```java
  public DelayQueue() {}
   
  public DelayQueue(Collection<? extends E> c) {
      this.addAll(c);
  }
  ```
* ### DelayQueue是什么
  > #### DelayQueue是一个无界的BlockingQueue，用于放置实现了Delayed接口的对象，其中的对象只能在其到期时才能从队列中取走。这种队列是有序的，即队头对象的延迟到期时间最长。注意：不能将null元素放置到这种队列中。
* ### DelayQueue能做什么
  1. 淘宝订单业务:下单之后如果三十分钟之内没有付款就自动取消订单。 
  2. 饿了吗订餐通知:下单成功后60s之后给用户发送短信通知。

# 实际开发中的应用
  > #### 简单的延时队列要有三部分：第一实现了Delayed接口的消息体、第二消费消息的消费者、第三存放消息的延时队列

1. #### 消息体。实现接口 Delayed ，重写方法 compareTo 和 getDelay
    ```java
    public class Message implements Delayed{

        private Map<String,String> body=new HashMap<>();  //消息内容
        private long excuteTime;//执行时间
        private String type;
    
        public Map<String, String> getBody() {
            return body;
        }
        public void setBody(Map<String, String> body) {
            this.body = body;
        }
        public long getExcuteTime() {
            return excuteTime;
        }
        public void setExcuteTime(long excuteTime) {
            this.excuteTime = excuteTime;
        }
        public String getType() {
            return type;
        }
        public void setType(String type) {
            this.type = type;
        }
        public Message(long delayTime,String type) {
            this.excuteTime = TimeUnit.NANOSECONDS.convert(delayTime, TimeUnit.MILLISECONDS) + System.nanoTime();
            this.type=type;
        }
        @Override
        public int compareTo(Delayed delayed) {
            long d = (getDelay(TimeUnit.NANOSECONDS) - delayed.getDelay(TimeUnit.NANOSECONDS));
            return (d == 0) ? 0 : ((d < 0) ? -1 : 1);
        }
        @Override
        public long getDelay(TimeUnit unit) {
            return  unit.convert(this.excuteTime - System.nanoTime(), TimeUnit.NANOSECONDS);
        }
    }
    ```
2. #### 声明消费者。实现接口Runnable
    ```java
    public class Consumer implements Runnable {

        // 延时队列
        private DelayQueue<Message> queue;
        public static boolean  isRun=false;
    
        public Consumer(DelayQueue<Message> queue) {
            this.queue = queue;
        }
    
        @Override
        public void run() {
            isRun=true;
            while (true) {
                try {
                    Message take = queue.take();
                    System.out.println("消息类型：" + take.getType());
                    Map<String,String> map=take.getBody();
                    System.out.println("消息内容：");
                    for (String key:map.keySet()){
                        System.out.println("key="+map.get(key));
                    }
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
    ```
3. #### 延迟队列管理者，用于在任何地方获取 DelayQueue 
    ```java
    public class DelayQueueManager {

        private static DelayQueue<Message> delayQueue=null;
    
        static {
            // 创建延时队列
           delayQueue = new DelayQueue<Message>();
        }
    
        public static DelayQueue<Message> getDelayQueue(){
            return delayQueue;
        }
    }
    ```
4. #### 使用延迟队列发送消息
    ```java
    // 添加延时消息,m1 延时5s
  	Message m1 = new Message( 5000,"订单");
  	m1.getBody().put("content","12345");
  	// 添加延时消息,m1 延时5s
  	DelayQueueManager.getDelayQueue().offer(m1);
  	if(!Consumer.isRun){
  		// 启动消费线程
  		new Thread(new Consumer(DelayQueueManager.getDelayQueue())).start();
  	}
    ```

# DelayQueue源码详解
  * #### DelayQueue类定义为：
    ```java
    public class DelayQueue<E extends Delayed> extends AbstractQueue<E> implements BlockingQueue<E>
    ```
  * #### 该类同样继承了AbstractQueue抽象类并实现了BlockingQueue接口，这里不再叙述。
队列中的元素必须实现Delayed接口，那我们先来看一下Delayed接口，Delayed接口的定义很简单：
    ```java
    public interface Delayed extends Comparable<Delayed> {
        long getDelay(TimeUnit unit);
    }
    ```
  * #### 只有一个getDelay(TimeUnit)方法，该方法返回与此对象相关的的剩余时间。同时我们看到Delayed接口继承自Comperable接口，所以实现Delayed接口的类还必须要定义一个compareTo方法，该方法提供与此接口的getDelay方法一致的排序。

    > DelayQueue中比较重要的字段如下：
    
    ```java
    private final transient ReentrantLock lock = new ReentrantLock();
    private final PriorityQueue<E> q = new PriorityQueue<E>();
    private Thread leader = null;
    private final Condition available = lock.newCondition();
    ```
    * lock：全局独占锁，用于实现线程安全
    * q：优先队列，用于存储元素，并按优先级排序
    * leader：用于优化内部阻塞通知的线程
    * available：用于实现阻塞的Condition对象

  * #### 其实看到这里，我们应该已经能够了解DelayQueue的大致实现思路了：
    > 以支持优先级的PriorityQueue无界队列作为一个容器，因为元素都必须实现Delayed接口，可以根据元素的过期时间来对元素进行排列，因此，先过期的元素会在队首，每次从队列里取出来都是最先要过期的元素。

* ## 入队
    我们来看一下add(E e)方法：
    ```java
    public boolean add(E e) {
        return offer(e);
    }
    ```
    该方法通过调用offer(E e)来添加元素：
    ```java
    public boolean offer(E e) {
        // 获取全局独占锁
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            // 向优先队列中插入元素
            q.offer(e);
            // 如果队首元素是刚插入的元素，则设置leader为null，并唤醒阻塞在available上的线程
            if (q.peek() == e) {
                leader = null;
                available.signal();
            }
            return true;
        } finally {
            // 释放全局独占锁
            lock.unlock();
        }
    }
    ```
    PriorityQueue的入队操作与PriorityBlockingQueue基本一致，这里不再叙述。
  
    下面我们主要介绍一下leader变量：
    > #### leader是等待获取队列头元素的线程，应用主从式设计减少不必要的等待。如果leader不等于空，表示已经有线程在等待获取队列的头元素。所以，通过await()方法让出当前线程等待信号。如果leader等于空，则把当前线程设置为leader，当一个线程为leader，它会使用awaitNanos()方法让当前线程等待接收信号或等待delay时间。

* ## 出队
  出队我们来看一下会阻塞的take()方法：
  ```java
  public E take() throws InterruptedException {
    // 获取全局独占锁
    final ReentrantLock lock = this.lock;
    lock.lockInterruptibly();
    try {
      for (;;) {
        // 获取队首元素
        E first = q.peek();
        // 队首为空，则阻塞当前线程
        if (first == null){
          available.await();
        }else {
          // 获取队首元素的超时时间
          long delay = first.getDelay(NANOSECONDS);
          // 已超时，直接出队
          if (delay <= 0) return q.poll();
          // 释放first的引用，避免内存泄漏
          first = null; // don't retain ref while waiting
          // leader != null表明有其他线程在操作，阻塞当前线程
          if (leader != null){
              available.await();
          }else {
            // leader指向当前线程
            Thread thisThread = Thread.currentThread();
            leader = thisThread;
            try {
              // 超时阻塞
              available.awaitNanos(delay);
            } finally {
              // 释放leader
              if (leader == thisThread) leader = null;
            }
          }
        }
      }
    } finally {
      // leader为null并且队列不为空，说明没有其他线程在等待，那就通知条件队列
      if (leader == null && q.peek() != null)
        available.signal();
      // 释放全局独占锁
      lock.unlock();
    }
  }
  ```
  > #### 这里为什么如果不设置first = null，则会引起内存泄漏呢？线程A到达，列首元素没有到期，设置leader = 线程A，这是线程B来了因为leader != null，则会阻塞，线程C一样。假如线程阻塞完毕了，获取列首元素成功，出列。这个时候列首元素应该会被回收掉，但是问题是它还被线程B、线程C持有着，所以不会回收，这里只有两个线程，如果有线程D、线程E…呢？这样会无限期的不能回收，就会造成内存泄漏。