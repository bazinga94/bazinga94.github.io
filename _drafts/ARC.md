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

ARC

ARC는 Swift Runtime이라는 라이브러리에 구현되어있다.

컴파일시 해당 라이브러리에서는 동적으로 할당되는 레퍼런스(init 된 object, 포인터, 클로져 블록 등등)에 대해 HeapObject라는 struct로 표현한다

HeapObject에 해당 레퍼런스에 대한 reference count(strong reference!)가 저장되어 있는데

ARC는 컴파일 시점에 해당 레퍼런스가 할당되는 시점 부터 사용되는 시점까지 추적하여 retain, release 코드를 추가한다
따라서 해당 레퍼런스의 reference count는 retain, release 코드들이 runtime중에 count를 관리한다.

Count 관리 매커니즘은
해당 객체가 init 되거나 참조되는 경우 count ++
참조가 해제 되는 경우 count --

Count 가 0 이 되면 deinit으로 메모리에서 해제한다

+ 여기에 더해서 왜 우린 weak을 사용하냐에 까지 확인해보면

예시로 ARC는 

func tempFunction() {
var obj = TempClass()
obj.retain()       // ARC

doWork(obj)

obj.release()      // ARC
}

이런식으로 메모리를 관리하는것 같은데
만약 obj를 다른 클래스에서 참조하고 obj가 다른 해당 클래스 레퍼런스를 참조하면 retain cycle이 생겨서 메모리가 해제 되지 않는다.
