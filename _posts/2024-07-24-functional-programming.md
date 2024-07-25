---
layout: posts
title: "함수형 프로그래밍"
date: 2024-07-24 13:00:00 +0900
---

함수형 프로그래밍 언어를 사용하면 코드를 간결하게 작성할 수 있어 개발 시간을 단축할 수 있습니다.

또 부작용을 허용하지 않는 순수함수를 지향하여 동시에 여러 스레드에서 문제 없이 동작하는 프로그램을 쉽게 작성할 수 있습니다.

![Screenshot 2023-08-20 at 23.34.43.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5f2d3266-bcc0-47a2-839d-cbaa43decb9b/Screenshot_2023-08-20_at_23.34.43.png)

객체 지향 프로그래밍 패러다임은 객체를 중심으로 생각하고 작성하는 것입니다.

반면 **데이터를 함수로 연결하는 것을 중심으로 생각하고 프로그래밍 하는 것**을 **함수형 프로그래밍** 패러다임이라고 합니다.

## 특징

### 불변성(Immutability)

함수형 프로그래밍 언어는 **불변성**을 지향합니다. 따라서 변경 가능한 상태를 최대한 제거하려고 노력합니다.

**순수 함수**를 지향한다 설명하기도 하는데, 순수 함수는 **같은 입력에 대해 항상 같은 출력이 보장되는 함수**입니다.

불변성을 추구함에 따라 다음과 같은 여러 가지 장점을 가집니다.

- 프로그램의 검증이 쉽다.
    
    프로그램을 구성하는 모듈들이 오로지 입력 값의 영향만 받기 때문에 테스트 코드를 작성하기 쉽습니다.
    
    프로그래머가 예측하지 못하는 시점에 변경될 수 있는 내부의 상태가 없기 때문에 예측 가능해집니다.
    
- 최적화가 가능하다.
    
    이전에 계산한 함수의 값을 캐싱해 두었다가 필요할 때 다시 사용하는 메모이제이션은 함수의 불변성이 보장되지 않으면 불가능합니다.
    
    호출 할 때마다 결과 값이 달라지는 함수는 캐싱을 하는 것이 의미가 없기 때문입니다.
    
- 동시성 프로그램을 작성하기 쉽니다.
    
    동시성 프로그램을 작성하기 힘든 이유는 여러 스레드들이 프로그램의 상태를 공유하기 때문입니다.
    
    함수형 프로그래밍에서는 변경 가능한 상태를 원천적으로 배제하기 때문에 프로그래머는 동기화에 대한 골치 아픈 문제에서 벗어날 수 있습니다.
    

### 일급 객체(First-class) 함수, 고차 함수

함수형 프로그래밍에서 함수는 **일급 객체**입니다.

이 말은 함수를 변수에 할당할 수 있고, 다른 함수에 인자로 전달할 수 있으며, 반환값도 될 수 있는 **고차 함수**를 의미합니다.

함수를 하나의 값처럼 다룰 수 있기에 재사용할 수 있고, 핵심 코드를 boilerplate 없이 간단히 표현할 수 있습니다.

### 지연 연산(Lazy evaluation)

함수형 프로그래밍 언어는 **지연 연산**을 지원합니다.

지연 연산이란 어떤 값이 실제로 쓰이기 전까지 그 값의 계산을 최대한 미루는 것입니다.

**값을 미리 계산해서 저장하지 않기** 때문에 **공간을 절약**할 수 있고, **값이 꼭 필요할 때만 계산**하기 때문에 프로그램의 **성능에도 긍정적인 영향**을 줍니다.

지연 연산은 주로 메모이제이션과 함께 사용되는데, 필요할 때 계산을 수행하고 다음에 필요할 때 캐싱해 둔 값을 재사용 하는 것입니다.

## 구조체와 함수형 프로그래밍에 대한 해석

클래스의 인스턴스, 즉 객체는 정체성을 가집니다. 그리고 전달될 때 참조를 전달합니다.

따라서 **객체는 앱의 전반적인 상태에 영향을 줄 수 있고 복잡성을 증가시키기 때문에 사용에 주의**해야 합니다.

**정체성이 필요하지 않을 경우 클래스 대신 구조체를 활용해 모델링 하는 것을 권장**됩니다.

구조체의 인스턴스는 값을 복사해서 전달합니다.

따라서 복사된 값에 변화를 주더라도 원본에는 아무런 영향을 주지 않습니다.

그 덕에 앱 전반에 대해 크게 신경을 쓸 필요가 없습니다.

이러한 구조체의 특성은 함수형 프로그래밍에 적합합니다.

불변성을 생각하면, 같은 입력에 대해 같은 출력이 나와야 합니다.

**함수가 구조체 인스턴스를 다룬다면, 복사본이 전달될 것이고 범위 내에서 외부의 영향은 받지 않습니다.**

즉, 불변성이 보장되는 것이고 예측 가능한 결과값을 항상 얻을 수 있을 것입니다.

## 참조

[함수형 프로그래밍 언어에 대한 고찰](https://engineering.linecorp.com/ko/blog/functional-programing-language-and-line-game-cloud)