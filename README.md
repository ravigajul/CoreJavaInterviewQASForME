# CoreJavaInterviewQASForME
Got it! Here are some core Java coding questions from a QA/testing perspective:

### 1. **Reverse a String Without Using Built-in Methods**  
```java
public class ReverseString {
    public static String reverse(String str) {
        StringBuilder reversed = new StringBuilder();
        for (int i = str.length() - 1; i >= 0; i--) {
            reversed.append(str.charAt(i));
        }
        return reversed.toString();
    }

    public static void main(String[] args) {
        System.out.println(reverse("QA Testing"));
    }
}
```

### 2. **Find Duplicate Elements in an Array**
```java
import java.util.HashSet;

public class FindDuplicates {
    public static void findDuplicates(int[] arr) {
        HashSet<Integer> seen = new HashSet<>();
        for (int num : arr) {
            if (!seen.add(num)) {
                System.out.println("Duplicate found: " + num);
            }
        }
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 2, 5, 6, 3};
        findDuplicates(arr);
    }
}
```

### 3. **Check If a String is a Palindrome**
```java
public class PalindromeCheck {
    public static boolean isPalindrome(String str) {
        int left = 0, right = str.length() - 1;
        while (left < right) {
            if (str.charAt(left) != str.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }

    public static void main(String[] args) {
        System.out.println(isPalindrome("madam")); // true
        System.out.println(isPalindrome("testing")); // false
    }
}
```

### 4. **Find Missing Number in an Array of 1 to N**
```java
public class MissingNumber {
    public static int findMissing(int[] arr, int n) {
        int sum = n * (n + 1) / 2;
        int actualSum = 0;
        for (int num : arr) {
            actualSum += num;
        }
        return sum - actualSum;
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 5}; // missing 4
        System.out.println("Missing number: " + findMissing(arr, 5));
    }
}
```

### 5. **Find First Non-Repeated Character in a String**
```java
import java.util.LinkedHashMap;
import java.util.Map;

public class FirstNonRepeatedChar {
    public static char firstNonRepeated(String str) {
        Map<Character, Integer> charCount = new LinkedHashMap<>();
        for (char ch : str.toCharArray()) {
            charCount.put(ch, charCount.getOrDefault(ch, 0) + 1);
        }
        for (Map.Entry<Character, Integer> entry : charCount.entrySet()) {
            if (entry.getValue() == 1) {
                return entry.getKey();
            }
        }
        return '\0'; // return null char if no unique char is found
    }

    public static void main(String[] args) {
        System.out.println(firstNonRepeated("swiss")); // w
    }
}
```

### 6. **Find Factorial of a Number Using Recursion**
```java
public class Factorial {
    public static int factorial(int n) {
        if (n == 0 || n == 1) {
            return 1;
        }
        return n * factorial(n - 1);
    }

    public static void main(String[] args) {
        System.out.println(factorial(5)); // 120
    }
}
```

### 7. **Check if Two Strings are Anagrams**
```java
import java.util.Arrays;

public class AnagramCheck {
    public static boolean areAnagrams(String str1, String str2) {
        char[] arr1 = str1.toCharArray();
        char[] arr2 = str2.toCharArray();
        Arrays.sort(arr1);
        Arrays.sort(arr2);
        return Arrays.equals(arr1, arr2);
    }

    public static void main(String[] args) {
        System.out.println(areAnagrams("listen", "silent")); // true
        System.out.println(areAnagrams("hello", "world")); // false
    }
}
```
Advanced Java coding questions focusing on **multi-threading, collections, and other core Java topics** relevant for a **QA testing perspective**.

---

## **Multi-threading Questions**

### **1. Print Even and Odd Numbers Using Two Threads**
```java
class PrintNumbers {
    private int num = 1;
    private final int limit;
    
    public PrintNumbers(int limit) {
        this.limit = limit;
    }

    public synchronized void printOdd() {
        while (num < limit) {
            while (num % 2 == 0) {
                try {
                    wait();
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
            System.out.println(Thread.currentThread().getName() + " - " + num);
            num++;
            notify();
        }
    }

    public synchronized void printEven() {
        while (num < limit) {
            while (num % 2 != 0) {
                try {
                    wait();
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
            System.out.println(Thread.currentThread().getName() + " - " + num);
            num++;
            notify();
        }
    }

    public static void main(String[] args) {
        PrintNumbers pn = new PrintNumbers(10);

        Thread t1 = new Thread(pn::printOdd, "Odd-Thread");
        Thread t2 = new Thread(pn::printEven, "Even-Thread");

        t1.start();
        t2.start();
    }
}
```

---

### **2. Implement Thread Safety Using `ReentrantLock`**
```java
import java.util.concurrent.locks.ReentrantLock;

class Counter {
    private int count = 0;
    private final ReentrantLock lock = new ReentrantLock();

    public void increment() {
        lock.lock();
        try {
            count++;
        } finally {
            lock.unlock();
        }
    }

    public int getCount() {
        return count;
    }
}

public class ThreadSafeCounter {
    public static void main(String[] args) {
        Counter counter = new Counter();

        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        };

        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);

        t1.start();
        t2.start();

        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }

        System.out.println("Final Count: " + counter.getCount());
    }
}
```

---

## **Collections and Data Structures Questions**

### **3. Remove Duplicates from an ArrayList**
```java
import java.util.*;

public class RemoveDuplicates {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 3, 4, 5, 5, 6));
        Set<Integer> uniqueNumbers = new HashSet<>(numbers);
        System.out.println("Unique Elements: " + uniqueNumbers);
    }
}
```

---

### **4. Find the First Non-Repeating Character Using `LinkedHashMap`**
```java
import java.util.LinkedHashMap;
import java.util.Map;

public class FirstUniqueCharacter {
    public static char findFirstUnique(String str) {
        Map<Character, Integer> charCount = new LinkedHashMap<>();

        for (char c : str.toCharArray()) {
            charCount.put(c, charCount.getOrDefault(c, 0) + 1);
        }

        for (Map.Entry<Character, Integer> entry : charCount.entrySet()) {
            if (entry.getValue() == 1) {
                return entry.getKey();
            }
        }
        return '\0'; // No unique character
    }

    public static void main(String[] args) {
        System.out.println(findFirstUnique("swiss")); // Output: w
    }
}
```

---

### **5. Implement LRU Cache Using `LinkedHashMap`**
```java
import java.util.*;

class LRUCache<K, V> extends LinkedHashMap<K, V> {
    private final int capacity;

    public LRUCache(int capacity) {
        super(capacity, 0.75f, true);
        this.capacity = capacity;
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
        return size() > capacity;
    }

    public static void main(String[] args) {
        LRUCache<Integer, String> cache = new LRUCache<>(3);
        cache.put(1, "One");
        cache.put(2, "Two");
        cache.put(3, "Three");

        System.out.println("Cache before access: " + cache);
        cache.get(1);  // Access key 1
        cache.put(4, "Four"); // This will remove key 2 as it's least recently used

        System.out.println("Cache after access: " + cache);
    }
}
```

---

## **Other Core Java Questions for QA Perspective**

### **6. Count the Frequency of Words in a String**
```java
import java.util.*;

public class WordFrequency {
    public static void main(String[] args) {
        String text = "QA testing is fun and QA is important";

        Map<String, Integer> wordCount = new HashMap<>();
        String[] words = text.toLowerCase().split("\\s+");

        for (String word : words) {
            wordCount.put(word, wordCount.getOrDefault(word, 0) + 1);
        }

        System.out.println("Word Frequency: " + wordCount);
    }
}
```

---

### **7. Find the Intersection of Two Arrays**
```java
import java.util.*;

public class ArrayIntersection {
    public static int[] findIntersection(int[] arr1, int[] arr2) {
        Set<Integer> set = new HashSet<>();
        List<Integer> result = new ArrayList<>();

        for (int num : arr1) {
            set.add(num);
        }

        for (int num : arr2) {
            if (set.contains(num)) {
                result.add(num);
                set.remove(num);
            }
        }

        return result.stream().mapToInt(i -> i).toArray();
    }

    public static void main(String[] args) {
        int[] arr1 = {1, 2, 3, 4, 5};
        int[] arr2 = {3, 4, 5, 6, 7};

        System.out.println("Intersection: " + Arrays.toString(findIntersection(arr1, arr2)));
    }
}
```

---

### **8. Producer-Consumer Problem Using `BlockingQueue`**
```java
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;

class Producer implements Runnable {
    private BlockingQueue<Integer> queue;

    public Producer(BlockingQueue<Integer> queue) {
        this.queue = queue;
    }

    public void run() {
        try {
            for (int i = 1; i <= 5; i++) {
                queue.put(i);
                System.out.println("Produced: " + i);
                Thread.sleep(500);
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}

class Consumer implements Runnable {
    private BlockingQueue<Integer> queue;

    public Consumer(BlockingQueue<Integer> queue) {
        this.queue = queue;
    }

    public void run() {
        try {
            while (true) {
                Integer item = queue.take();
                System.out.println("Consumed: " + item);
                Thread.sleep(1000);
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}

public class ProducerConsumerDemo {
    public static void main(String[] args) {
        BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(5);

        Thread producer = new Thread(new Producer(queue));
        Thread consumer = new Thread(new Consumer(queue));

        producer.start();
        consumer.start();
    }
}
```
---
