---
layout: post
title:  "SwiftUI .postion vs .offset, Vapor"
date:   2025-03-29 19:00:00 +0900
tags:
    - til
---

- 오늘은 루크와 테라로사에 왔다.

- 이전에 삐약반 과제를 따라하다가 [.postion 모디파이어](https://developer.apple.com/documentation/swiftui/view/position(x:y:))와 [.offset 모데파이어](https://developer.apple.com/documentation/swiftui/view/offset(x:y:))의 차이가 궁금했다.
- 아래와 같은 예제로 실험해봤다.

  ```swift
  import SwiftUI
  import PlaygroundSupport

  struct PositionVsOffsetView: View {
      @State var xPos: CGFloat = 0.0
      @State var yPos: CGFloat = 0.0
      
      var body: some View {
          VStack {
              ZStack {
                  Circle()
                      .fill(Color.red)
                      .opacity(0.5)
                      .frame(width: 100, height: 100)
                      .position(x: xPos, y: yPos) // 뷰의 가운데 점을 부모 뷰의 특정 좌표에 둔다.
                      .border(Color.black)
                      
                  
                  Rectangle()
                      .fill(Color.blue)
                      .opacity(0.5)
                      .frame(width: 100, height: 100)
                      .offset(x: xPos, y: yPos) // 지정한 세로, 가로 거리 만큼 오프셋을 둔다 (옮긴다)
                      .border(Color.black)
              }
              
              HStack {
                  Button {
                      xPos -= 10.0
                  } label: {
                      Image(systemName: "arrow.left")
                  }

                  Button {
                      yPos -= 10.0
                  } label: {
                      Image(systemName: "arrow.up")
                  }
                  
                  Button {
                      yPos += 10.0
                  } label: {
                      Image(systemName: "arrow.down")
                  }
                  
                  Button {
                      xPos += 10.0
                  } label: {
                      Image(systemName: "arrow.right")
                  }
              }
              
              Text("xPos: \(Int(xPos)), yPos: \(Int(yPos))")
          }
          .background(Color.orange)
          .frame(width: 512, height: 512)
      }
  }

  PlaygroundPage.current.setLiveView(PositionVsOffsetView())
  ```
  
  ![](/postionOffset.mov)

  - Circle에는 position 모디파이어를, Rectangle에는 offset 모디파이어를 사용했다.
  - SwiftUI의 레이아웃 과정을 생각하면서 생각해보면 이해가 쉽다.
  - border를 보면 position 모디파이어를 쓴 Circle은 부모 뷰가 제안한 영역을 모두 갖는다. 그 후 부모뷰 좌표 평면의 (x, y)에 뷰의 중앙점을 둔다.
  - offset 모디파이러를 쓴 Rectangle은 자식 뷰가 필요로 하는 영역만 갖는다. 그 후 원래 위치로부터 (x, y) 만큼 이동해 렌더링한다.
  - [Hacking with Swift](https://www.hackingwithswift.com/books/ios-swiftui/absolute-positioning-for-swiftui-views)에서는 전자와 후자를 각각 절대적 좌표와 상대적 좌표라고 설명하는데 의미 있는 설명인 것 같다.
  - [Naljin](https://sujinnaljin.medium.com/swiftui-offset-과-position-의-layout-단계-f0f6af736e18)은 레이아웃 과정에 따른 position과 offset의 차이를 아주 구체적으로 설명하고 있어서 도움이 된다.


- Vapor를 가지고 놀아보고 있다. 작은 서버 짜기에는 부족함이 없어보인다.
