---
title: "Backtracking 알고리즘"
date: 2020-07-10T00:32:21+09:00
---
## Backtracking 이란?

어떤 노드의 유망성 점검 후, 유망하지 않으면 그 노드의 부모노드로 되돌아간 후 다른 자손노드를 검색

KEYWORD !
"배제", "풀이 시간 단축"

유망하지 않으면 배제를 하고 부모노드로 되돌아가면서 풀이 시간을 단축!!


Backtracking의 가장 유명한 예제 중 하나인 4-Queens Problem!

4-Queens Problem은 4개의 Queen을 서로 상대방을 위협하지 않도록 4*4 체스판에 위치시키는 문제이다.

좌측최상단이 (1,1)이다.

![Queen4](https://user-images.githubusercontent.com/66955409/87060419-77ffb480-c245-11ea-8a4d-5bdf3a1e2cad.JPG))

![Tree Image](https://user-images.githubusercontent.com/66955409/87061288-84384180-c246-11ea-9676-d06408c41ea3.JPG)

되추적은 깊이우선탐색(Depth-First Search)과 마찬가지로 스택을 사용한다!

![유망](https://user-images.githubusercontent.com/66955409/87061567-e133f780-c246-11ea-8ca3-b12c14b56b1d.JPG)

깊이우선탐색은 (1,1) 노도의 자식인 (2,1),(2,2),(2,3),(2,4)를 스택에 넣을 것이다.

하지만!! 되추적은 (2,1),(2,2)를 스택에 넣지 않는다.

그 이유는 유망하지 않기 때문이다!!!

이처럼 되추적은 스택에 자식 노드를 넣기 전에 유망한지, 즉 해답이 될 가능성이 있는지 확인한 후 스택에 넣는다.

그리고 (2,1),(2,2)는 해답이 될 가능성이 없기에 가능성에서 배제한다!

![end part](https://user-images.githubusercontent.com/66955409/87062291-d7f75a80-c247-11ea-9ffa-6f1a3ef240de.JPG)





