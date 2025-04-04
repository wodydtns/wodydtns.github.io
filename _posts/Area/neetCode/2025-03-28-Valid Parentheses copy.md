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
> 다음 문자들로 구성된 문자열 s가 주어집니다: '(', ')', '{', '}', '[', ']'.
> 입력 문자열 s는 다음 조건을 모두 만족할 때만 유효합니다:
> 모든 열린 괄호는 같은 유형의 닫힌 괄호로 닫혀야 합니다.
> 열린 괄호는 올바른 순서로 닫혀야 합니다.
> 모든 닫힌 괄호는 같은 유형의 열린 괄호와 대응되어야 합니다.
> s가 유효한 문자열이면 true를, 그렇지 않으면 false를 반환하세요.

### 입력 형식
- 문자열

### 출력 형식
- 유효한 문자열이면 true, 아니면 false

### 예제 입력 및 출력
```plaintext
**예제 1:**
입력 : s = "[]"
출력 : true
```

**예제 2:**
```plaintext
입력 : s = "([{}])"
출력 : true
```


**예제 3:**
```plaintext
입력 : s = "[(])"
출력 : false
```

# 접근 방법
- 스택 자료 구조 사용
- 반복문을 통해 '(', '[', '{' 일 경우 stack에 추가, 반대일 경우 제거
- stack 에 값이 비어있을 경우 false

# 시간 및 공간 복잡도
- 시간 복잡도: O(n) - 각 문자를 한 번씩만 처리
- 공간 복잡도: O(n) - 모두 열린 괄호면 stack 에 모두 저장

## 코드 구현

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> isValid = new Stack<>();
        for(char inputChar : s.toCharArray()){
            if(inputChar == '(' || inputChar == '{'
            || inputChar == '['){
                isValid.push(inputChar);
            }else{
                if(isValid.size() == 0){
                    return false;
                }
                char compareChar = isValid.pop();
                if( (inputChar == ')' && compareChar != '(') ||
                    (inputChar == '}' && compareChar != '{') ||
                    (inputChar == ']' && compareChar != '[')
                ) {
                    return false;
                }
            }
        }
        return true;
    }
}
```