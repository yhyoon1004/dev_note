# THREAD 

>1. 동시성 이슈
>2. `ThreadLocal`를 이용한 동시성 이슈 해결
>



## 1. 동시성 이슈
JAVA는 멀티 스레드를 지원하는 언어라 동시성 이슈가 발생할 수 있다.  
`동시성 이슈란?`  
보통 `전역변수`와 `싱글톤` 객체에서 자주 발생하며,  
동시의 여러요청이 발생해 동시간대 변수 저장작업이 겹쳐서 생긴다.
- `UserService.class`
  ```java
      @Component
      public class UserService{
          public String username = null;
          
          public String addUser(String name){
              this.username = name;
              return this.getUsername();
          }   
          
          public String getUsername(){
              return this.username;
          }
      }
  ```
- `MemberController.class`
  ```java
      @RequestMapping("/member")
      @RestController
      @RequiredArgsConstructor
      public class MemberController{
          private final UserService userService;
          @RequestMapping("/join")
          public String joinUser(@RequestParam String name){
              return userService.addUser(name);
          }
      }
  ```
위  와 같은 REST에서 UserService 클래스는 @Component로 인해 싱글톤 객체이다.  
그러므로 다른곳에서 UserService의 username에 접근가능한데  
문제는 여기서 발생한다.
동시에 '/member/join?name=kim','/member/join?name=sung' 순차적으로 요청이 왔을 때
자바의 멀티스레드 기능에 의해 thread1 ,thread2 가 작업을 각각 맡아 수행한다.  

- 초기 : `📦UserService.username= null`  

- 동시에 요청 :
  - `thread1` ➡️ addUser("kim"); `📦UserService.username = kim`  
  - `thread2` ➡️ addUser("sung"); `📦UserService.username = sung`  
- 서비스 처리 :  
  - `thread1` ➡️ getUsername(); `🧾sung`  
  - `thread2` ➡️ getUsername(); `🧾sung`  

> ⚠️이슈포인트 : `kim`을 저장하는 요청은 `sung`요청에 의해 값이 덮어쓰여 kim으로 반환되지 않음.


---
  
## 2. `ThreadLocal`를 이용한 동시성 이슈 해결
  Java의 ThreadLocal 객체를 이용하여 값을 저장시키면  
  ThreadLocal가 작업하는 스레드 별로 각각 분리해서 값을 저장시킴
- `ThreadLocalUserService.class`
  ```java
        @Component
        public class ThreadLocalUserService{
  //        public String username = null;
          public ThreadLocal<String> username = new ThreadLocal<>(); 
      
          public String addUser(String name){
  //            this.username = name;
              this.username.set(name);
              return this.getUsername();
          }   
          
          public String getUsername(){
  //            return this.username;
              return this.username.get();
          }
        }
  ```

- `ThreadLocalMemberController.class`
  ```java
      @RequestMapping("/member")
      @RestController
      @RequiredArgsConstructor
      public class ThreadLocalMemberController{
          
        private final ThreadLocalUserService threadLocalUserService;
          
        @RequestMapping("/join")
        public String joinUser(@RequestParam String name){
        //    return userService.addUser(name);
            String result =  threadLocalUserService.addUser(name);
            threadLocalUserService.username.remove();  //사용후 메모리 누수 방지를 위해 비워줌
        }
      }
  ```  
    
  

동시에 `/member/join?name=kim`, `/member/join?name=sung` 순차적으로 요청이 왔을 때

- 초기 : `📦ThreadLocalUserService.username = null`  

- 동시에 요청 :
    - `thread1` ➡️ addUser("kim"); `📦ThreadLocalUserService.username.thread1  = kim`
    - `thread2` ➡️ addUser("sung"); `📦ThreadLocalUserService.username.thread2 = sung`
- 서비스 처리 :
    - `thread1` ➡️ getUsername(); `🧾kim`
    - `thread2` ➡️ getUsername(); `🧾sung`  
- ThreadLocal 비우기 :
    - `thread1` ➡️ threadLocalUserService.username.remove();  
    `📦ThreadLocalUserService.username.thread1  = null`
    - `thread2` ➡️ threadLocalUserService.username.remove();  
    `📦ThreadLocalUserService.username.thread2 = null`  

