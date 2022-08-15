# Optional

- java8부터 지원하는 `Optional class`
- optional<T>는 null이 올 수 있는 값을 감싸는 Wrapper 클래스로, 참조하더라도 `NPE(NullPointerException)`가 발생하지 않도록 도와줌

> NPE
> - 개발할 때 가장 많이 발생하는 예외 중 하나
> - NPE를 피하기 위해서 null 검사 로직을 추가해야 하는데, null 검사를 해야 하는 변수가 많은 경우 코드가 복잡해지고 로직이 번거로워지는 문제가 발생함
> - 코드 예시
> 
> ```
> List<String> names = getNames(); names.sort(); // names가 null이라면 NPE가 발생함
> 
> List<String> names = getNames(); // NPE를 방지하기 위해 null 검사를 해야함 
> 
> if(names != null){ 
>     names.sort(); 
> }
> 
> ```

- Optional 클래스는 아래와 같은 value에 값을 저장하기 때문에 값이 null이더라도 바로 NPE가 발생하지 않음
- 각종 메소드를 제공

```
public final class Optional<T> { // If non-null, the value; if null, indicates no value is present 

private final T value; 
... }

```

> 참고 사이트
> - https://hbase.tistory.com/212
> - https://homoefficio.github.io/2019/10/03/Java-Optional-%EB%B0%94%EB%A5%B4%EA%B2%8C-%EC%93%B0%EA%B8%B0/