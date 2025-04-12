---
title: "[neetcode] Two Integer Sum II"
date: 2025-04-10
categories: [Algorithm, 코딩테스트]
tags: [자바, 코딩테스트, 문제 기록]
toc: true
published: true
---

## 난이도 
- Medium
## 문제 설명
> 비내림차순으로 정렬된 정수 배열 numbers가 주어집니다.
> 두 숫자의 인덱스 [index1, index2]를 반환하세요(1부터 시작하는 인덱스). 이 두 숫자의 합이 주어진 목표 숫자 target과 같아야 하며, index1 < index2여야 합니다. 참고로 index1과 index2는 같을 수 없으므로 같은 요소를 두 번 사용할 수 없습니다.
> 항상 정확히 하나의 유효한 해결책이 존재합니다.
> 해결책은 O(1)의 추가 공간을 사용해야 합니다.

### 입력 형식
- 정수 배열

### 출력 형식
- 정수 배열

### 예제 입력 및 출력
```plaintext
**예제 1:**
출력 : numbers = [1,2,3,4], target = 3
입력 : [1,2]
```

# 접근 방법
- 투 포인터를 사용하고(반드시 해결책이 있으므로), 두 수의 값이 target과 동일하면, left + 1, right + 1 을 가진 배열을 반환한다
- 두 수의 합이 target 보다 크면, right 값을 1 뺀다
- 두 수의 합이 target 보다 작으면, left 값을 1 더한다

# 시간 및 공간 복잡도
- 시간 복잡도: O(n) - 모든 배열을 한번 순회해야함
- 공간 복잡도: O(1) - 추가 공간을 사용하지 않고, 포인터만 사용

```java

class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0;
        int right = numbers.length - 1;
        while(left < right){
            int sumValue = numbers[left] + numbers[right];
            if(sumValue > target){
                right--;
            } else if(sumValue < target){
                left++;
            } else{
                return new int[]{left +1, right+1};
            }
        }
        return new int[0];
    }
}
```