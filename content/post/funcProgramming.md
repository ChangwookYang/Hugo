---
title: "함수형 프로그래밍이란?"
date: 2020-07-13T23:47:00+09:00
---

### 함수형 프로그래밍이란?

![명령형카카오](https://user-images.githubusercontent.com/66955409/87318578-814d9180-c563-11ea-81bf-4b5acf3baf5f.png)
기존의 명령형 방식 : 너는 이걸하고 저걸 누구랑 어떻게 해서 어떠한 결과를  산출해 내라!  
![함수형카카오](https://user-images.githubusercontent.com/66955409/87318590-83afeb80-c563-11ea-9bf0-a2f4e1f2d609.png)
함수형 형식 : 이거는 이거다!  


* 함수형 프로그래밍의 특징  
    * 인풋과 아웃풋이 있다  
    * 외부 환경으로부터 철저히 독립적이다.  
    * 동일한 인풋에 있어서 언제나 동일한 아웃풋을 만든다.  
    (순수함수 : 인풋이 같으면 아웃풋은 동일하다.)  
  
어떤 함수의 동작에 의해 프로그램 내 특정 상태가 변경되는 상황이 발생할 수 있다.  
함수형 프로그래밍은 함수의 동작에 의한 함수의 동작에 의한 변수의 부수적인 값 변경을 원천 배제함으로써 오류를 방지한다.  
  

1. 함수형 프로그래밍은 '선언형'이다.  
    기존은 '명령형' 형식 - 너는 이걸하고 너는 저걸 누구랑 어떻게해서 어떠한 결과를 산출해내라  
    선언형은 이거'는' 이거다!
    순수함수들은 인풋만 같으면 같은 결과를 산출한다.

2. 함수도 값이다.
```javascript
function say_it (given){
	console.log(given);
}
var say_it = function (given){
	console.log(given);
}
const say_it = (given) => console.log(given);
```  
순수함수의 경우 인자값에 대한 정해진 값을 리턴해주기 때문에 함수를 값으로 생각할 수 있다.

3. 고계함수
함수를 값으로 볼 수 있다면 함수도 다른 함수의 인자로 넣어줄 수 있다.  
다른 함수를 인자로 받거나 다른 함수를 반환하는 함수를 고계함수라 한다.  
```javascript
var calc = function(num1, num2, op){
    return op(num1, num2);
}
const calc = (num1, num2, op) => op(num1, num2);

const add = (num1, num2) => num1 + num2;
const multiplay = (num1, num2) => num1 * num2;

calc(2, 3 add);
calc(2, 3, multiply);
```  
![고계함수1](https://user-images.githubusercontent.com/66955409/87318685-a3471400-c563-11ea-9c86-e2f77c722648.png)  
```javascript
const calcWidth = (op) => (num) => op(2, num)
const add2 = calcWidth(add);
const multiply2 = calcWidth(multiply);

add2(3) // 5 리턴
multiply2(3) // 6 리턴
```
![고계함수2](https://user-images.githubusercontent.com/66955409/87318699-aa6e2200-c563-11ea-99af-c0db3ede58bc.png)

4. 커링
함수형 언어 Scalar
```javascript
def add(num1 : Int, num2 : Int):Int = num1 + num2;
add(2, 3)

def add_curry(num1:Int)(num2:Int):Int = num1 + num2
add_curry(2)(3)
// 숫자 하나를 인자로 받는 함수의 실행모습
val add2 = add_curry(2)(_)
add2(9) // 11 리턴
```  
여러 인자를 받는 함수에 일부 인자만 넣어서 나머지 인자를 받는 다른 함수를 만들어 낼 수 있는 함수프로그래밍 기법을 *커링*이라고 한다.  
커링을 활용하면 모든 인자들이 준비되지 않았을 때 부분적용된 상태의 함수를 만들어내서 마련해두거나 다른 함수로 인자를 넘겨줄 수 있다.  
  
5. 함수 컴비네이터
![함수컴비네이터1](https://user-images.githubusercontent.com/66955409/87318719-b0fc9980-c563-11ea-990b-492999eab788.png)
![함수컴비네이터2](https://user-images.githubusercontent.com/66955409/87318759-b8bc3e00-c563-11ea-8cdc-b30a2fc520ff.png)

