---
title: "LRU 알고리즘"
date: 2020-07-26T22:20:21+09:00
---

## LRU 알고리즘

> Least Recently Used Algorithm

Cache 알고리즘 중에 가장 유명한 알고리즘 중 하나로 LRU알고리즘이 있다.

캐시의 리소스 양은 제한되어 있고 제한된 리소스 내에서 데이터를 빠르게 저장하고 접근해야한다.



LRU 알고리즘은 메모리 상에서 **<u>가장 최근에 사용된적 없는</u>** 캐시의 메모리를 새 데이터로 갱신해준다.

  
   --- 

### 알고리즘 동작

Step1 : 1 -> 2 -> 3 순차 호출 시

>[3] [2] [1]

Step2 : Get(2) : 2번 캐시를 호출함

> [2] [3] [1]

Step3 : Put(4) : 데이터 4를 새로 캐시에 씀, Least Recently Used 인 1은 제거

> [4] [2] [3] 

  
---  

가장 최근에 사용된 항목은 리스트 맨 앞에 위치하고

가장 최근에 사용되지 않은 항목 순서대로 리스트에서 제거된다.

LRU 알고리즘 구현은 LinkedList를 이용한 Queue로 이루어지고,  
 접근 성능 개선을 위해 Map을 같이 사용한다.

```Java
public class LRUCacheImpl {
	
	private class ListNode {
		private int key;
		private int val;
		private ListNode prev;
		private ListNode next;
		
		public ListNode(int key, int val){
			this.key = key;
			this.val = val;
			this.prev = null;
			this.next = null;
		}
	}
	
	private Map<Integer, ListNode> nodeMap;
	private int capacity;
	private ListNode head;
	private ListNode tail;

	public LRUCacheImpl(int capacity){
		this.nodeMap = new HashMap<>();
		this.capacity = capacity;	// cache 크기 초기화
		head = new ListNode(0, 0);
		tail = new ListNode(0, 0);
		head.next = tail;
		tail.prev = head;
	}
	
	private void remove(ListNode node){
		node.prev.next = node.next;	// 이전 노드가 
		node.next.prev = node.prev;
		nodeMap.remove(node.key);	// map에서 제거
	}
	
	private void insertToHead(ListNode node){
		this.head.next.prev = node;
		node.next = this.head.next;
		node.prev = this.head;	// 포인터용 head 노드가 존재한다.
		this.head.next = node;
		nodeMap.put(node.key, node);
	}
	
	public int get(int key){
		if(!nodeMap.containsKey(key)){
			return -1;
		}
		ListNode node = nodeMap.get(key);
		remove(node);
		insertToHead(node);	// 맨앞에 가져다놓는다.
		return node.val;
	}
	
	public void put(int key, int value){
		ListNode newNode = new ListNode(key, value);
		if(nodeMap.containsKey(key)){
			ListNode oldNode = nodeMap.get(key);
			remove(oldNode);
		} else {
			if(nodeMap.size >= capacity){
				ListNode tailNode = tail.prev;
				remove(tailNode);
			}
		}
		insertToHead(newNode);
	}
}


```

** [출처링크](https://jins-dev.tistory.com/entry/LRU-Cache-Algorithm-%EC%A0%95%EB%A6%AC)