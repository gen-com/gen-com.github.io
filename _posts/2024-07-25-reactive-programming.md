---
layout: posts
title: "반응형 프로그래밍"
date: 2024-07-25 13:00:00 +0900
---

반응형 프로그래밍(Reactive Programming)은 소프트웨어 개발 패러다임 중 하나로, 데이터 스트림과 변경 사항의 전파를 쉽게 처리할 수 있도록 설계되었습니다.

# 왜 ?

- 비동기 처리의 필요성

    현대 애플리케이션은 사용자 인터페이스의 반응성을 유지하면서 네트워크 요청, 타이머 등 다양한 비동기 작업을 수행해야 합니다. 기존의 동기적 프로그래밍 모델로는
    이러한 비동기 작업을 효율적으로 관리하기 어려웠습니다.

- 콜백 지옥

    비동기 작업을 처리하기 위해 콜백 함수를 사용하는 방식은 코드의 가독성을 크게 떨어뜨리고, 유지보수를 어렵게 만들었습니다. 반응형 프로그래밍은 이를 해결하기
    위한 대안으로 등장했습니다.
    
- 데이터 스트림 처리의 필요성

    실시간 데이터 스트림을 효율적으로 처리하는 필요성이 증가했습니다. 반응형 프로그래밍은 이러한 데이터 스트림을 자연스럽게 모델링하고 처리할 수 있는 방법을
    제공합니다.

이러한 배경을 통해 반응형 프로그래밍은 현대 소프트웨어 개발에서 중요한 역할을 차지하게 되었습니다. 이를 통해 개발자들은 비동기 작업을 더 효율적으로 처리하고,
코드의 가독성과 유지보수성을 높일 수 있게 되었습니다.

# 동기와 비동기

## 동기 프로그래밍 (Synchronous)

동기 프로그래밍에서는 하나의 작업이 완료될 때까지 다음 작업을 시작하지 않습니다. 즉, 작업이 순차적으로 진행됩니다. 각 작업이 완료될 때까지 프로그램 실행이
멈추며, 다음 작업은 이전 작업이 끝난 후에 시작됩니다. 그러다보니 코드의 흐름이 직관적이고 디버깅이 상대적으로 쉽습니다.

하지만 파일 읽기/쓰기, 네트워크 요청 등 시간이 오래 걸리는 작업이 있을 경우, 해당 작업이 완료될 때까지 프로그램이 블록됩니다. 이는 사용자 인터페이스가
멈추거나 응답하지 않는 상황을 초래할 수 있습니다.

## 비동기 프로그래밍 (Asynchronous)

비동기 프로그래밍에서는 작업이 완료될 때까지 기다리지 않고, 다른 작업을 계속 진행합니다. 작업이 완료되면 결과를 처리하는 콜백 함수나 약속(Promise)
등을 통해 결과를 처리합니다.

시간이 오래 걸리는 작업이 비동기적으로 처리되므로, 해당 작업이 진행되는 동안 다른 작업을 계속 수행할 수 있습니다. 이는 사용자 인터페이스가 더 반응성을
유지할 수 있게 합니다.

## 콜백 지옥

```swift
import Foundation

// 비동기 작업을 시뮬레이션하는 함수
func asyncTask(taskName: String, completion: @escaping (String) -> Void) {
    let delay = Int.random(in: 1...3)
    DispatchQueue.global().asyncAfter(deadline: .now() + .seconds(delay)) {
        completion("\(taskName) 완료됨")
    }
}

// 콜백 지옥 예제
func performTasks() {
    asyncTask(taskName: "Task 1") { result1 in
        print(result1)
        
        asyncTask(taskName: "Task 2") { result2 in
            print(result2)
            
            asyncTask(taskName: "Task 3") { result3 in
                print(result3)
                
                asyncTask(taskName: "Task 4") { result4 in
                    print(result4)
                    
                    asyncTask(taskName: "Task 5") { result5 in
                        print(result5)
                        
                        // 이와 같은 방식으로 콜백이 계속 중첩될 수 있습니다.
                    }
                }
            }
        }
    }
}

performTasks()

```

콜백이 호출될 때마다 무한 들여쓰기가 진행됩니다.

## Combine을 통한 해결

```swift
import Foundation
import Combine

// 비동기 작업을 시뮬레이션하는 함수
func asyncTask(taskName: String) -> Future<String, Never> {
    return Future { promise in
        let delay = Int.random(in: 1...3)
        DispatchQueue.global().asyncAfter(deadline: .now() + .seconds(delay)) {
            promise(.success("\(taskName) 완료됨"))
        }
    }
}

// Combine을 사용하여 콜백 지옥 해결
var cancellables = Set<AnyCancellable>()

func performTasksWithCombine() {
    asyncTask(taskName: "Task 1")
        .flatMap { result1 -> Future<String, Never> in
            print(result1)
            return asyncTask(taskName: "Task 2")
        }
        .flatMap { result2 -> Future<String, Never> in
            print(result2)
            return asyncTask(taskName: "Task 3")
        }
        .flatMap { result3 -> Future<String, Never> in
            print(result3)
            return asyncTask(taskName: "Task 4")
        }
        .flatMap { result4 -> Future<String, Never> in
            print(result4)
            return asyncTask(taskName: "Task 5")
        }
        .sink { result5 in
            print(result5)
        }
        .store(in: &cancellables)
}

performTasksWithCombine()
```

1. `Future`: asyncTask 함수는 이제 `Future<String, Never>`를 반환합니다. `Future`는 비동기적으로 값 하나를 제공하는 `Combine` 퍼블리셔입니다.
2. `flatMap`: 각 비동기 작업을 연결하기 위해 `flatMap` 연산자를 사용합니다. 이는 각 작업이 완료된 후 다음 작업을 시작할 수 있게 합니다.
3. `sink`: 최종적으로 `sink` 연산자를 사용하여 스트림의 결과를 구독하고 출력합니다.
4. `Set<AnyCancellable>`: `Combine` 퍼블리셔를 구독하기 위해 `cancellables` 집합을 사용하여 메모리 관리를 합니다.

코드가 구조화되고 선언식으로 작성되어 가독성이 더 좋습니다.

# 메소드에서 데이터의 관점으로

일반적으로 우리는 메소드(함수)의 관점에서 데이터를 처리했습니다. 물론 이와 같은 방식도 좋지만 복잡한 상황에서 함수가 이리저리 꼬이게 되면 역시 머리가 아파집니다.

![메소드의 관점](https://gist.github.com/user-attachments/assets/85d30b41-8a0b-40a3-a4ee-7eae0146d5a0)

그래서 관점을 한 번 옮겨볼 필요가 있습니다.

> 데이터의 관점에서 한 번 생각해 보는 것은 어떤가요 ?

![데이터 관점](https://gist.github.com/user-attachments/assets/aef4e945-a024-4d8b-a5fc-b0625e5c25fa)

위의 그림을 보면 데이터와 그 옆에 긴 흐름(선)이 있습니다. 데이터의 변화가 생기면 흐름을 따라서 흘러 갑니다.

밑에 있는 구독자(모듈)들이 자신에게 필요한 값에 대해 구독을 하고 데이터를 받아서 보도록 합니다.

> **데이터는 그저 흘러가기만 하면 되고, 관심있는 모듈은 알아서 받고 처리하면 됩니다 !**

아주 단순하지만 강력한 구조가 됩니다.
