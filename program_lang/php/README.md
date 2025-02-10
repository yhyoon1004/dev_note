# PHP
> 1. PHP 및 Composer 설치
> 2. 언어 특징
> 3. 문법
> 4. 대표 라이브러리

---

## 1. PHP 및 Composer 설치
- php 파일 다운로드 
  1. `https://www.php.net/downloads.php` 해당 링크에서 OS에 맞는 php파일을 다운로드
  2. 압축을 풀고 해당 php실행파일(`php.exe`)이 있는 경로를 OS 환경 변수 path에 등록.   
     (실행파일 경로 안치고 실행하게)
  3. `php -v` 명령어로 설치 확인
- Composer(php 라이브러리 관리 도구) 설치
  1. `https://getcomposer.org/download/` 해당 링크에 설명하는 대로 설치
  2. php와 마찬가지로 설치된 `composer.phar` 파일이 있는 곳을 OS 환경 변수 path에 등록
  3. `composer -V`로 설치 확인  
  

- 여러 버전의 php를 사용하고 싶을 경우 각각의 버전의 php와 composer를 따로 설치하여  
  환경변수에 추가 등록하고 원하는 버전을 환경변수에 첫번째로 등록

---

## 2. 언어 특징
- 인터프리터 언어
  - 별도 컴파일 과정이 필요없어 수정 및 유지보수가 빠르고, 편리
  - 한번실행하고 나면 실행한 코드가 끝나고 변수나 데이터를 재사용하지않아 반복작업 비효율

---

## 3. 문법
```
# 변수 선언
$value1 = 123;
$value2 = 'abc';
$value3 = array();
$value4 = ['가','나'];

# 상수 선언
const fix_value = '가나다'

# 배열요소 접근
$data['type'] = "string";

# 객체요소(json, db 값) 접근 (stdClass사용 `->`를 이용하여 접근)
$obj = new stdClass();
$obj->name = "John";
$obj->age = 30;

# 클래스 요소의 메서드 실행
public MyClass { .. }
  ...
  $sample = new MyClass();
  $sample->myMethod();


# if문, 반복문, 스위치-케이스 일반 문법과 동일

# 객체, 배열, map 데이터와 같은 데이터 반복문 접근시
foreach ($data as $k => $v){ ... }

#접근 제어자
private, public , static .. 일반 문법 과 동일
```


---

## 4. 대표 프레임워크/라이브러리
- `Laravel` : REST_API, WEB 프레임워크 제공