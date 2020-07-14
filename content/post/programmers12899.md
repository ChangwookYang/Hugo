---
title: "프로그래머스 12899 - 124나라의숫자"
date: 2020-07-15T00:33:21+09:00
---

# 124 나라의 숫자

문제 설명  
[124나라의 숫자](https://programmers.co.kr/learn/courses/30/lessons/12899)
![124나라의숫자](https://user-images.githubusercontent.com/66955409/87445497-166b8b80-c633-11ea-94d2-7a84ac77b359.JPG)

### 나의 풀이
```Java
public String solution(int n) {
    String answer = "";
    
    while( n > 0 ){
        String addNum = "";
        if(n%3 == 0){
            addNum = "4"; 
            n = n/3-1;
        } else {
            addNum = String.valueOf(n%3);
            n = n/3;
        }
        answer = addNum + answer;
    }

    return answer;
}

```
나의 풀이라고 썻지만 사실은 타인의 풀이에 가깝다.  
진법 풀이의 응용문제인 듯하다.  
  
10진법을 3진법으로 바꾸는데 사실 1, 2는 그대로 내려오면되고  
3으로 나눌때 0이되면 4로 바꾸면 되는..  
여기서 타인의 풀이를 보고도 잘 이해 안되었던 부분은 아래와 같다.
```Java
n = n/3-1;
```

직접 계산해보니 9를 3진법으로 나누면 100이 된다.  
9 / 3 .. 0  
3 / 3 .. 0  
1  
  
3으로 나누어 떨어지면 자릿수가 늘어나게 된다..!!  
하여 -1을 빼니  
9 / 3 .. 0 -> 4  
2 / 3 .. 2  
  
문제의 예시와 기가막히게 맞아 떨어진다.  
