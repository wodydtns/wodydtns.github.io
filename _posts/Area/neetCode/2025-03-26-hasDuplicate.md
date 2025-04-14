---
title: "[neetcode]이진탐색"
date: 2025-03-26
categories: [Algorithm, 코딩테스트]
tags: [자바, 코딩테스트, 문제 기록]
toc: true
published: true
---
# 중복 포함 여부 확인 (Contains Duplicate)

## 난이도 
- Easy
## 문제 설명
> 정렬된 고유 정수 배열과 목표값이 주어질 때 이진 탐색을 구현하세요  
> 오름차순으로 정렬된 서로 다른(중복 없는) 정수 배열 nums와 정수 target이 주어집니다.  
> nums 내에서 target을 검색하는 함수를 구현하세요. target이 존재하면 해당 인덱스를 반환하고, 그렇지 않으면 -1을 반환하세요.  
> 여러분의 해결책은 반드시 O(log n) 시간 복잡도로 실행되어야 합니다.

### 입력 형식
- 정수 배열

### 출력 형식
- target과 동일한 값이 있으면 index, 없으면 -1

### 예제 입력 및 출력
```plaintext
**예제 1:**
출력 : nums = [-1,0,2,4,6,8], target = 4
입력 : 3
```

**예제 2:**
```plaintext
출력 : nums = [-1,0,2,4,6,8], target = 3
출력: -1
```

# 접근 방법
- 이잔탐색 구현
- 정렬된 배열에서 중간값을 확인하고, target 값과 비교 후 탐색 범위를 절반씩 축소
- left , right pointer를 사용
- 중간값이 target보다 크면 right--;
- 중간값이 target보다 작으면 left++;
- 중간값이 target과 같으면 해당 인덱스 반환
- left pointer가 right pointer보다 크면 target이 없으므로 -1 반환

# 시간 및 공간 복잡도
- 시간 복잡도: O(log n) - 각 단계마다 탐색 범위가 절반으로 줄어듭니다.
- 공간 복잡도: O(1) - 추가적인 자료구조를 사용하지 않고 포인터만 사용합니다.

## 코드 구현

```java
class Solution {
    public boolean hasDuplicate(int[] nums) {
        Set<Integer> distinctSet = new HashSet<>();
        for(int num : nums){
            distinctSet.add(num);
        }
        return distinctSet.size() == nums.length ? false : true;
    }
}
```