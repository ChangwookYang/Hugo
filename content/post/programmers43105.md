---
title: "프로그래머스 43105, 정수삼각형"
date: 2020-07-25T11:30:00+09:00

---
### 프로그래머스 43105, 정수삼각형

[문제 링크](https://programmers.co.kr/learn/courses/30/lessons/43105)
![문제설명](https://user-images.githubusercontent.com/66955409/88447100-a945ca80-ce6a-11ea-8911-e6bcd16cd8ee.png)



---

### 동적 프로그래밍 문제이다.

점화식을 알면 생각보다 쉽게 풀수 있다!  
각 행의 합은 (이전 행 열인덱스-1), (이전행의 열인덱스)까지의 합 중  최대값을 가져오면 된다.

#### 점화식
Max(i, j) = triangle(i, j) + 최대값(Max(i-1, j-1), Max(i-1, j))

| i \  j | j=0  | j=1  | j=2  | j=3  | j=4  |
| ------ | ---- | ---- | ---- | ---- | ---- |
| i = 0  | 0    | 0    | 0    | 0    | 0    |
| i = 1  | 0    | 7    | 0    | 0    | 0    |
| i = 2  | 0    | 10   | 15   | 0    | 0    |
| i = 3  | 0    | 18   | 16   | 15   | 0    |


```JAVA
public int solution(int[][] triangle) {
        int answer = 0;
        
        int length = triangle.length;
        int[][] max = new int[length+1][length+1];
        max[1][1] = triangle[0][0];
        for(int i=2; i <= length; i++){
            for(int j=1; j <= i; j++){
                max[i][j] = triangle[i-1][j-1] + Math.max(max[i-1][j-1], max[i-1][j]);
            }
        }

        for(int i=1; i <= length; i++){
            if(answer < max[length][i]) answer = max[length][i];
        }
        
        return answer;
    }
```