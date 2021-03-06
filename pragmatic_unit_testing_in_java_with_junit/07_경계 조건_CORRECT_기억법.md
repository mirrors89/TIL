# 경계 조건: CORRECT 기억법
- 단위 테스트는 종종 겅계 조건들에 관계된 결함들을 미연에 방지하는데 도움이된다.
    - [C]onformance(준수): 값이 기대한 양식을 준수하고 있는가?
    - [O]rdering(순서): 값의 집합이 적절하게 정렬되거나 정렬되지 않았나?
    - [R]ange(범위): 이성적인 최솟값과 최댓값 안에 있는가?
    - [R]eference(참조): 코드 자체에서 통제할 수 없는 어떤 외부 참조를 포함하고 있는가?
    - [E]xistence(존재): 값이 존재하는가?
    - [C]ardinality(기수): 정확히 충분한 값들이 있는가?
    - [T]ime(절대적 혹은 상대적 시간): 모든 것이 순서대로 일어나는가? 정확한 시간에? 정시에?

## [C]ORRECT: [C]onformance(준수)
- 많은 데이터 요소가 특정 양식을 따라야 한다.
- 구조적 데이터의 경우 테스트 케이스를 조합하면 그 수가 폭발적으로 늘어나기도 합니다. (n * m)
- 기대하는 구조에 입력 데이터가 맞는지 확인해 볼 수 있는 방법을 더 많이 브래인스토밍하면 더욱더 잘할 수 있다.
- 시스템의 데이터 흐름을 이해하면 불필요한 검사를 최소화할 수 있다.

## C[O]RRECT: [O]rdering(순서)
- 데이터 순서 혹은 컬렉션에 있는 데이터 하나의 위치가 코드를 잘못 수행할 수 있다.

## CO[R]RECT: [R]ange(범위)
- 기본형의 과도한 사용에 대한 코드 냄새를 기본형 중독이라고 한다.
- 자바 같은 객체 지향 언어의 장점은 사용자 정의 추상화를 클래스로 만들 수 있다는 점.

- 불변식을 단언 형태로 넣을 수도 있습니다. @After 메서드를 추가하여 테스트가 완료되었을 때마다 확인할 수 있다.
### 불변성을 검사하는 사용자 정의 매처 생성
### 불변 메서드를 내장하여 범위 테스트

## COR[R]ECT: [R]eference(참조)
- 메서드를 테스트할 때 고려 사항
    - 범위를 넘어서는 것을 참고하고 있지 않은지
    - 외부 의존성은 무엇인가
    - 특정 상태에 있는 객체를 의존하고 있는가
    - 반드시 존재해야 하는 그 외 다른 조건들

- 어떤 상태에 대해 가정할 때는 그 가정이 맞지 않으면 코드가 합리적으로 잘 동작하는지 검사해야한다? 
    - 사전조건 - 어떤 가정이 맞는 상태
    - 사후조건 - 코드가 참을 유지해야하는 조건들
    - 사이드 이펙트 검사

## CORR[E]CT: [E]xistence(존재)
- 어떠한 예외가 발생했을 때 문제 원인을 이해하기 어려울 수 있다.
- 특정한 메시지를 예외에서 알려주면 준제를 추적하는 과정을 매우 단순하게 만들 수 있다.
- 행복 경로만 테스트 하지말고 불행 경로도 테스트하라
    - null을 반환
    - 기대하는 파일이 없음
    - 네트워크가 다운됨


## CORRE[C]T: [C]ardinality(기수)
- 0-1-n 법칙
    - 기수를 다루는 테스트에서는 0,1,n 이라는 경계조건에만 집중하고, n은 비즈니스 요구 사항에 따라 바꿔가며 테스트 한다.


## CORREC[T]: [T]ime(절대적 혹은 상대적 시간)
- 시간에 관하여 다음에 담아 두어야 할 측면 몇개는 다음과 같다.
    - 상대적 시간
    - 절대적 시간
    - 동시성 문제들

- 상대적 시간
    - 타임아웃 문제: 타임아웃을 설정하지 않으면 무한 대기에 빠질 수 있음.
- 절대적 시간
    - 벽시계 시간 : 시스템 시계에 의존하는 테스트를 작성하는것
- 동시성 문제들
    - 다수의 클라이언트 스레드를 보여주는 테스트를 작성해야함.
