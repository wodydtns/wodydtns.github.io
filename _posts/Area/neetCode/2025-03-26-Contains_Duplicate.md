---
title: "[neetcode]중복 포함 여부 확인 문제"
date: 2025-03-26
categories: [Algorithm, 코딩테스트]
tags: [자바, 코딩테스트, 문제 기록]
toc: true
published: true
---
# 중복 포함 여부 확인 (Contains Duplicate)

## 문제 설명
> 정수 배열 nums가 주어졌을 때, 배열에 어떤 값이라도 두 번 이상 나타나면 true를, 그렇지 않으면 false를 반환하세요.

### 입력 형식
- 정수 배열

### 출력 형식
- 중복값이 있으면 true, 없으면 false

### 예제 입력 및 출력
```plaintext
**예제 1:**
입력: nums = [1, 2, 3, 3]
출력: true
```

**예제 2:**
```plaintext
입력: nums = [1, 2, 3, 4]
출력: false
```

# 접근 방법
- 배열에서 중복값을 제거하기 위한 자료구조 사용(set)
- set에 담긴 element와 배열에 담긴 element의 길이 비교

# 시간 및 공간 복잡도
- 시간 복잡도: O(n) - 배열의 모든 요소를 한 번씩 순회합니다.
- 공간 복잡도: O(n) - 최악의 경우 모든 요소가 HashSet에 저장됩니다.

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