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

## Git

Git은 분산 버전 관리 시스템으로, 소스 코드의 변경 사항을 추적하고 여러 개발자 간에 협업을 가능하게 합니다.

### 주요 개념

- 저장소(Repository): 코드와 변경 이력을 저장하는 곳입니다. 로컬 저장소(사용자의 PC)와 원격 저장소(Git)가 있습니다.
- 커밋(Commit): 파일 변경 사항을 기록하는 스냅샷입니다. 각 커밋은 고유한 해시값으로 식별됩니다.
- 브랜치(Branch): 독립적인 개발 선을 의미합니다. 기본 브랜치는 보통 main 또는 master이며, 기능 개발이나 버그 수정을 위해 별도의 브랜치를 생성할 수 있습니다.
- 머지(Merge): 두 개의 브랜치를 하나로 합치는 작업입니다. 충돌이 발생할 경우 수동으로 해결해야 합니다.
- 리베이스(Rebase): 브랜치의 기반을 다른 브랜치의 최신 커밋으로 변경하는 작업입니다. 커밋 히스토리를 깔끔하게 유지할 수 있습니다.
- 푸시(Push): 로컬 저장소의 변경 사항을 원격 저장소에 업로드합니다.
- 풀(Pull): 원격 저장소의 변경 사항을 로컬 저장소로 가져옵니다.

### 일반적인 과정

1. 일반적으로 `init` 혹은 `clone`으로 환경을 구축합니다.
2. 작업을 수행하고, `add`로 파일을 스테이지로 올린 후 `commit`을 수행합니다.
3. `commit`한 내용을 `push`로 원격 저장소에 반영합니다.
4. 협업자의 변경 사항을 `pull`로 반영합니다.

## Gist

Gist는 GitHub에서 제공하는 서비스로, 코드 스니펫이나 메모와 같은 작은 파일들을 쉽게 공유하고 관리할 수 있도록 해줍니다. Gist는 Git을 기반으로 작동합니다.
Gist는 Git을 기반으로 하지만, 용도와 기능 면에서 몇 가지 중요한 차이점이 있습니다.

### 용도
Git: 소프트웨어 프로젝트의 전체 소스 코드를 버전 관리하고 협업하는 데 사용됩니다.
    
Gist: 코드 스니펫, 작은 스크립트, 메모 등을 공유하고 관리하는 데 사용됩니다.

### 저장소 구조
Git: 일반적인 Git 저장소는 폴더 구조를 가지며, 여러 파일과 디렉토리를 포함할 수 있습니다.
    
Gist: Gist는 단일 파일 또는 소수의 파일들로 구성되며, 간단한 구조를 가집니다.

### 협업 기능
Git: 브랜칭, 병합, 풀 리퀘스트 등을 통해 복잡한 협업을 지원합니다.
    
Gist: 포크와 간단한 편집 기능을 통해 협업을 지원하지만, 복잡한 협업 기능은 없습니다.

## 참고 문헌

[Apple Developer Documentation - Debugging](https://developer.apple.com/documentation/xcode/debugging)

[Git Pro Book](https://git-scm.com/book/ko/v2)
