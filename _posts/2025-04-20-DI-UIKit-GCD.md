---
layout: post
title:  "DI 개선 고민, UIKit 작업, GCD 다시 보기"
date:   2025-04-20 18:00:00 +0900
tags:
    - til
---

## DI 개선 고민
- 챌린지2에서 DI를 적극적으로 사용하는데, 뷰에서 뷰 모델을 주입받을 때 불필요한 코드가 많았다. 최대한 구문이 간단하고 뷰가 컨테이너를 몰라도 되도록 리팩토링해봤다.
    - https://github.com/0tak2/SafeAreaBoard/issues/13#issuecomment-2817045944
    - https://github.com/0tak2/SafeAreaBoard/pull/28
    - 이제 뷰는 컨테이너를 직접 의존하지 않는다.

## UIKit
- 픽플즈 디자인 중 UILabel에서 텍스트가 프레임을 넘어가는 경우 "..."이 아니라 "...더보기"로 표시해야하는 부분이 있다. NSString의 `boundingRect(with:options:attributes:context:)`를 이용하면 전체를 렌더링하는데 필요한 공간의 크기를 알 수 있었다. 챗지피티와 함께 구현했다.
- https://github.com/Picplz/picplz-ios/issues/30#issuecomment-2817141351
- 참고 자료
    - https://developer.apple.com/documentation/foundation/nsstring/boundingrect(with:options:attributes:context:)
    - https://onemoonstudio.tistory.com/entry/iOS-Text-Size-구하기

## GCD
- 오늘 이사가 동시성 프로그래밍을 아주 열심히 공부하던데... 나도 겸사겸사 GCD와 Swift Concurrency를 같이 찾아봤다.
- GCD
    - libdispatch의 구현체
    - 작업을 태스크 단위로 큐에서 처리
- 네 가지 경우의 수가 가능하다.
    - SerialQueue.async
    - SerialQueue.sync
    - ConcurrentQueue.asnyc
    - ConcurrentQueue.sync
- SerialQueue
    - 단일 쓰레드를 사용한다.
    - 각 태스크가 순차적으로 수행된다.
- ConcurrentQueue
    - 멀티 쓰레드를 사용한다.
    - 각 태스크가 여러 쓰레드에 분배되어 동시에 수행될 수 있다.
- Sync
    - sync는 호출한 쪽의 쓰레드를 블로킹한다.
- Async
    - async는 호출한 쪽의 쓰레드를 블로킹하지 않는다.
- 직관적으로 이해가 잘 안 되었던 것
    - ConcurrentQueue + sync
        - ConcurrentQueue여봤자 sync로 제어권이 블로킹되면 의미가 있나? -> 결론: 의미가 있다
        - 예제 1
            ```swift
            @IBAction func performSyncOperation() {
                DispatchQueue.global().sync {
                    print("print statement 1")
                    DispatchQueue.global().sync {
                        print("print statement 2")
                    }
                    print("print statement 3")
                }
            }
            ```
            - 같은 큐에 대해 sync안에 sync가 중첩되어 디스패치되었지만 큐DispatchQueue.global()은 Concurrent 큐이기 때문에 실행에 문제가 없다. 다만 각 지점에서 블로킹될 뿐이다.
        - 예제 2
            ```swift
            DispatchQueue.global().async {
                queue.sync { // 1번
                    print("Task 1 started")
                    sleep(2) // 2초 대기
                    print("Task 1 finished")
                }
                print("say something") // 2번
            }

            DispatchQueue.global().async {
                queue.sync { // 1번
                    print("Task 1 started")
                    sleep(2) // 2초 대기
                    print("Task 1 finished")
                }
                print("say something") // 2번
            }

            DispatchQueue.global().async {
                queue.sync { // 1번
                    print("Task 1 started")
                    sleep(2) // 2초 대기
                    print("Task 1 finished")
                }
                print("say something") // 2번
            }
            ```
            - ChatGPT가 만들어준 예제. 병렬적으로 여러 작업을 수행하고자 하지만 내부적으로 순차진행이 필요한 경우 (여기서는 1번 -> 2번의 순서) 의미가 있겠다.
        - Kodeco 책의 정리
            - "In other words, a task being synchronous or not speaks to the source of the task. Being serial or concurrent speaks to the destination of the task."
            - 동기/비동기는 작업을 만든 쪽에 대한 이야기이고, 순차/동시성은 작업이 (디스패치되어) 도달한 곳에 대한 이야기이다.
- 참고할만한 자료
    - https://medium.com/@kkapilchoubisa/gcd-basics-serial-and-concurrent-queues-sync-vs-async-fdc3b22bbb3a
    - https://moonbc.com/2025/04/03/apple의-libdispatchgcd-효율적으로-사용하는-법/
    - https://github.com/swiftlang/swift-corelibs-libdispatch - 실제 구현체. C로 작성되었다.
- Swift Concurrency도 한 번 정리해보면 좋겠다. [여기](https://developer.apple.com/videos/play/wwdc2021/10132/)에서 시작하면 될 것 같다. TSPL에서는 [여기](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency)
