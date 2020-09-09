---
title: "순열알고리즘(DFS)"
date: 2020-09-10T02:02:00+09:00
---

### 순열 알고리즘

#### 1. 순열이란? 서로 다른 n개 중 r개를 골라 순서로 고려해 나열한 경우의 수

   [프로그래머스 42839](https://programmers.co.kr/learn/courses/30/lessons/42839)



#### 2. DFS를 사용하여 구현!

   [DFS/BFS 알고리즘](https://changwookyang.github.io/post/bfsdfs/)



#### 3. 알고리즘 설계

| 변수    | 설명                                  |
| ------- | ------------------------------------- |
| arr     | r개를 뽑기 위한 n개의 배열            |
| output  | 뽑힌 r개의 배열                       |
| visited | 중복해서 뽑지 않기 위해 체크하는 배열 |

DFS를 돌면서 모든 인덱스를 방문하여 output에 값을 넣는다.

이미 들어간 값은 visited 값을 true로 바꾸어 중복하여 넣지 않도록 한다.

depth의 값은 output에 들어간 숫자의 길이이다.(현재 output의 배열길이)

depth의 값이 r만큼 되면 순열 완성



>ex) arr = { 1, 2, 3 };
>
>아래 채워지는 칸들이 output 배열에 해당

<img width="800" alt="1111" src="https://user-images.githubusercontent.com/66955409/92633997-9437d480-f30e-11ea-8b45-beac1cea2484.PNG">

##### 소스코드
```java
// 사용 예시: permutation(arr, output, visited, 0, n, 3); -- n개 중에서 3개 뽑아서 순열을 만든다!
// permutation(new int[]{1,2,3}, new int[3], new bolean[3], 0, 3, 3)
void permutation(int[] arr, int[] output, boolean[] visited, int depth, int n, int r) {
    if (depth == r) {
        print(output, r);
        return;
    }
 
    for (int i=0; i<n; i++) {
        if (visited[i] != true) {
            visited[i] = true;
            output[depth] = arr[i];
            perm(arr, output, visited, depth + 1, n, r);
            output[depth] = 0; // 이 줄은 없어도 됨
            visited[i] = false;;
        }
    }
}

```

##### 실제실현 로직
```java
// 초기
public static void main(String[] args) {
    int[] arr = {1, 2, 3};
    int[] output = new int[n];
    boolean[] visited = new boolean[n];
    int n = 3;
    int depth = 0;
    int r = 3;

    permutation(arr, output, visited, depth, n, r);
}

[when output={0,0,0}, visited={false,false,false}, depth=0, n=3, r=3]
i = 0
visited[0] = true;
output[0] = arr[0];
permutation(arr, output, visited, depth + 1, n, r);

	[when output={1,0,0}, visited={true,false,false}, depth=1, n=3, r=3]
	i = 0 -> visited[0] = true이므로 PASS
	i = 1
	visited[1] = true;
	output[1] = arr[1]; 
	permutation(arr, output, visited, depth + 1, n, r);

		[when output={1,2,0}, visited={true,true,false}, depth=2, n=3, r=3]
		i = 0 -> visited[0] = true이므로 PASS
		i = 1 -> visited[1] = true이므로 PASS
		i = 2
		visited[2] = true;
		output[2] = arr[2];
		permutation(arr, output, visited, depth + 1, n, r);
			[when output={1,2,3}, visited={true,true,true}, depth=3, n=3, r=3]
			depth == r이므로 RETURN
		output[2] = 0;
		visited[2] = false;

	output[1] = 0;
	visited[1] = false;
	
	i = 2
    visited[i:2] = true;
	output[depth:1] = arr[i:2];
	permutation(arr, output, visited, depth + 1, n, r);

		[when output={1,3,0}, visited={true,false,true}, depth=2, n=3, r=3]
		i = 0 -> visited[0] = true이므로 PASS
		i = 1
        visited[1] = true;
		output[2] = arr[1];
		permutation(arr, output, visited, depth + 1, n, r);
			[when output={1,3,2}, visited={true,true,true}, depth=3, n=3, r=3]
			depth == r이므로 RETURN
		output[2] = 0;
		visited[2] = false;

	output[1] = 0;
	visited[1] = false;


output[0] = 0;
visited[0] = false;

i = 1
visited[1] = true;
output[0] = arr[1];
permutation(arr, output, visited, depth + 1, n, r);
	[when output={2,0,0}, visited={true,false,false}, depth=1, n=3, r=3]

	...
```





[참고사이트](https://bcp0109.tistory.com/14)

