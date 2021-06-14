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

Objective - c 런타임, NSObject…. 어렵다 어려워


KVO, 코어데이터에서 Objective - c 런타임?

NSObject, NSObjectProtocol ?

@objc dynamic ?

NSObject 
-> 옵씨 클래스 계층 구조의 최상위 클래스
하위 클래스에서 런타임 시스템에 대한 기본 인터페이스와 objective c 객체로 작동하는 기능을 상속

옵씨 런타임 시스템이란?
-> 객체 생성, 해제에 따른 메모리 영역 관리(ARC) 송신된 메시지에 대응하는 메서드 검색(Dynamic Dispatch ?)등 을 한다.
런타임 시스템은 개발자가 직접 사용하는게 아니라 NSObject에 있는 메서드로 제공된다.
NSObject를 상속한 모든 클래스는 상속된 기능으로 옵씨 런타임 시스템을 이용할 수 있다.

따라서 root class 즉 NSObject는 런타임 시스템에 대한 인터페이스 역할 이라고 볼 수 있다.

*** Cocoa touch framework 는 NeXT 회사의 OPENSTEP의 주요 API를 사용 NS -> NeXT Step


NSObjectProtocol
-> 채택하는 경우 가지게 되는 특성
- 상속 계층 내의 클래스 위치
- 프로토콜 채택
- 특정 메시지에 응답 할 수 있는 능력 ????

NSObject 가 이 프로토콜을 채택했다.
