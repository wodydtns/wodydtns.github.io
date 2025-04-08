---
title: "[neetcode] Plus One"
date: 2025-04-09
categories: [Algorithm, 코딩테스트]
tags: [자바, 코딩테스트, 문제 기록]
toc: true
published: true
---


## 난이도 
- Easy
## 문제 설명
> 

### 입력 형식
- 정수 배열

### 출력 형식
- 정수

### 예제 입력 및 출력
```plaintext
**예제 1:**
출력 : nums = [1,2,3]
입력 : 0
```

**예제 2:**
```plaintext
출력 : nums = [0,2]
출력: 1
```

# 접근 방법
- 숫자 정렬 한 후 index값과 배열의 값이 동일하지 않으면 해당 index값을 반환
- 모든것이 있다면 길이값 반환

# 시간 및 공간 복잡도
- 시간 복잡도: O(nlogn) - 모든 배열을 한번 순회해야함
- 공간 복잡도: O(1), O(n) - 정렬에 따라 달라짐

## 코드 구현
```java
class Solution {
    public int missingNumber(int[] nums) {
        Arrays.sort(nums);
        for(int i = 0; i < nums.length; i++){
            if(nums[i] != i){
                return i;
            }
        }
        return  nums.length;
    }
}


```

## 더 좋은 방법
- HashSet을 사용해서 시간 복잡도 낮축
```java

```