---
title: "[neetcode] Counting Bits"
date: 2025-04-13
categories: [Algorithm, 코딩테스트]
tags: [자바, 코딩테스트, 문제 기록]
toc: true
published: true
---

## 난이도 
- Easy

## 문제 설명
> 정수 n이 주어지면, 범위 [0, n]의 모든 숫자의 이진 표현에서 1의 개수를 세어 반환합니다.
> 
> 배열 output을 반환하세요. 여기서 output[i]는 i의 이진 표현에서 1의 개수입니다.

### 입력 형식
- 정수 n

### 출력 형식
- 정수 배열

### 예제 입력 및 출력
```plaintext
**예제 1:**
입력: n = 4
출력: [0,1,1,2,1]
설명:
0 --> 0 (0개의 1)
1 --> 1 (1개의 1)
2 --> 10 (1개의 1)
3 --> 11 (2개의 1)
4 --> 100 (1개의 1)
```

# 접근 방법
- bitCount를 사용해 1의 숫자 개수를 가져오고 결과 배열에 반환

# 시간 및 공간 복잡도
- 시간 복잡도: O(n) - 모든 정수 숫자를 순회해야함
- 공간 복잡도 : O(1) - 해시맵을 사용하여 최대 n개의 요소를 저장해야 함

## 코드 구현
```java
class Solution {
    public int[] countBits(int n) {
        int[] answer = new int[n + 1];
        for(int i = 0; i <= n;i++){
            int bitCount = Integer.bitCount(i);
            answer[i] = bitCount;
        }
        return answer;
    }
}
```