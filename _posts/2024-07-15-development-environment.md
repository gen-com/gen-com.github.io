---
layout: posts
title: "iOS 개발을 위한 환경"
date: 2024-07-15 13:00:00 +0900
---

## IDE는 무엇인가 ?

프로그래밍 개발을 하는 사람이라면 IDE라는 단어를 많이 들어봤을 겁니다~~(OO 코딩 테스트에서는 외부 IDE 사용이 불가능합니다)~~. IDE는 `통합 개발 환경
`(Integrated Development Environment)을 의미하며, 개발을 위한 도구 모음을 제공하는 소프트웨어 응용 프로그램입니다. IDE는 개발자의 생산성을 높이고,
코드의 품질을 향상시키며, 개발 과정을 체계적으로 관리할 수 있도록 도와줍니다.

### IDE의 주요 구성 요소

- 소스 코드 편집기
    구문 강조, 자동 완성, 코드 포맷팅 등의 기능을 제공하여 코드 작성 과정을 돕습니다.
- 컴파일러 또는 인터프리터
    소스 코드를 기계어로 변환하여 실행 가능하게 합니다.
- 디버거
    프로그램의 실행을 제어하고, 중단점 설정, 변수 검사 등을 통해 오류를 찾고 수정할 수 있게 합니다.
- 버전 관리 시스템 통합
    Git과 같은 버전 관리 시스템과 연동되어 코드의 변경 사항을 추적하고 관리할 수 있습니다.

### Xcode

Apple에서 제공하는 IDE로, macOS와 iOS 등의 애플리케이션 개발에 사용됩니다.

## 디버깅

디버깅은 프로그램에서 발생할 수 있는 오류를 찾아내고 수정하는 데 매우 중요합니다.

### 브레이크포인트 설정

![debug](https://docs-assets.developer.apple.com/published/490252326a8f9805eb9fde4456d57996/setting-breakpoints-to-pause-your-running-app-1@2x.png)

- 추가: 소스 코드의 왼쪽 가장자리에 있는 줄 번호 옆을 클릭하면 파란색 화살표(브레이크포인트)가 나타납니다. 
이는 코드 실행이 해당 지점에서 중단됨을 의미합니다.

- 관리: 브레이크포인트를 오른쪽 클릭하여 설정을 조정하거나 삭제할 수 있습니다.

![breakpoint](https://docs-assets.developer.apple.com/published/0599792a0f39a1675e2fc12a06197687/stepping-through-code-and-inspecting-variables-to-isolate-bugs-1@2x.png)

브레이크포인트를 설정한 상태에서 앱을 실행하고, 해당 부분에 도달하면 앱이 멈춥니다.

### 스텝 디버깅

- Step Over: 함수 호출은 실행하지만 함수 내부로 들어가지 않고 다음 줄로 이동합니다.
- Step Into: 함수 내부로 들어가서 한 줄씩 실행합니다.
- Step Out: 현재 함수의 실행을 마치고 호출한 함수로 돌아갑니다.
- Continue: 다음 브레이크포인트 또는 프로그램 종료 시점까지 실행을 계속합니다.

![step debugging](https://docs-assets.developer.apple.com/published/870c41e7250ac061a333c1ec9977c41a/stepping-through-code-and-inspecting-variables-to-isolate-bugs-2@2x.png)

### LLDB 명령어 사용

LLDB (LLVM Debugger)는 강력한 디버깅 도구입니다. 콘솔에서 다양한 명령어를 사용하여 프로그램 상태를 제어할 수 있습니다.

- `po` (Print Object): 객체의 상태를 출력합니다.
- `bt` (Backtrace): 호출 스택을 출력합니다.
- `frame variable`: 현재 프레임의 변수를 출력합니다.

## 참고 문헌

[Apple Developer Documentation - Debugging](https://developer.apple.com/documentation/xcode/debugging)
