# 线程池
- Executor(Interface),ThreadPoolExecutor(Impl)
- 构造参数说明：
    - coolPoolsize
        - 默认情况下一直活着，除非设置ThreadPoolExecutor的allowCoreThreadTimeout=true,核心线程闲置超时也会被终止
    - maxinumPoolsize
        - 最大线程数，新任务超过最大值就要等待
    - keepAliveTime
        - 线程闲置保活时间
    - unit
        - keepAliveTime的单位
    - workQueue:任务队列，存储runnable对象
    - ThreadFactory  
        - 创建线程
- 参考AsyncTask参数配置：
    - coolposize=cpucount+1
    - maximumpoolsize=cpucount*2+1
    - 核心线程无超时机制，非核心线程超时时间1秒
    - 任务队列容量128
- Executor构建线程池的静态方法（但是并不建议使用Executor来构建线程池）
    - Executors.newFixedThreadPool()
        - 固定核心线程，全是核心线程，无超时机制，无任务队列上线 
            - 适合需要快速响应的任务
    ```
    /**
     * Creates a thread pool that reuses a fixed number of threads
     * operating off a shared unbounded queue.  At any point, at most
     * {@code nThreads} threads will be active processing tasks.
     * If additional tasks are submitted when all threads are active,
     * they will wait in the queue until a thread is available.
     * If any thread terminates due to a failure during execution
     * prior to shutdown, a new one will take its place if needed to
     * execute subsequent tasks.  The threads in the pool will exist
     * until it is explicitly {@link ExecutorService#shutdown shutdown}.
     *
     * @param nThreads the number of threads in the pool
     * @return the newly created thread pool
     * @throws IllegalArgumentException if {@code nThreads <= 0}
     */
    public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
    }
    ```
    - Executors.newCachedThreadPool()
        - 全是非核心线程，最大线程数Interget.Max_Value,60秒超时机制
        - 无法存储任务，有新任务立即执行
        - 适合执行大量耗时较少的任务（Retrofit,Rxjava）
    ```
    /**
     * Creates a thread pool that creates new threads as needed, but
     * will reuse previously constructed threads when they are
     * available.  These pools will typically improve the performance
     * of programs that execute many short-lived asynchronous tasks.
     * Calls to {@code execute} will reuse previously constructed
     * threads if available. If no existing thread is available, a new
     * thread will be created and added to the pool. Threads that have
     * not been used for sixty seconds are terminated and removed from
     * the cache. Thus, a pool that remains idle for long enough will
     * not consume any resources. Note that pools with similar
     * properties but different details (for example, timeout parameters)
     * may be created using {@link ThreadPoolExecutor} constructors.
     *
     * @return the newly created thread pool
     */
    public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
    }
    ```
    - Executors.newScheduledThreadPool()
        - 核心线程固定，最大线程Inter.Max_Value
        - 超时时间为0，非核心线程闲置会被立即回收
        - 适合定时任务，具有周期性的任务
    ```
    /**
     * Creates a new {@code ScheduledThreadPoolExecutor} with the
     * given core pool size.
     *
     * @param corePoolSize the number of threads to keep in the pool, even
     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set
     * @throws IllegalArgumentException if {@code corePoolSize < 0}
     */
    public ScheduledThreadPoolExecutor(int corePoolSize) {
        super(corePoolSize, Integer.MAX_VALUE,
              DEFAULT_KEEPALIVE_MILLIS, MILLISECONDS,
              new DelayedWorkQueue());
    }
    ```
    - Executor.SingleThreadExecutor()
        - 只有一个核心线程
        - 单线程的任务
    ```
    /**
     * Creates an Executor that uses a single worker thread operating
     * off an unbounded queue. (Note however that if this single
     * thread terminates due to a failure during execution prior to
     * shutdown, a new one will take its place if needed to execute
     * subsequent tasks.)  Tasks are guaranteed to execute
     * sequentially, and no more than one task will be active at any
     * given time. Unlike the otherwise equivalent
     * {@code newFixedThreadPool(1)} the returned executor is
     * guaranteed not to be reconfigurable to use additional threads.
     *
     * @return the newly created single-threaded Executor
     */
    public static ExecutorService newSingleThreadExecutor() {
        return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                    0L, TimeUnit.MILLISECONDS,
                                    new LinkedBlockingQueue<Runnable>()));
    }
    ```
- Executors 返回的线程池对象的弊端如下(摘自《阿里巴巴 Android 开发手册》(1.0.0)):
    - FixedThreadPool 和 SingleThreadPool:允许的请求队列(LinkedBlockingQueue)长度为Integer.MAX_VALUE，可能会堆积大量的请求，从而导致 OOM;   
    - CachedThreadPool 和 ScheduledThreadPool:允许的创建线程数量为Integer.MAX_VALUE，可能会创建大量的线程，从而导致OOM。
- 因此不建议使用Executor来构建线程池，而应该用
ThreadPoolExcutor来够构造线程池；
```
int NUMBER_OF_CORES = Runtime.getRuntime().availableProcessors();
int KEEP_ALIVE_TIME = 1;
TimeUnit KEEP_ALIVE_TIME_UNIT = TimeUnit.SECONDS; BlockingQueue<Runnable> taskQueue = new LinkedBlockingQueue<Runnable>(); ExecutorService executorService = new ThreadPoolExecutor(NUMBER_OF_CORES, NUMBER_OF_CORES*2, KEEP_ALIVE_TIME, KEEP_ALIVE_TIME_UNIT,
taskQueue, new BackgroundThreadFactory(), new DefaultRejectedExecutionHandler());
```