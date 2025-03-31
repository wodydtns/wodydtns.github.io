---
title: "[알고리즘] Priority Queue 학습하기"
date: 2025-03-31
categories: [Algorithm, 코딩테스트]
tags: [자바, 코딩테스트, 자료구조]
toc: true
published: true
---

# 우선순위 큐 알아보기
## 우선순위 큐의 정의
- 일반 Queue와 달리 요소들이 우선순위에 따라 정렬되는 자료구조
- 일반 Queue가 FIFO 방식으로 동작하는 반면, 우선순위 큐는 가장 높은(혹은 낮은) 우선순위를 가진 요소가 먼저 제거됨

## 우선순위 큐의 특징
- 우선순위 기반 접근 : 요소들이 우선순위에 따라 정렬
- 동적 정렬 : 새로운 요소가 추가될 때마다 자동으로 정렬
- 효율적인 연산 : 최댓값/최솟값을 O(1) 시간에 접근 가능
- 삽입/삭제 시간: O(log n) 시간 복잡도로 요소를 삽입하거나 제거 가능

## 우선순위 큐의 주요 메서드
# 우선순위 큐의 주요 메서드

| 메서드 | 설명 | 시간 복잡도 |
|--------|------|------------|
| `offer(E e)` | 요소를 큐에 추가 | O(log n) |
| `add(E e)` | 요소를 큐에 추가 (용량 제한 있을 경우 예외 발생) | O(log n) |
| `poll()` | 가장 높은 우선순위의 요소를 제거하고 반환 | O(log n) |
| `remove()` | 가장 높은 우선순위의 요소를 제거하고 반환 (큐가 비어있을 경우 예외 발생) | O(log n) |
| `peek()` | 가장 높은 우선순위의 요소를 반환 (제거하지 않음) | O(1) |
| `element()` | 가장 높은 우선순위의 요소를 반환 (큐가 비어있을 경우 예외 발생) | O(1) |
| `size()` | 큐의 크기 반환 | O(1) |
| `isEmpty()` | 큐가 비어있는지 확인 | O(1) |
| `clear()` | 큐의 모든 요소 제거 | O(n) |

## Java에서의 PriorityQueue 구현

```java
import java.util.PriorityQueue;

public class PriorityQueueExample {
    public static void main(String[] args) {
        // 기본 최소 힙(MinHeap) 생성
        // 최소 힙이 기본
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        
        // 요소 추가
        minHeap.offer(5);
        minHeap.offer(2);
        minHeap.offer(8);
        minHeap.offer(1);
        
        // 요소 출력 (자동으로 오름차순 정렬됨)
        while (!minHeap.isEmpty()) {
            System.out.println(minHeap.poll()); // 1, 2, 5, 8 순으로 출력
        }
    }
}

```

## 최대 힙 구현하기
```java
import java.util.PriorityQueue;
import java.util.Collections;

public class MaxHeapExample {
    public static void main(String[] args) {
        // 최대 힙(MaxHeap) 생성
        // collections.reverseOrder() 사용
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
        
        // 요소 추가
        maxHeap.offer(5);
        maxHeap.offer(2);
        maxHeap.offer(8);
        maxHeap.offer(1);
        
        // 요소 출력 (자동으로 내림차순 정렬됨)
        while (!maxHeap.isEmpty()) {
            System.out.println(maxHeap.poll()); // 8, 5, 2, 1 순으로 출력
        }
    }
}

```

## 객체에 대한 우선순위 큐
- 객체에 대한 우선순위 큐 구현 시  해당 객체가 Comparable 인터페이스를 구현하거나 Comparator를 제공해야함

### Comparable 인터페이스 구현
```java
class Person implements Comparable<Person> {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    @Override
    public int compareTo(Person other) {
        // 나이를 기준으로 오름차순 정렬
        return this.age - other.age;
    }
    
    @Override
    public String toString() {
        return name + " (" + age + ")";
    }
}

public class PersonPriorityQueueExample {
    public static void main(String[] args) {
        PriorityQueue<Person> personQueue = new PriorityQueue<>();
        
        personQueue.offer(new Person("Alice", 25));
        personQueue.offer(new Person("Bob", 20));
        personQueue.offer(new Person("Charlie", 30));
        
        while (!personQueue.isEmpty()) {
            System.out.println(personQueue.poll()); // 나이 순으로 출력
        }
    }
}

```

### Comparator 사용하기
```java
import java.util.Comparator;
import java.util.PriorityQueue;

class Student {
    private String name;
    private double gpa;
    
    public Student(String name, double gpa) {
        this.name = name;
        this.gpa = gpa;
    }
    
    public double getGpa() {
        return gpa;
    }
    
    @Override
    public String toString() {
        return name + " (GPA: " + gpa + ")";
    }
}

public class ComparatorExample {
    public static void main(String[] args) {
        // GPA 기준 내림차순 정렬을 위한 Comparator
        Comparator<Student> byGpaDesc = (s1, s2) -> Double.compare(s2.getGpa(), s1.getGpa());
        
        PriorityQueue<Student> studentQueue = new PriorityQueue<>(byGpaDesc);
        
        studentQueue.offer(new Student("Alice", 3.8));
        studentQueue.offer(new Student("Bob", 4.0));
        studentQueue.offer(new Student("Charlie", 3.5));
        
        while (!studentQueue.isEmpty()) {
            System.out.println(studentQueue.poll()); // GPA 높은 순으로 출력
        }
    }
}
```

## 우선순위 큐의 활용 사례
- 다익스트라 알고리즘(Dijkstra's Algorithm) : 최단 경로를 찾는 알고리즘에서 방문할 다음 노드를 결정할 때 사용
- 프림 알고리즘(Prim's Algorithm): 최소 신장 트리를 구성할 때 사용
- 허프만 코딩(Huffman Coding): 데이터 압축 알고리즘에서 사용
- 이벤트 시뮬레이션: 우선순위가 높은 이벤트부터 처리하는 시뮬레이션
- 작업 스케줄링: 우선순위가 높은 작업부터 처리하는 스케줄러
- 중앙값 찾기: 두 개의 우선순위 큐를 사용하여 스트림의 중앙값을 효율적으로 찾을 수 있음