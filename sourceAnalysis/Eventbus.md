## Eventbus（基于3.1.1源码）
- EventBus使用了观察者模式。
- register(Object subscribe)方法逻辑
- 在运行时，在注册方法下，默认使用反射来找到使用订阅方法信息的集合，（查找过程会有一个concurrenthashmap(**METHOD_CACHE**)来缓存注册Eventbus的类和订阅方法的映射关系），接着在同步代码块内，遍历前面订阅方法集合，一一订阅。
- 订阅过程是，收集注册类和订阅方法对应的映射关系（**typesBySubscriber**），再保存以订阅事件的类型为key,注册类和订阅方法绑定关系的集合为value的hashmap(**subscriptionsByEventType**).
- post（Object event）方法逻辑
```
/** Posts the given event to the event bus. */
    public void post(Object event) {
        PostingThreadState postingState = currentPostingThreadState.get();
        List<Object> eventQueue = postingState.eventQueue;
        //把事件添加到postingState的队列里
        eventQueue.add(event);

        if (!postingState.isPosting) {
            postingState.isMainThread = isMainThread();
            postingState.isPosting = true;
            if (postingState.canceled) {
                throw new EventBusException("Internal error. Abort state was not reset");
            }
            try {
                while (!eventQueue.isEmpty()) {
                    //遍历队列一一发送
                    postSingleEvent(eventQueue.remove(0), postingState);
                }
            } finally {
                postingState.isPosting = false;
                postingState.isMainThread = false;
            }
        }
    }
    ```
    - 接着看postSingleEvent(..)
    ```
    private void postSingleEvent(Object event, PostingThreadState postingState) throws Error {
        Class<?> eventClass = event.getClass();
        boolean subscriptionFound = false;
        if (eventInheritance) {
            List<Class<?>> eventTypes = lookupAllEventTypes(eventClass);
            int countTypes = eventTypes.size();
            for (int h = 0; h < countTypes; h++) {
                Class<?> clazz = eventTypes.get(h);
                //postSingleEventForEventType（..）发送每一个事件
                subscriptionFound |= postSingleEventForEventType(event, postingState, clazz);
            }
        } else {
            subscriptionFound = postSingleEventForEventType(event, postingState, eventClass);
        }
        if (!subscriptionFound) {
            if (logNoSubscriberMessages) {
                logger.log(Level.FINE, "No subscribers registered for event " + eventClass);
            }
            if (sendNoSubscriberEvent && eventClass != NoSubscriberEvent.class &&
                    eventClass != SubscriberExceptionEvent.class) {
                post(new NoSubscriberEvent(this, event));
            }
        }
    }
    ```
    再看postSingleEventForEventType（...）
    ```
    private boolean postSingleEventForEventType(Object event, PostingThreadState postingState, Class<?> eventClass) {
        CopyOnWriteArrayList<Subscription> subscriptions;
        synchronized (this) {
            //根据register(..)中收集到的事件类型和订阅信息的映射关系，拿到需要的订阅方法相关集合
            subscriptions = subscriptionsByEventType.get(eventClass);
        }
        if (subscriptions != null && !subscriptions.isEmpty()) {
            for (Subscription subscription : subscriptions) {
                postingState.event = event;
                postingState.subscription = subscription;
                boolean aborted = false;
                try {
                    //准备一一处理订阅方法
                    postToSubscription(subscription, event, postingState.isMainThread);
                    aborted = postingState.canceled;
                } finally {
                    postingState.event = null;
                    postingState.subscription = null;
                    postingState.canceled = false;
                }
                if (aborted) {
                    break;
                }
            }
            return true;
        }
        return false;
    }

```
- 处理事件的订阅方法的逻辑，根据线程模式来处理，但是无论如何都将调用void invokeSubscriber(Subscription subscription, Object event)
```
     private void postToSubscription(Subscription subscription, Object event, boolean isMainThread) {
        switch (subscription.subscriberMethod.threadMode) {
            case POSTING:
                invokeSubscriber(subscription, event);
                break;
            case MAIN:
                if (isMainThread) {
                    invokeSubscriber(subscription, event);
                } else {
                    mainThreadPoster.enqueue(subscription, event);
                }
                break;
            case MAIN_ORDERED:
                if (mainThreadPoster != null) {
                    //在HandlerPoster中切换到主线程处理
                    mainThreadPoster.enqueue(subscription, event);
                } else {
                    // temporary: technically not correct as poster not decoupled from subscriber
                    invokeSubscriber(subscription, event);
                }
                break;
            case BACKGROUND:
                if (isMainThread) {
                    //在BackgroundPoster调用线程池处理
                    backgroundPoster.enqueue(subscription, event);
                } else {
                    invokeSubscriber(subscription, event);
                }
                break;
            case ASYNC:
                //在AsyncPoster调用线程池处理，和BACKGROUND的区别在于ASYNC总是新开线程处理，而前者是在主线程的时候会新开线程处理事件。
                asyncPoster.enqueue(subscription, event);
                break;
            default:
                throw new IllegalStateException("Unknown thread mode: " + subscription.subscriberMethod.threadMode);
        }
    }
```
- 最后通过反射吊起订阅类的方法，并传入事件类的实例。
```
     void invokeSubscriber(Subscription subscription, Object event) {
        try {
            subscription.subscriberMethod.method.invoke(subscription.subscriber, event);
        } catch (InvocationTargetException e) {
            handleSubscriberException(subscription, event, e.getCause());
        } catch (IllegalAccessException e) {
            throw new IllegalStateException("Unexpected exception", e);
        }
    }
```
- unregister(Object subscribe)
        - 通过订阅类和事件类型的集合(**typesBySubscriber**)，找到订阅者对应的事件类型；再通过(**subscriptionsByEventType**)事件类型拿到订阅者和订阅方法的集合(**typesBySubcriber**)，一一解除订阅。
- 通过反射找到订阅信息这种方式，在大量使用EventBus的情况下，会有效率问题；也可以在编译期间，通过注解处理器（annotationProcessor）来生成辅助类，保存订阅方法的相关信息，类似ButterKnife，Arouter的做法。
- 粘性事件是可以先发布事件，后面需要的时候在再注册和订阅；在注册流程中，会自行通知订阅方法。可以用到一些缓存场景。[推荐阅读](http://greenrobot.org/eventbus/documentation/configuration/sticky-events/)
- [可以阅读这边源码分析文章](https://juejin.im/post/5ae2e6dcf265da0b9d77f28e)