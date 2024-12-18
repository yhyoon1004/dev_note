---
[공식문서 링크](https://react.dev/reference/react/useRef#usage)

---

### **useRef 란?**
`값이 변해도 렌더링을 하지 않는` Hook으로 리액트가 제공하는 함수다.  
useRef로 초기화 한 변수의 값은 초기화해 준 `값을 반환해주는 것이 아닌 Ref 객체를 반환`한다.  
그러르모 Ref로 초기화해준 값을 얻기위해서는 `ref변수.current`로 초기화해 준 값을 얻을 수 있다.
---
### **사용법**
useRef는 react Hook이므로 함수의 최상단에 위치 선언하여 사용한다.  
useState와 같이 초기화하는 방법은 같다.

```
function MyComponent() {

  const examplelRef = useRef(0);
  const inputRef = useRef(null);
  consol.log("ref로 초기화해준 값 : ", exampleRef.current);
  ...
}
```

---

### **코드 예제**
```
import {useRef} from "react";

export default function Counter() {
    // useRef() Hook을 사용하여 변수 값 초기화
    let ref = useRef(0);

    //ref의 값을 조회하기 위한 함수
    function click() {
        alert('you clicked previous ' + ref.current + ' times!');
        ref.current = ref.current+1;
    }

    return (
        <>
            <button onClick={click}> Click Button </button>
            <hr/>
            <div>value = {ref.current}</div>
        </>
    );
};
```
위 코드에서 버튼 클릭시, alert 콘솔화면에 값이 1씩 증가하는 것을 볼 수 있지만 화면에서 `'value = 값' 는 변하지 않는 것`을 알 수 있다.
이는 `ref 값은 state 값과 다르게 값이 변경된다고 리렌더링(해당 컴포넌트 함수를 재호출)을 하지 않기 때문이다.`
---
### **주의할 점**



