## Java生产者消费者
生产者消费者问题是一个著名的线程同步问题，该问题描述如下：有一个生产者在生产产品，这些产品将提供给若干个消费者去消费，为了使生产者和消费者能并发执行，在两者之间设置一个具有多个缓冲区的缓冲池，生产者将它生产的产品放入一个缓冲区中，消费者可以从缓冲区中取走产品进行消费，显然生产者和消费者之间必须保持同步，即不允许消费者到一个空的缓冲区中取产品，也不允许生产者向一个已经放入产品的缓冲区中再次投放产品。

### lock-unlock(Condition)
```java
import java.util.PriorityQueue;
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Test {
    private static final int QUEUE_SIZE = 10;

    private PriorityQueue<Integer> queue = new PriorityQueue<Integer>(QUEUE_SIZE);
    
    private Lock lock = new ReentrantLock();

    private Condition consumerCon = lock.newCondition();
    private Condition producerCon = lock.newCondition();

    //消费者
    class Consumer extends Thread {

        @Override
        public void run() {
            while (true) {
                lock.lock();
                try {
                    if (queue.size() == 0) {
                        System.out.println("队列Empty，等待...");
                        consumerCon.await();
                    }
                    queue.poll();//每次移走队首元素
                    producerCon.signal();
                    System.out.println("Get数据，队列剩余" + queue.size());
                } catch (Exception e) {
                    e.printStackTrace();
                } finally {
                    lock.unlock();
                }

                try {
                    Thread.sleep((long) (1000 * Math.random()));
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    //生产者
    class Producer extends Thread {

        @Override
        public void run() {
            while (true) {
                lock.lock();
                try {
                    if (queue.size() == QUEUE_SIZE) {
                        System.out.println("队列Full，等待...");
                        producerCon.await();
                    }
                    queue.offer(1);//每次插入一个元素
                    consumerCon.signal();
                    System.out.println("Insert数据，队列剩余：" + queue.size());
                } catch (Exception e) {
                    e.printStackTrace();
                } finally {
                    lock.unlock();
                }

                try {
                    Thread.sleep((long) (1000 * Math.random()));
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    public static void main(String[] args) {
        Test test = new Test();
        Producer producer = test.new Producer();
        Consumer consumer = test.new Consumer();

        producer.start();
        consumer.start();
    }
}
```


### wait-notify

```java
    /*消费者 */
    public class Consumer implements Runnable {
        private Vector vector;

        public Consumer(Vector v) {
            this.vector = v;
        }

        public void run() {
            synchronized (vector) {
                while (true) {
                    try {
                        if (vector.size() == 0) {
                            System.out.println("消费者释放锁，等待中...");
                            vector.wait();
                        }
                        System.out.println("消费者消费了");
                        vector.clear();
                        vector.notify();
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }

    /*  生产者 */
    public class Producer implements Runnable {
        private Vector vector;

        public Producer(Vector v) {
            this.vector = v;
        }

        public void run() {
            synchronized (vector) {
                while (true) {
                    try {
                        if (vector.size() != 0) {
                            System.out.println("生产者释放锁，等待中...");
                            vector.wait();
                        }
                        System.out.println("生产者生产了");
                        vector.add(new String("apples"));
                        vector.notify();
                        Thread.sleep(500);
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }

    public void test() {
        Vector vector = new Vector();
        Thread consumer = new Thread(new Consumer(vector));
        Thread producer = new Thread(new Producer(vector));
        consumer.start();
        producer.start();
    }
```