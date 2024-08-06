## 동시성을 다루는 클린 코드 작성법

### 목차
1. 서론
2. 동시성이 필요한 이유
3. 미신과 오해
4. 난관
5. 동시성 방어 원칙
   - 단일 책임 원칙 (Single Responsibility Principle, SRP)
   - 따름 정리: 자료 범위를 제한하라
   - 따름 정리: 자료 사본을 사용하라
   - 따름 정리: 스레드는 가능한 독립적으로 구현하라
6. 라이브러리를 이해하라
   - 스레드 환경에 안전한 컬렉션
   - 실행자 프레임워크 (Executor Framework)
7. 실행 모델을 이해하라
   - 생산자-소비자 (Producer-Consumer)
   - 읽기-쓰기 (Readers-Writers)
   - 식사하는 철학자들 (Dining Philosophers)
8. 동기화하는 메서드 사이에 존재하는 의존성을 이해하라
   - 동기화하는 부분을 작게 만들어라
   - 올바른 종료 코드는 구현하기 어렵다
9. 스레드 코드 테스트하기
   - 말이 안 되는 실패는 잠정적인 스레드 문제로 취급하라
   - 다중 스레드를 고려하지 않은 순차 코드부터 제대로 돌게 만들자
   - 다중 스레드를 쓰는 코드 부분을 다양한 환경에 쉽게 끼워 넣을 수 있게 스레드 코드를 구현하라
   - 다중 스레드를 쓰는 코드 부분을 상황에 맞게 조율할 수 있게 작성하라
   - 프로세서 수보다 많은 스레드를 돌려보라
   - 다른 플랫폼에서 돌려보라
   - 코드에 보조 코드(instrument)를 넣어 돌려라. 강제로 실패를 일으키게 해보라
10. 결론

---

### 1. 서론
동시성 프로그래밍은 현대 애플리케이션에서 필수적이며, 멀티코어 프로세서와 병렬 처리를 통해 성능을 향상시킬 수 있습니다. 하지만 동시성 문제는 복잡한 문제를 동반하며, 이를 잘 관리하기 위해 클린 코드 작성이 중요합니다.

### 2. 동시성이 필요한 이유
동시성은 여러 작업을 동시에 처리할 수 있도록 하여 성능과 자원 활용도를 극대화합니다. 예를 들어, 웹 서버는 여러 클라이언트 요청을 동시에 처리하고, 데이터베이스는 복잡한 쿼리를 효율적으로 수행할 수 있어야 합니다.

### 3. 미신과 오해
- **미신:** "스레드가 많으면 성능이 무조건 향상된다."
- **오해:** "동시성 문제를 완벽하게 피할 수 있다."
이러한 미신은 동시성 문제를 잘못 이해하게 하며, 성능 저하와 유지보수 문제를 초래할 수 있습니다.

### 4. 난관
동시성 프로그래밍에서 직면할 수 있는 난관에는 경쟁 조건, 데드락, 라이브락 등이 있습니다. 이러한 문제들은 디버깅을 어렵게 만들며, 시스템의 신뢰성을 저하시킬 수 있습니다.

### 5. 동시성 방어 원칙

- **단일 책임 원칙 (Single Responsibility Principle, SRP)**
  - 동시성 코드와 비동시성 코드를 분리하여 유지보수성과 가독성을 높입니다.
  - **예제 (자바):**
    ```java
    public class Task {
        public void execute() {
            // 비동기 작업 실행
        }
    }
    
    public class ConcurrentTask extends Task {
        public void execute() {
            // 동시성 문제를 처리하는 작업 실행
        }
    }
    ```

- **따름 정리: 자료 범위를 제한하라**
  - 자료 범위를 최소화하여 동시성 문제를 줄입니다.
  - **예제 (자바):**
    ```java
    public class DataProcessor {
        private final List<Integer> data = new ArrayList<>();
        
        public void addData(int value) {
            synchronized (data) {
                data.add(value);
            }
        }
        
        public List<Integer> getData() {
            synchronized (data) {
                return new ArrayList<>(data);
            }
        }
    }
    ```

- **따름 정리: 자료 사본을 사용하라**
  - 공유 자료의 사본을 사용하여 동시 수정 문제를 피합니다.
  - **예제 (자바):**
    ```java
    public final class ImmutableData {
        private final int value;

        public ImmutableData(int value) {
            this.value = value;
        }

        public int getValue() {
            return value;
        }
    }
    ```

- **따름 정리: 스레드는 가능한 독립적으로 구현하라**
  - 스레드가 서로 독립적으로 동작하도록 구현합니다.
  - **예제 (자바스크립트):**
    ```javascript
    function fetchData(url) {
        return fetch(url).then(response => response.json());
    }

    async function processData() {
        const data1 = await fetchData('https://api.example.com/data1');
        const data2 = await fetchData('https://api.example.com/data2');
        // 독립적으로 데이터 처리
    }
    ```

### 6. 라이브러리를 이해하라

- **스레드 환경에 안전한 컬렉션**
  - 자바의 `java.util.concurrent` 패키지는 스레드 안전한 컬렉션을 제공합니다.
  - **예제 (자바):**
    ```java
    import java.util.concurrent.ConcurrentHashMap;
    
    public class SafeCollectionExample {
        private final ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
        
        public void add(String key, Integer value) {
            map.put(key, value);
        }
        
        public Integer get(String key) {
            return map.get(key);
        }
    }
    ```

- **실행자 프레임워크 (Executor Framework)**
  - 자바의 `ExecutorService`는 스레드 풀을 관리하고 비동기 작업을 실행합니다.
  - **예제 (자바):**
    ```java
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    
    public class ExecutorServiceExample {
        public static void main(String[] args) {
            ExecutorService executor = Executors.newFixedThreadPool(4);
            
            Runnable task = () -> {
                System.out.println("Task executed by " + Thread.currentThread().getName());
            };
            
            for (int i = 0; i < 10; i++) {
                executor.submit(task);
            }
            
            executor.shutdown();
        }
    }
    ```

### 7. 실행 모델을 이해하라

- **생산자-소비자 (Producer-Consumer)**
  - 생산자와 소비자가 큐를 통해 데이터를 주고받는 패턴입니다.
  - **예제 (자바):**
    ```java
    import java.util.concurrent.ArrayBlockingQueue;
    import java.util.concurrent.BlockingQueue;

    public class ProducerConsumerExample {
        private static final int CAPACITY = 10;
        private final BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(CAPACITY);
        
        class Producer implements Runnable {
            public void run() {
                try {
                    for (int i = 0; i < 100; i++) {
                        queue.put(i);
                        System.out.println("Produced " + i);
                    }
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        }
        
        class Consumer implements Runnable {
            public void run() {
                try {
                    while (true) {
                        Integer value = queue.take();
                        System.out.println("Consumed " + value);
                    }
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        }

        public static void main(String[] args) {
            ProducerConsumerExample example = new ProducerConsumerExample();
            
            Thread producer = new Thread(example.new Producer());
            Thread consumer = new Thread(example.new Consumer());
            
            producer.start();
            consumer.start();
        }
    }
    ```

- **읽기-쓰기 (Readers-Writers)**
  - 읽기 작업과 쓰기 작업이 공존할 수 있도록 하는 패턴입니다.
  - **예제 (자바):**
    ```java
    import java.util.concurrent.locks.ReadWriteLock;
    import java.util.concurrent.locks.ReentrantReadWriteLock;

    public class ReadersWritersExample {
        private final ReadWriteLock lock = new ReentrantReadWriteLock();
        private int value = 0;

        public void write(int newValue) {
            lock.writeLock().lock();
            try {
                value = newValue;
            } finally {
                lock.writeLock().unlock();
            }
        }

        public int read() {
            lock.readLock().lock();
            try {
                return value;
            } finally {
                lock.readLock().unlock();
            }
        }
    }
    ```

- **식사하는 철학자들 (Dining Philosophers)**
  - 철학자들이 동시에 식사할 수 있도록 하는 문제 해결 패턴입니다.
  - **예제 (자바):**
    ```java
    import java.util.concurrent.locks.Lock;
    import java.util.concurrent.locks.ReentrantLock;

    public class DiningPhilosophers {
        private static final int PHILOSOPH

ERS_COUNT = 5;
        private final Lock[] forks = new Lock[PHILOSOPHERS_COUNT];

        public DiningPhilosophers() {
            for (int i = 0; i < PHILOSOPHERS_COUNT; i++) {
                forks[i] = new ReentrantLock();
            }
        }

        public void dine(int philosopherIndex) {
            int leftFork = philosopherIndex;
            int rightFork = (philosopherIndex + 1) % PHILOSOPHERS_COUNT;
            
            forks[leftFork].lock();
            try {
                forks[rightFork].lock();
                try {
                    // Eating
                } finally {
                    forks[rightFork].unlock();
                }
            } finally {
                forks[leftFork].unlock();
            }
        }
    }
    ```

### 8. 동기화하는 메서드 사이에 존재하는 의존성을 이해하라

- **동기화하는 부분을 작게 만들어라**
  - 동기화 블록을 최소화하여 성능을 향상시킵니다.
  - **예제 (자바):**
    ```java
    public class SmallSyncBlock {
        private final Object lock = new Object();
        
        public void performTask() {
            synchronized (lock) {
                // 최소화된 동기화 블록
            }
            // 동기화 블록 밖에서 다른 작업 수행
        }
    }
    ```

- **올바른 종료 코드는 구현하기 어렵다**
  - 스레드 종료는 복잡하며, 이를 신중하게 설계해야 합니다.
  - **예제 (자바):**
    ```java
    public class GracefulShutdown {
        private volatile boolean running = true;
        
        class Worker extends Thread {
            public void run() {
                while (running) {
                    // 작업 수행
                }
            }
        }
        
        public void shutdown() {
            running = false;
        }
    }
    ```

### 9. 스레드 코드 테스트하기

- **말이 안 되는 실패는 잠정적인 스레드 문제로 취급하라**
  - 스레드 문제는 예기치 않은 실패로 나타날 수 있으며, 이를 잠정적으로 스레드 문제로 고려합니다.

- **다중 스레드를 고려하지 않은 순차 코드부터 제대로 돌게 만들자**
  - 순차 코드에서 문제를 해결한 후, 동시성 문제를 추가하여 디버깅합니다.

- **다중 스레드를 쓰는 코드 부분을 다양한 환경에 쉽게 끼워 넣을 수 있게 스레드 코드를 구현하라**
  - 다양한 테스트 환경에서 동작할 수 있도록 스레드 코드를 유연하게 작성합니다.

- **다중 스레드를 쓰는 코드 부분을 상황에 맞게 조율할 수 있게 작성하라**
  - 코드가 다양한 상황에 맞게 조정될 수 있도록 구현합니다.

- **프로세서 수보다 많은 스레드를 돌려보라**
  - 성능을 측정하고 자원 활용을 최적화하기 위해 스레드를 프로세서 수보다 많이 실행하여 테스트합니다.

- **다른 플랫폼에서 돌려보라**
  - 코드가 다양한 플랫폼에서 동작하는지 테스트하여 이식성을 확인합니다.

- **코드에 보조 코드(instrument)를 넣어 돌려라. 강제로 실패를 일으키게 해보라**
  - 보조 코드를 추가하여 의도적으로 실패를 유도하고 문제를 진단합니다.

### 10. 결론
동시성 프로그래밍은 복잡하지만, 이를 잘 관리하면 시스템의 성능과 반응성을 크게 향상시킬 수 있습니다. 클린 코드를 작성하기 위해 동시성 방어 원칙을 준수하고, 신뢰할 수 있는 라이브러리와 패턴을 활용하며, 철저한 테스트와 튜닝을 수행하는 것이 중요합니다.

---

### 출처: 다른 분이 요약한 내용 + 유튜브 영상

- [Chapter 13 - 동시성.md](https://github.com/Yooii-Studios/Clean-Code/blob/master/Chapter%2013%20-%20%EB%8F%99%EC%8B%9C%EC%84%B1.md)
- [Clean Code: Chapter 13 (Part 1/2: Concurrency)](https://www.youtube.com/watch?v=f99zSz5D5RE)
