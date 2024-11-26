# Shell Script

---

<aside>
💡 ShellScript란?
→ Unix 커맨드(명령어)들을 나열해서 실행하는 것.
언제 어떤 조건으로 명령을 실행시킬 것인지 등등..

</aside>

## 쉘 스크립트 작성법

- 기본적인 작성법
    
    ```bash
    #!/bin/sh
    echo "Hello, World!"
    ```
    
    - 쉘 스크립트 파일은 .sh 확장자로 설정한다.
    - 실제 코드를 작성하기 이전 첫 행은 #!/bin/sh를 작성한다.
    (시스템에게 지금부터는 셀 스크립트를 사용하는 것임을 알려주기 위해서)
    - 쉘 스크립트 실행을 위해 해야 하는 작업
    
    ```bash
    //실행 전 생성한 코드의 권한 설
    $ chmod 775 test.sh  
    //권한설정 
    //파일소유자(7)-그룹(5)-다른사용자(5)
    //7 -> 이진수 계산 "-rwx" = 0+4+2+1 => 7
    //5 -> 이진수 계산 "-rwx" = 0+4+0+1 => 5
    ```
    
    ```bash
    실행 명령어
    $ ./test.sh
    $ sh test.sh
    $ bash test.sh
    ```
    

---

## 쉘 스크립트 기본 명령어

- 주석
문단 처음에 #을 붙인다.
    
    ```bash
    #!/bin/sh
    # 주석 처리
    ```
    
- 입출력
출력 - echo
입력 - read
    
    ```bash
    #!/bin/sh
    read NAME
    echo "Hello, $NAME!"
    _____________________
    #실행 결과
    $./test.sh
    Yunhwan
    Hello, Yunhwan!
    ```
    
    ※ bash에서는 -e플래그로 특수문자 이스케이프 가능
    
    ```bash
    #!/bin/bash
    echo -e "hello\n$NAME!" # \n 개행 처리됨
    ```
    
- 변수
    - 변수 이름 - 영문, 숫자, “_ “를 사용하여 작성한다.
        
        ```bash
        #!/bin/bash
        Var_name1=yunhwan1
        var_Name2=yunhwan2
        ```
        
    - 변수에 값 초기화 시, =앞뒤에 공백이 없어야한다.
    문자열일 경우 “”(쌍따옴표)로 감싼다.
        
        ```bash
        #!/bin/bash
        var="value"
        var_num=6
        ```
        
    - 변수를 사용할 때는  변수 명 앞에  $를 넣는다.
    또는 $을 넣어서 변수를 {}로 감싼다.
        
        ```bash
        #!/bin/bash
        var01="value"
        echo ${var01}
        ```
        
    - 하나의 변수에 한 개의 값만 저장된다.
    - 변수의 값이 덮어 쓰이는 것을 방지할려면 readonly를 키워드를 사용한다.
        
        ```bash
        #!/bin/bash
        readonly var
        var="readonlyValue"
        ```
        
    - 변수를 unset 으로 삭제할 수 있다. (readonly변수는 삭제 불가능)
    
    | 변수 | 기능 |
    | --- | --- |
    | $0 | 스크립트명 |
    | $1 ~ $9 | 인수, 첫 번째의 인수는 $1, 2번째 인수는 $2로 액세스 |
    | $# | 스크립트에 전달된 인수의 수 |
    | $* | 모든 인수를 모아 하나로 처리 |
    | $@ | 모든 인수를 각각 처리 |
    | $? | 직전에 실행한 커맨드의 종료 값(0은 성공, 1은 실패) |
    | $$ | 이 쉘 스크립트의 프로세스 ID |
    | $! | 마지막으로 실행한 백그라운드 프로세스 ID |

```bash
#!/bin/sh
echo "\$0 (스크립트명)  : $0"
echo "\$1 (1번째 인수) : $1"
echo "\$2 (2번째 인수) : $2"
echo "\$# (인수의 수) : $#"
echo "\"\$*\": \"$*\""
echo "\"\$@\": \"$@\""
var="exit값은 0이 될 것이다."
echo $?
____________________________
실행 결과
$ ./test.sh first second 3rd
$0（스크립트 명）: test.sh
$1（1번째 인수）: first
$2（2번째 인수）: second
$3（3번째 인수）: 3rd
$#（인수의 수）: 3
"$*": "first second third"
"$@": "first second third"
0
```

- 특수 문자
    
    *** ? [ ' " ` \ $ ; & ( ) | ~ < > # % = space tab 개행** 과 같은 특수문자는 문자열로써 사용할 때 \ 를 앞에 붙여서 사용
    
- 변수 값의 치환
    
    
    | **문법** | **설명** |
    | --- | --- |
    | ${var} | 변수 값을 바꿔 넣는다. |
    | ${var:-word} | 변수가 아직 세팅되지 않거나 공백 문자열의 경우 word를 반환한다. |
    | ${var:=word} | 변수가 아직 세팅되지 않거나 공백 문자열을 word로 초기화한 후 반환한다 |
    | ${var:?word} | 변수가 아직 세팅되지 않거나 공백 문자열의 경우 치환에 실패하고, 스탠다드 에러에 에러가 표시된다. |
    | ${var:+word} | 변수가 세팅되지 않은 경우 word가 반환된다. var에는 저장되지 않는다.  |
    
    ```bash
    #!/bin/sh
    
    echo "1 - ${var:-wordSetInEcho1}"
    echo "2 - var = ${var}"
    echo "3 - ${var:=wordSetInEcho3}"
    echo "4 - var = ${var}"
    unset var
    echo "5 - ${var:+wordSetInEcho5}"
    echo "6 - var = $var"
    var="newVarValue"
    echo "7 - ${var:+wordSetInEcho7}"
    echo "8 - var = $var"
    echo "9 - ${var:?StandardErrorMessage}"
    echo "10 - var = ${var}"
    ________________________________
    실행결과
    
    1 - wordSetInEcho1
    2 - var = 
    3 - wordSetInEcho3
    4 - var = wordSetInEcho3
    5 - 
    6 - var = 
    7 - wordSetInEcho7
    8 - var = newVarValue
    9 - newVarValue
    10 - var = newVarValue
    ```
    
- 배열 (Bash)
    
    ```bash
    #!/bin/bash
    
    #bash shell로 배열을 작성하는 방법
    ARRAY=(item1 item2 item3 item4)
    ARRAY[0]="ITEM1"
    ARRAY[2]="ITEM3"
    
    echo "ARRAY[0]: ${ARRAY[0]}"
    echo "ARRAY[1]: ${ARRAY[1]}"
    
    #모든 아이템에 액세스
    echo "ARRAY[*]: ${ARRAY[*]}"
    echo "ARRAY[@]: ${ARRAY[@]}"
    
    ______________________________
    실행 결과
    
    $ ./test.sh
    ARRAY[0]: ITEM1
    ARRAY[1]: item2
    ARRAY[*]: ITEM1 item2 ITEM3 item4
    ARRAY[@]: ITEM1 item2 ITEM3 item4
    ```
    
- 연산자

| **연산자** | **의미** | **예제** |
| --- | --- | --- |
| + | 덧셈 | echo `expr 10 + 20` => 30 |
| - | 뺄셈 | echo `expr 20 - 10` => 10 |
| \* | 제곱 | echo `expr 11 \* 11` => 121 |
| / | 나눗셈 | echo `expr 10/2` => 5 |
| % | 나머지 | echo `expr 10 % 4` => 2 |
| = | 자정 | a=$b b의 값은 a에 저장된다 |
| == | 동일 | [ "$a" == "$b" ] $a과 $b가 동일하는 경우 TRUE가 반환된다. |
| ≠ | 다름 | [ "$a" != "$b" ] $a과 $b가 동일하지 않는 경우 TRUE가 반환된다. |
| -eq | 동일 | [ "$a" -eq "$b" ] 와 $a와 $b가 동일한 경우 TRUE가 반환된다. |
| -ne | 다름 | [ "$a" -ne "$b" ] $a와 $b가 동일하지 않은 경우 TRUE가 반환된다. |
| -gt | 보다 큼 | [ "$a" -gt "$b" ] $a가 $b보다 큰 경우 TRUE가 반환된다. |
| -lt | 보다 작음 | [ "$a" -lt "$b" ] $a가 $b보다 작은 경우 TRUE가 반환된다. |
| -ge | 보다 크거나 같거나 | [ "$a" -ge "$b" ] $a가 $b보다 크거나 같은 경우 TRUE가 반환된다. |
| -le | 보다 작거나 같거나 | [ "$a" -le "$b" ] $a가 $b보다 작거나 같은 경우 TRUE가 반환된다. |
| ! | ~가 아니다 | [ ! "$a" -gt "$b" ]$a가 $b보다 크지 않은 경우 TRUE가 반환된다. |
| -o | 어느쪽이든 | [ "$a" -gt "$b" -o "$a" -lt "$b" ]$a가 $b보다 크거나 작은 경우 TRUE가 반환된다. (Bash 확장 / POSIX폐지 예정) |
| -a | 양쪽 | [ "$a" -gt 90 -a "$a" -lt 100 ] $a가 90보다 크고 100보다는 작은 경우 TRUE가 반환된다. |
| -z | 문자열이 비었는 지 | [ -z "$a" ]$a에 어떤 것도 지정되지 않은 경우 TRUE가 반환된다. |
| -n | 문자열이 비었는 지 | [ -n "$a" ] $a에 어떠한 것이 지정되어 있다면 TRUE가 반환된다. |

---

## 쉘 스크립트 조건문

- if문 형식 : if [ 조건 ] then 명령문 fi
    
    조건이 참일 경우 명령어 실행
    
    조건과 다른 경우  elif [ 조건 ] 실행
    
    조건과 일하는 게 없을 경우 else 다음 명령어 실행 후 종료
    
    else가 없을 경우 그대로 종료
    
    ```bash
    #!/bin/sh
    if ["$1" -gt "$2" ]
    then
    	echo "1번째 인수가  2번째 인수보다 크다"
    elif ["$1" -eq "$2"]
    then
    	echo "1번째 인수와 2번째 인수가 동일하다"
    else
    	echo "1번째 인수가 2번째 인수보다 작다"
    fi
    ________________________
    실행 결과 
    $ ./test.sh 2 7
    1번째 인수가 2번째 인수보다 작다
    $ ./test.sh 10 5
    1번째 인수가 2번째 인수보다 크다
    $ ./test.sh 9 9
    1번째 인수와 2번째 인수가 동일하다
    ```
    
- switch문 형식 :  case 변수 in 조건|값 ) 명령문;; esac
    
    ```bash
    #!/bin/sh
    value="sung"
    case "$value" in 
    	"yun") echo "value is yun"
    	;;
    	"hwan") echo "value is hwan"
    	;;
    	"sung") echo "value is sung"
    	;;
    esac
    _________________________________
    실행 결과
    $ ./test.sh
    value is sung
    ```
    

## 쉘 스크립트 반복문

- while문
    
    ```bash
    #!/bin/sh
    a=0
    while [$a -lt 5]
    do
    	echo $a
    	a=`expr $a +1`
    done
    ____________________________
    실행 결과
    $ ./test.sh
    0
    1
    2
    3
    4
    ```
    
- until 문
    
    ```bash
    #!/bin/sh
    a=0
    until [!$a -lt 5]
    do
    	echo $a
    	a=`expr $a + 1`
    done
    _______________________
    실행 결과
    $ ./test.sh
    0
    1
    2
    3
    4
    ```
    
- for 문
    
    ```bash
    #!/bin/sh
    for var in 0 1 2 3 4 #범위의 작성법(bash) => {0,4}
    do
    	echo $var
    done
    __________________________
    실행결과
    $ ./test.sh
    0
    1
    2
    3
    4
    ```
    

## 쉘 스크립트 함수 작성법

```bash
#!/bin/sh

#함수를 작성, 함수 정의 생성
MyFunction(){
	echo "함수의 echo 호출"
}
MyParamFunc(){
	echo "인수1:$1 인수2:$2"
}
#함수 호출
MyFunction
MyParamFunc param1 param2
___________________________
함수 실행 결과
$ ./test.sh
함수의 echo 호출
인수1:param1 인수2:param2

```

![Untitled](Shell%20Script%20f5d9cdc30ad44b08918dab9a8678b364/Untitled.png)

---

<aside>
💡 참고

[[Linux] 쉘 스크립트(Shell script) 기초](https://engineer-mole.tistory.com/200)

</aside>
