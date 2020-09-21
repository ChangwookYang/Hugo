---
title: "프로그래머스 #42626 - 더 맵게"
date: 2020-09-22T00:20:00+09:00
---

### 프로그래머스 #42626 - 더 맵게


#### [힙 개념]

##### 1. 힙이란 ?

* 완전 이진 트리의 일종으로 우선순위 큐를 위해 만들어진 자료구조
* 여러 값 중에서 최소, 최대값을 빠르게 찾기위한 자료구조
* 힙은 느슨한 정렬상태
* 부모의 키 값이 자식보다 항상 큰/작은 이진 트리를 말한다. (큰 - 최대힙 / 작은 - 최소힙)
* 힙 트리는 중복을 허용한다.

##### 2.  힙 구현

* 힙의 표준자료구조는 배열 (구현을 쉽게 하기위해 인덱스 0은 사용 X)
* 힙에서의 부모 노드와 자식 노드의 관계
  * 왼쪽 자식의 인덱스 = 부모 인덱스 x 2
  * 오른쪽 자식의 인덱스 = 부모 인덱스 x 2 + 1
  * 부모의 인덱스 = 자식 인덱스 / 2

##### 3. 힙 삽입

* 힙에 새로운 요소가 들어오면 마지막 노드에 이어서 삽입
* 새로운 노드를 부모 노드들과 교환해서 힙의 성질을 만족할때까지 교환
* 다른 자식 노드는 부모보다 이미 작거나 같으므로 비교할 필요 없다.

##### 4. 힙 삭제

* 삭제는 무조건 루트에서 일어난다.
* 최대 힙에서 최대값은 루트노드이므로 루트노드가 삭제된다.
* 삭제된 루트 노드에는 힙의 마지막 노드를 가져오고 힙을 재구성한다.




#### [프로그래머스 #42626]

[문제링크](https://programmers.co.kr/learn/courses/30/lessons/42626)

##### 1. 힙 직접 구현(테스트 케이스 일부 통과 실패)

```java
public static int solution(int[] scoville, int K) {
		List<Integer> heap = Arrays.stream(scoville).boxed().sorted().collect(Collectors.toList());
        
    	// heap 계산을 쉽게하기 위해 0추가
        heap.add(0, 0);

        while(true){
            if(heap.get(1) > K) break;
            else if(heap.size() == 2){
                if(heap.get(1) > K) break;
                else return -1;
            }

            Integer minFirstVal = heap.remove(1);
            Integer minSecondVal = heap.remove(1);

            answer++;

            Integer newValue = minFirstVal + (2*minSecondVal);
            heap.add(newValue);
            int index = heap.size()-1;
            while(index >= 2 && heap.get(index) < heap.get(index/2)){
                Integer temp = heap.get(index);
                heap.set(index, heap.get(index/2));
                heap.set(index/2, temp);
                index = index/2;
            }
        }
}
```



##### 2. PriorityQueue 사용(통과)

```java
public static int solution(int[] scoville, int K) {
        int answer = 0;

        PriorityQueue<Integer> priorityQueue = Arrays.stream(scoville).;

        for(int val : scoville){
            priorityQueue.add(val);
        }

        while(priorityQueue.size() > 1){
            int firstVal = priorityQueue.poll();
            if(firstVal >= K) break;
            else {
                answer++;
                int secondVal = priorityQueue.poll();
                priorityQueue.add(firstVal + 2*secondVal);
            }
        }
        
        if(priorityQueue.size() == 1 && priorityQueue.peek() < K) answer = -1;
        return answer;
}
```

