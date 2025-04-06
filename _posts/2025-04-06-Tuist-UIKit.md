---
layout: post
title:  "Tuist, UIKit"
date:   2025-04-06 18:00:00 +0900
tags:
    - til
---

- 밑바닥부터 시작하는 딥러닝 스터디를 시작했다. 첫 주제는 2장의 퍼셉트론이다.
- 파이썬으로 예제가 되어있는데, NumPy만 어떻게 하면 Swift로 될 것 같아서 프로젝트를 하나 만들었다.
- 이걸 하면서 처음으로 Tuist를 써봤다. 
    ```
    App -> CoreInterface <- Core
              ...
    ```
    - 이렇게 해두었는데 App에서 Core 패키지 접근하는게 가능했다
    - 이상한 일이다... 😭 좀 더 공부해봐야 알 것 같다.
    - [Project.swift](https://github.com/0tak2/DeepLearningFromScratchInSwift/blob/1e5ec6e3f856e21d3b94778ca4ed3b47be8df98e/Project.swift)
- 픽플즈를 계속 했다.
    - 스냅킷을 처음 적용했다. 손이 덜 아프다. 천국+정토+극락 그자체...
    - 바텀시트 관련된 디테일을 계속 수정했다.
        - UIView에 대해 특정한 코너만 Radius 주기
            - 예전에는 [이렇게](https://stackoverflow.com/questions/10167266/how-to-set-cornerradius-for-only-top-left-and-top-right-corner-of-a-uiview) 했다고 한다.
            - iOS 11부터는 [maskedCorners 프로퍼티](https://www.hackingwithswift.com/example-code/calayer/how-to-round-only-specific-corners-using-maskedcorners)를 이용해 우아하게 할 수 있다. 무려 신입생 시절에 나온 버전이다...
    - UICollectionView는 강력하지만 너무 불편하기도 하다. 오늘도 Diffable DataSource와 Compositional Layout을 어떻게 다루는지 까먹어서 헤맸다.
- 오늘의 질문들
    - UICollectionView 가로 스크롤
        - [ChatGPT](https://chatgpt.com/share/67f2426e-31d4-800a-af15-c6251ee49fea)
        - 가로로 배치되게 했는데 스크롤을 넘기면 그룹과 그룹 사이가 떨어지게 배치되는 문제가 있었다.
        - 그룹의 너비를 .fraction(1)로 해두어서 이 너비를 넘어갈 것 같으니 잘리는 것이었다.
        - .estimated(1000)으로 주어서 넘칠 수 있도록 했다.
        - orthogonalScrollingBehavior를 .continuous로 했더니, section.interGroupSpacing을 줬을 때 셀끼리 떨어지는 걸 볼 수 있었다. 아마 그룹 당 아이템 하나로 구성되고, 여러 그룹이 섹션을 이루고 있는 듯 하다. 생각보다 Compositional Layout의 섹션, 그룹 등에 대해 잘 모르고 있는 것 같다. 그룹마다 색을 다르게 준다든지 해서 디버깅이 필요하다.
