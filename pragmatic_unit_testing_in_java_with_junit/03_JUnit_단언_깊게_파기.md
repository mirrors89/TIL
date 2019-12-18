# JUnit 단언 깊게 파기

## JUnit 단언

- 어떤 조건이 참인지 검증하는 방법, 단언한 조건이 참이 아니면 그 자리에서 멈추고 실패를 보고한다.

### assertTrue
- 단언한 조건이 참인지 확인

```java
private Account account;

@BeforeEach
public void createAccount() {
    account = new Account("an account name");
}

@Test
public void hasPositiveBalance() {
    account.deposit(50);
    assertTrue(account.hasPositiveBalance());
}

@Test
public void depositIncreasesBalance() {
    int initalBalance = account.getBalance();
    account.deposit(100);
    assertTrue(account.getBalance() > initalBalance);
}
```

### assertThat은 명확한 값을 비교
```java
// assertJ
Assertions.assertThat(account.getBalance()).isEqualTo(100);
Assertions.assertThat(account.getName()).startsWith("xyz");
Assertions.assertThat(account.getName()).isNotEqualTo("plunderings");

// Null 체크
Assertions.assertThat(account.getName()).isNull();
Assertions.assertThat(account.getName()).isNotNull();

// 부동 소수정
Assertions.assertThat(2.32*3).isCloseTo(6.96d, Offset.offset(0.0005));
```


## 예외를 기대하는 세 가지 방법

### 단순한 방식: 애너테이션 사용
- 기대한 예외를 지정할 수 있는 인자를 제공
- JUnit5에서 사라짐
```java
// JUnit4
@Test(expected=InsufficientFundsException.class)
public void throwsWhenWithDrawingTooMuch() {
    account.withdraw(100);
}
```

### 옛 방식: try/catch와 fail
- 
```java

@Test
public void exceptionRule() {
    try {
        account.withdraw(100);
        fail();
    } catch(InsufficientFundsException expected) {
        Assertions.assertThat(expected.getMessage()).isEqualsTo("balance only 0");
    }
}
```



### 새로운 방식: ExpectedException 규칙
- 커스텀 규칙(Role)을 정의하여 테스트가 실행되는 흐름동안발생하는 일에 대한 더 큰 통제권을 부여.
- JUnit5에서 사라짐

```java
// JUnit4
@Rule
public ExpectedException thrown = ExpectedException.none();

@Test
public void exceptionRule() {
    thrown.expect(InsufficientFundsException.class);
    thrown.expectMessage("balance only 0");

    account.withdraw(100);
}

// JUnit5
@Test
public void exceptionRule() {
    assertThrows(InsufficientFundsException.class, () -> {
        account.withdraw(100);
    }, "balance only 0");
}

```


### 예외 무시
- 검증된 예외를 처리하려고 테스트코드에 try/catch 블록을 넣지 말고 예외를 다시 던져라.
- 테스트 실패가 아니라 테스트 오류로 보고한다.
