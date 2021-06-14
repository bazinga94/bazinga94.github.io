---
title:  "[iOS] View의 Display & Layout 관련 메소드"
excerpt: "자주 사용하지만 완벽하게 모르는 iOS 뷰 레이아웃 관련 메소드"
toc: true
toc_label: "목차"    # 목차 명
toc_icon: "cog"     # 목차 아이콘 ex) fish, carrot ...
toc_sticky: true    # 목차 상단에 고정
categories:
  - iOS
tags:
  - UIView
  - Layout
  - Display
last_modified_at: 2021-04-29T00:00:00-00:00
---

Dispatch 에 대해

https://medium.com/@venki0119/method-dispatch-in-swift-effects-of-it-on-performance-b5f120e497d3

https://jcsoohwancho.github.io/2019-10-11-Dynamic-Dispatch%EC%99%80-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94/

https://jcsoohwancho.github.io/2019-11-01-Swift%EC%9D%98-Dispatch-%EA%B7%9C%EC%B9%99/

https://jcsoohwancho.github.io/2019-11-02-Message-Dispatch/

https://developer.apple.com/swift/blog/?id=27

https://medium.com/@santu.chakraborty2009/method-dispatch-in-swift-47f4cc67ce9b


Dispatch 메소드란?
CPU가 실행할 메소드의 메모리 주소값을 찾아내는 과정! 
메소드의 메모리 주소값을 저장하는 방식은 크게 두가지로 나뉜다.

Direct(Static or compile time) / Dynamic(runtime) Dispatch

Dynamic Dispatch는 Table / Message Dispatch로 나뉘는데
Swift는 상황에 따라 3가지 메소드 타입을 전부 사용한다. (Objective C는 거의 Message Dispatch만 사용한다)


1. Direct Dispatch

가장 빠른 dispatch method 왜냐?? -> 컴파일 단계에서 어떤 메소드가 호출 되는지 컴파일러가 알고 있음
앱 성능에 가장 좋음

그럼 어떤 경우 Direct Dispatch를 사용하느냐 ??
- Value type에 구현된 method(struct, enum..)
- extension에 구현된 method, 단 NSObject의 subclass 가 아닌 경우
- Reference type에 구현된 method이지만 final을 사용하거나 private, inline(????)을 사용 하는 경우
- WMO(Whole Module Optimization ?? 어떻게 활성화하지…) 을 활성화 하면 모듈 전체를 하나의 덩어리로 취급하기 때문에 internal 선언만 하더라도 overriding이 불가능하다는 것이 보장


2. Table Dispatch

Class method 주소를 저장하기 위해 포인터 배열을 저장함(테이블 형태로?) 자기 자신이 처리 할 수 있는 모든 메소드에 대한 포인터를 유지하고 있음. 부모 타입으로 부터 상속받은 메소드는 같은 주소값을 유지, 오버라이드 하게 되면 이를 덮어쓴다.
런타임에 테이블을 조회하여 적절한 동적 메서드를 호출한다

- method가 protocol, class, NSObject의 subclass 인데 dynamic이나 @objc 인 경우
- NSObject의 extension, subclass overriding

3. Message Dispatch
자기 자신이 오버라이드 하거나 새로 정의한 메소드들만 테이블에 유지한다. 대신 부모 타입의 포인터를 가지고 있어서 부모 타입의 메소드들은 부모 타입에서 찾는다. -> 런타임에 메소드의 동작을 수정하거나 클래스를 동적으로 만드는것도 가능해짐 하지만 이것들은 swift가 제공하는 방식이 아님

-> Message Dispatch 사용을 방지 하기 위한 방법?
- 되도록이면 value type을 사용
- Final, private 적극 사용
- Whole Module Optimization을 사용하여 메소드가 subclassing 되지 않음을 보장
- Dynamic 키워드 사용하지 않기


++ protocol 도 dynamic??
프로토콜도 매번 그런게 아님



