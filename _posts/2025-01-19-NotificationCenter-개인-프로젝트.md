---
layout: post
title:  "NotificationCenter, 개인 프로젝트"
date:   2025-01-19 21:00:00 +0900
tags:
    - til
---

- NotificationCenter를 [공부](https://github.com/0tak2/obsidian-public/blob/main/iOS/Notification%20Center.md)하고 개인 프로젝트 꿈꾸에 [적용](https://github.com/0tak2/kkumku/issues/40)했다.
- 꿈꾸 검색 뷰에 디바운싱을 [적용](https://github.com/0tak2/kkumku/pull/42)했다. [이 글](https://stackoverflow.com/questions/27116684/how-can-i-debounce-a-method-call)을 많이 참고했다.
- CoreData 때문에 또 애를 먹었다.
  - 현재 다음과 같이 연결 테이블을 통해 다대다 관계이다.
    ```
    [Dream] n -- n [DreamAndTag] n -- n [DreamTag]
    ```
  - 기본적으로 Core Data의 릴레이션에 대한 Delete Rule은 Nullify이므로, Dream을 삭제하면 외래키로 잡혀있던 부분에 nil이 들어갈 뿐, 필요 없어진 태그가 알아서 삭제되지는 않는다.
  - Delete Rule을 Cascade로 해봤는데, 수정 시 사용되지 않는 태그에 대해 DreamAndTag를 지우는 과정에서 Dream도 삭제되는 문제가 있었다. 양방향 관계에 대해 Delete Rule을 Cascade로 지정하니 반대로도 적용된 것이다.
  - 우선 Delete Rule을 Nullify로 두고, 삭제이나 수정 시점에 연결 테이블에 nil을 포함한 엔티티가 있으면 제거, 태그 엔티티 중 참조되지 않는 게 있으면 제거하도록 구현해두었다.
- 그 외에도 몇몇 버그와 사용성 개선 커밋을 했다.
