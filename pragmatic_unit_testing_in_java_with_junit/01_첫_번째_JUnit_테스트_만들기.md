# 첫 번째 JUnit 테스트 만들기

## 단위 테스트를 작성하는 이유
- 빠른 피드백으로 생산성 증가
- 단위 테스트들이 쌓이면 회기 테스트를 지원하게 된다.
- 로직의 문제점을 빠르게 찾을 수 있음

## JUnit의 기본: 첫 번째 테스트 통과

### 예제
```java
@FunctionalInterface
public interface Scoreable {
    int getScore();
}
```

```java
import java.util.ArrayList;
import java.util.List;

public class ScoreCollection {
    private List<Scoreable> scores = new ArrayList<>();

    public void add(Scoreable scoreable) {
        scores.add(scoreable);
    }

    public int arithmeticMean() {
        int total = scores.stream().mapToInt(Scoreable::getScore).sum();
        return total / scores.size();
    }
}
```

### JUnit 테스트

InelliJ 에서 mac 기준 `Command + Shift + T` 로 테스트 클래스를 쉽게 만들 수 있다.

```java
import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.Test;

class ScoreCollectionTest {

    @Test // JUnit은 @Test 애너테이션이 붙은 메소드는 테스트로 실행
    public void test() {
        fail("Not yet implemented"); // 실패하는 테스트 코드
    }
}
```


## 테스트 준비, 실행 단언

### 예제 
```java

import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.Test;

class ScoreCollectionTest {

    @Test
    public void answersArithmeticMeanOfTwoNumbers() {
        // 준비 arrange
        ScoreCollection collection = new ScoreCollection();
        collection.add(() -> 5);
        collection.add(() -> 7);
        
        // 실행 act
        int actualResult = collection.arithmeticMean();

        // 단언 assert
        assertEquals(actualResult, 6);
    }
}
```

- 테스트에서 어떤 것을 하기 위해서는 먼저 테스트 상태를 설정하는 준비(arrange) 단계의 일들을 해야한다.
- 테스트를 준비한 후에는 검증하려는 코드인 arithmeticMean 메서드를 실행(act)
- 마지막으로 기대하는 결과를 단언(assert)
- 실패한 단언문은 오류를 보고하는 것 이상의 일을 한다. 실패한 단언문을 지나치지 말자.

## 테스트가 정말로 뭔가를 테스트하는가?
- 의도하지 않게 생각하는 것을 실제로 검증 하지 않는 나쁜 테스트를 작성할 수도 있다.
- 항상 테스트가 실패하는지 확인하는 것을 고려.
- Test First

