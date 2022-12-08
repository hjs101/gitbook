# 2일차

## 디자인 패턴

### 프록시(proxy) 패턴
---
- 대상 객체에 접근하기 전에, 해당 흐름을 가로채 인터페이스의 역할을 하는 디자인 패턴
- 객체의 속성, 변환 등을 보완
- 보안, 데이터 검증, 캐싱, 로깅 등에 사용함

#### 프록시 서버
- 서버와 클라이언트 사이에 클라이언트가 프록시 서버를 통해 간접적으로 접속

:sparkles: **내 경험** : Nginx의 ReverseProxy 기능을 이용하여 외부 IP로의 접속을 내부 서브넷과 연결시킴.

![image](https://user-images.githubusercontent.com/97939170/206518032-50aa9a86-e96b-42b1-8e33-3c50a5957be0.png)

##### 장점 : 보안성 강화
- 실제 서버의 포트를 숨길 수 도 있다.
- 버퍼 오버플로우 방어
- DDOS 방어
- HTTPS 구축
※ Buffer Overflow 공격 : 데이터는 메모리에 저장될 때 버퍼라는 공간에 저장되는데, 이 버퍼에 데이터가 담길 때 설정된 버퍼의 허용량을 초과하면, 버퍼의 공간 뒤에 값이 덮어씌워지게 된다. 이를 통해 기존의 데이터를 지우거나, 덮어씌우는 등의 공격을 할 수 있게 된다.

※ HTTPS : TLS/SSL인증서 통해 보안통신을 하는 경우에 URL에 Https가 붙게 된다. TLS는 Transfer Layer Security의 약자로, 전송계층(4 Layer)보안을 의미한다.

### 이터레이터(iterator) 패턴
---
- 이터레이터를 사용하여 Collection에 접근하는 것

**이터레이터 패턴 JS**
```
const mp = new Map()
mp.set('a', 1)
mp.set('b', 2)
mp.set('c', 3)
const st = new Set()
st.add(1)
st.add(2)
st.add(3)
for (let a of mp) console.log(a)
for (let a of st) console.log(a)
/*
[ 'a', 1 ]
[ 'b', 2 ]
[ 'c', 3 ]
1
2
3
*/
```

※ 이터레이터 프로토콜 : iterator로 객체를 순회할 때 쓰이는 규칙 (ex: for a of b 등)

### 노출 모듈(Revealing module) 패턴 (JS)
---
- 즉시 실행 함수를 이용해 접근제어자를 만드는 패턴
- JS는 접근제어자가 존재하지 않기 때문에, 해당 패턴을 통해 접근제어자를 구현할 수도 있다.

※ 즉시실행함수 : 함수가 정의되자마자 호출되는 함수

**접근제어자 정리**
```
public : 자식, 외부, 자신 모두 접근 가능
protected : 자신, 자식 접근 가능
private : 자신만 접근 가능
```

### MVC 패턴 (Model, View, Controller)

#### 모델(Model)
- 앱에서 사용하는 데이터를 뜻함
- DB, 상수, 변수 등의 데이터
- 컨트롤러에서 모델을 생성하거나 갱신함

#### 뷰(View)
- UI 요소
- 사용자가 보고 조작하는 화면
- 변경사항을 컨트롤러에 전달

#### 컨트롤러(Controller)
- 모델과 뷰를 이어주는 다리 역할
- 메인 로직 담당
- 모델, 뷰의 생명주기 관리
- 모델, 뷰로부터 알림을 받고 그것을 해석하여 맞은 편에 전달

※ MVC 패턴의 예 : React
