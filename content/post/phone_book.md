---
title: "전화번호 목록(프로그래머스 42577)"
date: 2020-07-18T23:32:41+09:00
---

## 전화번호 목록  
[프로그래머스 42577 URL](https://programmers.co.kr/learn/courses/30/lessons/42577)

![image](https://user-images.githubusercontent.com/66955409/87854785-3363cf80-c94f-11ea-91e7-0ffeea7a5c76.png)

---


첫번쨰 방법으로 풀다가 안되어서 두번째 방법으로 푸니까 잘 됐다!  
아이디어는 동일한데 첫번째는 정렬을 했다.  
크기 순서대로 앞에서부터 하면 더효율적일줄 알았는데 아니였다.
  
검색해보니 **Arrays.sort의 시간복잡도는 O(n log(n))** 이고 n^2 까지 더한 시간복잡도이고
두번쨰 풀이는 그냥 n^2이였다.

---

#### 첫번째 코드(효율성에서 x)
```JAVA
public boolean solution(String[] phone_book) {
    boolean answer = true;
    Arrays.sort(phone_book, new Comparator<String>(){

        @Override
        public int compare(String o1, String o2) {
            if(o1.length() < o2.length()){
                return -1;
            } else {
                return 1;
            }
        }
    });
    
    for(int i=0; i< phone_book.length-1 ;i++){
        String val = phone_book[i];
        for(int j=i+1; j<phone_book.length;j++){
            String val2 = phone_book[j].substring(0, val.length());
            if(val.equals(val2)){
                answer = false;
                return answer;
            } 
        }
    }
}            

```

#### 두번째 코드(통과)
```JAVA
public boolean solution(String[] phone_book) {
    boolean answer = true;
    for(int i=0; i< phone_book.length ;i++){
        String val = phone_book[i];
        for(int j=0; j<phone_book.length;j++){
            if(i==j || val.length() > phone_book[j].length()) continue;

            String val2 = phone_book[j].substring(0, val.length());
            if(val.equals(val2)){
                answer = false;
                return answer;
            } 

        }
    }

    return answer;
}
```