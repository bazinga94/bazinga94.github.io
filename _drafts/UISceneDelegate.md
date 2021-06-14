---
title:  "UISeceneDelegate"
excerpt: "UISeceneDelegate 그리고 UIAppDelegate"
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

https://icksw.tistory.com/137 참고

기존 AppDelegate의 역할은? 

App Delegate는 두 가지 주요 역할이 있다. 
첫 번째는 애플리케이션에 프로세스 수준 이벤트를 알리는 것입니다.  시스템은 프로세스가 시작되거나 종료 될 때 App Delegate에 알립니다.  
또한, 당신의 애플리케이션이 UI의 상태를 알려주는 두 번째 역할을 가지고있었습니다.

foreground로 진입하거나  (didEnterForeground, wilResignActive)
active 상태를 resign 하거나

이런 역할은 iOS 13 이전에 앱은 하나의 프로세스 하나의 UI만 관리했기 때문
그래서 AppDelegate의 didFinishLaunchingWithOptions 에는 Non-UI 셋업 작성들도 했고 데이터베이스 연결과 앱의 기동 단계에 필요한 비즈니스 작업들을 했음 그리고 나선 UI를 셋업했음

하지만 이제는 application은 하나의 프로세스만 관리하지만 동시아 여러개의 UI를 instances를 가질 수 있고 scene session들을 가질 수 있다.

따라서 AppDelegate는 여전히 프로세스 이벤트에 대한 라이프 사이클을 관리할 수 있지만 이제 UI 라이프 사이클에 대해서는 아무런 책임이 없어졌다. 그래서 이제 didFinishLaunching에서는 UI가 아닌 일회성 설정을 해주면 좋다.
고건 이제 SceneDelegate에서 해주니깐~!

동시에 AppDelegate는 이제 scene이 생성되거나 제거 되는 순간을 알 수 있다.

이제 바뀐 시스템을 예시로 살펴 보자
만약 앱을 하나 실행 시킨다면

먼저 UIAppDelegate에서 didFinishLaunching을 호출 한다.
이젠 UI셋업 로직을 실행 하지 않을것 이다. 대신 시스템은 scene session을 생성한다.
scene을 생성하기 전에 먼저 UIAppDelegate에서 UISeceneConfiguration을 생성하고
UISceneDelegate에서 willConnectToSession을 실행한다

여기서 개발자가 원하는데로 윈도우를 선택하여 configure까지 할 수 있다
그리고 앱에서 홈화면으로 이동하면 기존이랑 비슷하게 UISceneDelegate에서 
willResignActive
didEnterBackground
익숙한 순서대로 실행이 된다.

didDisconnect
* 중요!! 어느 시점에서 scene 연결은 끊어질 수 있다. 
시스템은 리소스를 회수하기 위해 scene이 백그라운드로 들어간 후 어느 시점에서 해당 scene를 메모리에서 해제 할 수 있다. 그렇게 되면 scene delegate도 메모리에서 해제되고 scene delegate가 보유한 모든 window 계층, UIView 계층도 해제가 된다.

이렇게 하면 scene과 관련된 커다란 메모리를 어플리케이션의 다른곳(아마 Scene Delegate(?))에서 할당하고 해제 할 수 있다.

따라서 유저 데이터나 상태를 이 상황에서 제거하지 않는것이 중요하다. scene은 다시 연결되거나 돌아올 수 있기 때문에

만약 앱 자체를 종료 시킨다면??
AppDelegate의 didDiscardSceneSessions 이 호출된다.
영구적으로 유저 데이터와 상태 들을 제거

정리하면 AppDelegate는 이제 앱의 실행 이벤트와 같은 LIfe Cycle에는 책임이 있지만 더 이상 UI Life Cycle에 대한 책임은 없어졌다.
UIKit은 App Delegate에 UI상태에 관여하는 메서드를 호출하지 않는다.

그런 이유로 AppDelegate는 새로운 책임이 생겼다 -> Session Life Cycle
시스템은 새로운 scene session이 생성되거나 기존 scene session이 삭제될 때 AppDelegate에 알리게 된다.

* state Restoration(상태 복원)
iOS 13 이후 앱에서는 scene 기반의 상태 복원을 구현하는 것이 중요하다고 한다.
하나의 앱을 여러개의 scene으로 나눠서 사용을 하다가 특정 scene들이 시스템에 의해 연결이 해제 될 수 있다(didDisconnect)

iOS13은 scene based 상태 복원 API를 가지고 있다.
상태를 인코딩하여 창을 다시 만들 수 있다.

NSUserActivity가 뭐지??