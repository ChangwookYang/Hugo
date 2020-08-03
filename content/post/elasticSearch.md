---
title: "ElasticSearch 기본 동작 이해"
date: 2020-08-03T22:40:00+09:00
---
### ElasticSearch 기본 동작 이해

[참고링크](https://lng1982.tistory.com/283)

ElasticSearch는 오픈 소스이고 **REST 기반의 실시간 검색 및 분석 엔진**이다.
Java로 작성되어졌으며 아파치 Lucene기반으로 검색 및 REST-Based API를 지원한다.


#### ElasticSearch 저장 구조
Index, Type, Document, Field, Mapping 용어들이 존재한다.

* Index란?
	* RDB의 데이터베이스와 유사하다.
* Type이란?
	* RDB의 테이블과 유사하다.
* Document란?
	* 테이블이 ROW와 유사하다.
	* JSON 문서로 되어있다. (key, value)
* Field란?
	* 엘라스틱 서치의 문서는 JSON이다.
	* JSON의 프로퍼티를 엘라스틱 서치에서는 필드라고 부른다.
	* RDB 테이블의 컬럼과 유사하다.
* Mapping이란?
	* RDB의 스키마와 유사하다.
	* http://localhost:9200/nklee/phone/1 POST 요청
	* 아래 JSON 데이터를 함께 전송하면 Elastic Search에서 mapping을 자동으로 생성해준다.
	{
      "number": "010-1111-1111",
      "author":"nklee"
    }
    

[Mapping 구조]
![이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile10.uf.tistory.com%2Fimage%2F99A4C43359A769361E05F2)
    
[저장구조]
![이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile8.uf.tistory.com%2Fimage%2F9969723359A75D4D3246AD)


ElasticSearch REST API URL 포맷은 다음과 같다.
> http://{Node:PortNumber}/{Index}/{Type}

Index는 소문자여야 한다.
Type은 Index와 마찬가지로 소문자를 추천한다.


---
#### 기본동작 테스트

> GET : 조회, POST : 저장, PUT : 수정, DELETE : 삭제

**저장하기**
[POST]
![이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile3.uf.tistory.com%2Fimage%2F9958443359A75DDC2AFC1E)
nklee 인덱스의 phone타입에 1이라는 아이디로 저장했다는 의미이다.
[결과값]

```JSON
{
    "_index": "nklee",
    "_type": "phone",
    "_id": "1",
    "_version": 1,
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "created": true
}
```

**조회하기**
[GET]
![이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile6.uf.tistory.com%2Fimage%2F994E4B3359A75DDC2B6EB3)
위에서 저장한 데이터를 조회하면 다음과 같이 JSON 결과값이 출력된다.
[결과값]
```JSON
{
    "_index": "nklee",
    "_type": "phone",
    "_id": "1",
    "_version": 1,
    "found": true,
    "_source": {
        "number": "010-1111-1111",
        "author": "nklee"
    }
}
```

**수정하기**
[PUT]
![이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile10.uf.tistory.com%2Fimage%2F99C08F3359A75DDC30F380)

이미 저장되어있는 author 값을 변경해서 요청
[결과값]
```JSON
{
    "_index": "nklee",
    "_type": "phone",
    "_id": "1",
    "_version": 3,
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "created": false
}
```

**모두 검색하기**
[GET]
![이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile30.uf.tistory.com%2Fimage%2F99CD7A3359A75DDC25B532)

저장되어있는 데이터 모두가 출력된다.
[결과값]
```JSON
{
    "took": 3,
    "timed_out": false,
    "_shards": {
        "total": 5,
        "successful": 5,
        "failed": 0
    },
    "hits": {
        "total": 2,
        "max_score": 1,
        "hits": [
            {
                "_index": "nklee",
                "_type": "phone",
                "_id": "2",
                "_score": 1,
                "_source": {
                    "number": "010-1111-1111",
                    "author": "nklee"
                }
            },
            {
                "_index": "nklee",
                "_type": "phone",
                "_id": "1",
                "_score": 1,
                "_source": {
                    "number": "010-1111-1111",
                    "author": "nklee22"
                }
            }
        ]
    }
}
```

**조건 검색하기**
[GET]
![이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile29.uf.tistory.com%2Fimage%2F99AB003359A75DDD31FAAA)
q 파라미터를 이용하여 조건 검색이 가능하다
```JSON
{
    "took": 2,
    "timed_out": false,
    "_shards": {
        "total": 5,
        "successful": 5,
        "failed": 0
    },
    "hits": {
        "total": 1,
        "max_score": 0.33912507,
        "hits": [
            {
                "_index": "nklee",
                "_type": "phone",
                "_id": "1",
                "_score": 0.33912507,
                "_source": {
                    "number": "010-1111-1111",
                    "author": "nklee22"
                }
            }
        ]
    }
}
```

**삭제**
![이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile27.uf.tistory.com%2Fimage%2F9938463359A75DDD2C5E25)
```JSON
{
    "found": false,
    "_index": "nklee",
    "_type": "phone",
    "_id": "2",
    "_version": 3,
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    }
}
```

