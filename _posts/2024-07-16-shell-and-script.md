---
layout: posts
title: "Shell과 Script"
date: 2024-07-16 15:00:00 +0900
---

Linux 환경의 쉘, 그리고 스크립트에 대해 알아봅시다.

## Shell

쉘은 사용자와 운영 체제 커널 사이의 인터페이스 역할을 하는 프로그램입니다. 사용자가 명령어를 입력하면, 쉘이 이를 해석하고 실행하여 결과를 반환합니다. 
쉘은 명령어를 직접 입력하여 사용하는 대화형 모드와 스크립트를 통해 여러 명령어를 한 번에 실행하는 비대화형 모드로 동작할 수 있습니다.

### 종류

여러 종류의 쉘이 있으며, 각 쉘은 고유한 기능과 문법을 제공합니다.

- Bourne Shell (sh): 가장 기본적인 쉘로, 많은 유닉스 시스템에서 기본 쉘로 사용되었습니다.
- Bash (Bourne Again Shell): GNU 프로젝트의 일환으로 개발된 쉘로, 현재 가장 널리 사용되는 쉘입니다. 리눅스 배포판에서 기본 쉘로 많이 사용됩니다.
- Korn Shell (ksh): 벨 연구소의 데이비드 콘(David Korn)이 개발한 쉘로, 많은 고급 기능을 제공합니다.
- C Shell (csh): C 프로그래밍 언어의 문법과 유사한 문법을 사용하는 쉘입니다.
- Z Shell (zsh): Bash와 Ksh의 기능을 결합한 쉘로, 많은 사용자 정의 기능을 제공합니다.

## Script

쉘 스크립트는 쉘 명령어들을 모아 놓은 텍스트 파일로, 쉘에서 해석되고 실행됩니다. 쉘 스크립트를 사용하면 반복 작업을 자동화하거나 복잡한 작업을 간단하게 수행할
수 있습니다. 스크립트는 일반적으로 .sh 확장자를 사용합니다.

### Shell Script의 기본 문법

1. 스크립트 시작

    쉘 스크립트는 일반적으로 `#!/bin/bash` 또는 해당 쉘의 경로를 첫 줄에 명시하여 시작합니다. 이 줄은 스크립트가 실행될 때 사용할 쉘을 지정합니다.

    ```bash
    #!/bin/bash
    ```

2. 주석

    쉘 스크립트에서 주석은 #으로 시작하며, 해당 줄의 나머지 부분은 무시됩니다. 주석은 코드 설명이나 메모를 작성하는 데 사용됩니다.

    ```bash
    # 이 스크립트는 Hello, World!를 출력합니다.
    echo "Hello, World!"
    ```
    
3. 변수

    쉘 스크립트에서는 변수를 선언하고 사용할 수 있습니다. 변수 값을 할당할 때는 공백을 사용하지 않습니다.

    ```bash
    #!/bin/bash

    name="User"
    echo "Hello, $name!"
    ```
    
4. 명령어 실행

    쉘 스크립트 내에서는 일반적인 쉘 명령어를 그대로 사용할 수 있습니다.

    ```bash
    #!/bin/bash

    # 현재 디렉토리의 파일 목록을 출력합니다.
    ls
    ```

5. 제어 구조

    쉘 스크립트에서는 조건문, 반복문 등의 제어 구조를 사용할 수 있습니다.

    조건문 (if 문):
    ```bash
    #!/bin/bash

    if [ "$name" == "User" ]; then
        echo "Welcome, User!"
    else
        echo "Who are you?"
    fi
    ```
    
    반복문 (for 문):
    ```bash
    #!/bin/bash

    for i in {1..5}; do
        echo "Iteration $i"
    done
    ```
    
    반복문 (while 문):
    ```bash
    #!/bin/bash

    count=1
    while [ $count -le 5 ]; do
        echo "Count: $count"
        count=$((count + 1))
    done
    ```
6. 함수

    함수를 정의하고 호출할 수 있습니다.

    ```bash
    코드 복사
    #!/bin/bash

    my_function() {
        echo "This is a function."
    }

    my_function
    ```
    
### 쉘 스크립트의 환경

1. 실행 권한

    쉘 스크립트를 실행하려면 실행 권한을 부여해야 합니다. `chmod` 명령어를 사용하여 실행 권한을 추가합니다.

    ```bash
    chmod +x script.sh
    ```
    
    스크립트를 실행하는 방법은 다음과 같습니다.

    ```bash
    ./script.sh
    ```
    
2. 환경 변수
    
    쉘 스크립트는 환경 변수를 사용할 수 있으며, 환경 변수는 스크립트 외부에서도 접근 가능합니다. export 명령어를 사용하여 환경 변수를 설정합니다.

    ```bash
    #!/bin/bash

    export MY_VAR="Hello"
    echo $MY_VAR
    ```
3. 입출력

    쉘 스크립트는 파일이나 사용자로부터 입력을 받을 수 있으며, 출력을 파일이나 화면에 전달할 수 있습니다.

    입력: read 명령어를 사용하여 사용자 입력을 받을 수 있습니다.
    ```bash
    #!/bin/bash

    echo "Enter your name:"
    read name
    echo "Hello, $name!"
    ```
    
    출력 리디렉션: > 또는 >>를 사용하여 출력을 파일로 리디렉션할 수 있습니다.
    ```bash
    #!/bin/bash

    echo "This will be written to a file." > output.txt
    echo "This will be appended to the file." >> output.txt
    ```
    
4. 명령어 치환

    명령어 치환을 사용하면 명령어의 출력을 변수에 저장하거나 다른 명령어의 인수로 사용할 수 있습니다.

    ```bash
    #!/bin/bash

    date=$(date)
    echo "Today's date is $date"
    ```
    
## 자동화

앞서 말했듯 쉘 스크립트는 리눅스 또는 유닉스 환경에서 반복적이고 복잡한 작업을 자동화하는 데 매우 유용합니다.

### 애플리케이션 배포 자동화 예시

쉘 스크립트를 사용하여 애플리케이션의 배포 과정을 자동화할 수 있습니다. 이를 통해 배포 시간과 오류를 줄일 수 있습니다.

```bash
#!/bin/bash

# 애플리케이션 배포 디렉토리 및 원격 저장소 설정
DEPLOY_DIR="/var/www/myapp"
REPO_URL="git@github.com:username/myapp.git"

# 기존 애플리케이션 백업
tar -czf $DEPLOY_DIR/backup_$(date +%Y-%m-%d_%H-%M-%S).tar.gz $DEPLOY_DIR

# 애플리케이션 업데이트
cd $DEPLOY_DIR
git pull $REPO_URL

# 애플리케이션 서비스 재시작
systemctl restart myapp

# 배포 완료 메시지 출력
echo "Application deployment completed."
```

### 크론(Cron)과의 연동

크론(Cron)은 유닉스 기반 시스템에서 작업을 일정 시간 간격으로 실행하기 위해 사용되는 시간 기반 작업 스케줄러입니다. 쉘 스크립트를 크론 잡(Cron Job)으로
등록하면 정기적으로 자동 실행할 수 있습니다.

- 크론 편집기 열기
    ```bash
    crontab -e
    ```

- 크론 잡 추가 (매일 자정에 스크립트 실행)
    ```bash
    0 0 * * * /path/to/script.sh
    ```

    각 `*` 문자는 작업이 실행될 시간과 주기를 나타냅니다. 각 필드는 다음 순서로 구성됩니다:

    - 분 (0-59)
    - 시간 (0-23)
    - 일 (1-31)
    - 월 (1-12)
    - 요일 (0-7, 0 또는 7은 일요일)

- 현재 사용자의 크론 작업 제거
    ```bash
    crontab -r
    ```

## 참고

[쉘 스크립트](https://www.shellscript.sh/)

[cron 스케줄](https://crontab.guru/)
