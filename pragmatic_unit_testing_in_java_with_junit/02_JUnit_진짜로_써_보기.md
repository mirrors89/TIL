# JUnit 진짜로 써 보기

## 테스트 대상 이해

### 예제
```java
public class Profile {
    private Map<String, Answer> answers = new HashMap<>();
    private int score;
    private String name;

    public Profile(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void add(Answer answer) {
        answers.put(answer.getQuestionText(), answer);
    }

    public boolean match(Criteria criteria) {
        score = 0;

        boolean kill = false;
        boolean anyMatches = false;
        for(Criterion criterion : criteria) {
            Answer answer = answers.get(criterion.getAnswer().getQuestionText());
            boolean match = criterion.getWeight() == Weight.DontCare || answer.match(criterion.getAnswer());

            if(!match && criterion.getWeight() == Weight.MustMatch) {
                kill = true;
            }
            if(match) {
                score += criterion.getWeight().getValue();
            }
            anyMatches |= match;
        }
        if(kill) {
            return false;
        }
        return anyMatches;
    }

    public int score() {
        return score;
    }
}
```


## 어떤 테스트를 작성할 수 있을지 결정
- 얼마나 많은 테스트를 작성해야 하는지 생각
- 테스트를 작성하고 나면 코드가 실제로 어떻게 동작하는지 더 잘 이해할 수 있다.
- 테스트를 작성할 때는 가장 신경 쓰는 부분이 어디인지 알고 있어야 한다.

## 단일 경로 커버
```java
class ProfileTest {

    @Test
    void matchAnswersFalseWhenMustMatchCriteriaNotMet() {
        // 설정
        Profile profile = new Profile("Bull Hockey, Inc.");
        Question question = new BooleanQuestion(1, "Got Bonuses?");
        Answer profileAnswer  = new Answer(question, Bool.FALSE);
        profile.add(profileAnswer);

        Criteria criteria = new Criteria();
        Answer criteriaAnswer = new Answer(question, Bool.TRUE);

        Criterion criterion = new Criterion(criteriaAnswer, Weight.MustMatch);
        criteria.add(criterion);

        boolean matches = profile.match(criteria);
        assertFalse(matches);
    }
}
```

## 두 번째 테스트 만들기
```java
class ProfileTest {

    @Test
    void matchAnswersTrueForAnyDontCareCriteria() {
        // 설정
        Profile profile = new Profile("Bull Hockey, Inc.");
        Question question = new BooleanQuestion(1, "Got milk?");
        Answer profileAnswer  = new Answer(question, Bool.FALSE);
        profile.add(profileAnswer);

        Criteria criteria = new Criteria();
        Answer criteriaAnswer = new Answer(question, Bool.TRUE);

        Criterion criterion = new Criterion(criteriaAnswer, Weight.DontCare);
        criteria.add(criterion);

        // 실행
        boolean matches = profile.match(criteria);

        // 단언
        assertTrue(matches);
    }
}
```

## @Before 메서드로 테스트 초기화
```java
class ProfileTestWithBefore {

    private Profile profile;
    private BooleanQuestion question;
    private Criteria criteria;
    
    @BeforeEach
    public void create() {
        profile = new Profile("Bull Hockey, Inc.");
        question = new BooleanQuestion(1, "Got Bonuses?");
        criteria = new Criteria();
    }
    
    @Test
    void matchAnswersFalseWhenMustMatchCriteriaNotMet() {
        // 설정
        Answer profileAnswer  = new Answer(question, Bool.FALSE);
        profile.add(profileAnswer);
        
        Answer criteriaAnswer = new Answer(question, Bool.TRUE);
        Criterion criterion = new Criterion(criteriaAnswer, Weight.MustMatch);
        criteria.add(criterion);

        // 실행
        boolean matches = profile.match(criteria);

        // 단언
        assertFalse(matches);
    }

    @Test
    void matchAnswersTrueForAnyDontCareCriteria() {
        // 설정
        Answer profileAnswer  = new Answer(question, Bool.FALSE);
        profile.add(profileAnswer);

        Answer criteriaAnswer = new Answer(question, Bool.TRUE);
        Criterion criterion = new Criterion(criteriaAnswer, Weight.DontCare);
        criteria.add(criterion);

        // 실행
        boolean matches = profile.match(criteria);

        // 단언
        assertTrue(matches);
    }
}
```
- ProfileTest 클래스의 모든 테스트 코드에 공통적인 초기화 코드가 중복으로 가지고 있다.
- @Before를 이용해 중복 로직을 분리할 수 있다.
- 테스트를 실행 할 때 마다 @Before 메소드를 먼저 실행한다.
- JUnit은 테스트마다 새로운 인스턴스를 생성한다.

## 이제 어떤가?
- 일이 좀 더 단순해지도록 코드를 구조화 하는 방법을 생각하자. 
- 추후에 테스트를 더 단순하게 작성할 수 있도록 정기적으로 정리하자.
