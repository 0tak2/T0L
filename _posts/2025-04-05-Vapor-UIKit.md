---
layout: post
title:  "Vapor, UIKit"
date:   2025-04-05 19:00:00 +0900
tags:
    - til
---

- 오늘은 이사, 루트, 듀이와 도서관에 갔다.
- 다이빙로그 백엔드를 Vapor로 짜고 있다. 인증 메일 전송 관련된 부분을 구현했다.
- 픽플즈가 많이 밀렸다. 고객 지도 피쳐를 리팩토링했다. 바텀시트 만든다고 뷰 컨트롤러에 로직을 몰아놨었는데 레이어를 나누었다.
- 오늘의 질문들
    - 이메일 검사 정규표현식
        - [ChatGPT](https://chatgpt.com/share/67f1ed56-8770-800a-95cb-c0e50e2bf3c5)
        - [Claude](https://claude.ai/share/639f5d5b-3b6d-4771-b7a6-5aeb608924c8)
        - 정규표현식은 쓰려고 하면 항상 머리가 백지가 된다. Claude는 Swift 최신에서 생긴 Regex 타입도 정확히 알려줬다.
    - Internal 에러 객체에 접근하기
        - [ChatGPT](https://chatgpt.com/share/67f1ede0-d990-800a-9e17-ea8bb964065b)
        - Vapor에 에러를 잡는 미들웨어를 만들었다. 예상하지 못한 에러는 500 상태코드로 하여 공통 형식으로 반환하게 했다.
        - 다만 Vapor 내부 에러(NotFound 등)도 잡아서 500으로 반환되는 문제가 있었다. 이 에러만 핸들링하려고 코드를 봤더니 internal이라 접근이 불가능했다.
        - Vapor를 포크해서 다시 짜는게 아닌 이상 어쩔 수 없는 문제이지만 바보같이 ChatGPT에 물어봤다. 역시 ChatGPT는 신이 아니다.
        - 코드를 자세히 보니 상위 타입으로 public으로 선언된 AbortError가 있었다. 이걸 잡도록 수정했다.
    - Date 차이 계산
        - [ChatGPT](https://chatgpt.com/share/67f1ee9e-afe8-800a-a52c-090696c30496)
        - 두 Data 객체 사이의 차이를 구하는 방법을 질문했다.
        - timeIntervalSince(_:) 메서드를 쓰면 된다.
        - 아는 거였는데... 생각이 안났다 ㅋㅋ
    - 외부 의존성 Static v.s. Dynamic
        - [ChatGPT](https://chatgpt.com/share/67f1eee6-8760-800a-8ab3-f5a8549b53b1)
        - 픽플즈에 SnapKit을 추가하는데 Static과 Dynamic 중 선택해야 했다.
        - 뭔지 하고 찾아봤는데 정적 링크할 것인지 동적 링크할 것인지의 이야기인 것 같다.
        - 그렇게 큰 프로젝트는 아니기 때문에 Static을 선택했다.
