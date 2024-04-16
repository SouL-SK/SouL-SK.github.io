
### 변경 전의 코드
```java
    public TestResponse getTestList(String param1, HttpRequest request) {

        TestResponse.Builder builder = TestResponse.newBuilder();

        List<TestInfo> testInfoList = new ArrayList<>();
        List<Object[]> testList = testRepository.findTestAndId(Long.valueOf(request.getTestId()));
        Map<TestResult, List<TestResultReply>> testResultMap = new HashMap<>();
        for (Object[] test : testList) {
            TestResult testResult = (TestResult) test[0];
            TestResultReply testResultReply = (TestResultReply) test[1];

            testResultMap.computeIfAbsent(testResult, k -> new ArrayList<>()).add(testResultReply);
        }

        for (Map.Entry<TestResult, List<TestResult>> entry : testResultMap.entrySet()) {
            List<TestResultInfo> testResultInfo = new ArrayList<>();
```

> JPA의 NativeQuery를 사용하여 Entity가 아닌 DTO (또는 VO) Interface를 사용하면 더 간결하게 작성할 수 있습니다.
>
> @Query ( value = "SQL문장", nativeQuery=true) 를 사용하면 Repository 내에서 DTO(또는VO) Interface를 사용할 수 있습니다.
>
> 2개의 서로다른 테이블을 한번에 join하여 조회하고 싶다면 nativeQuery를 사용하는 것도 좋은 방법 중의 하나 입니다.
> (이 경우 default인 JPQL과 문법이 다르니, 주의하셔야 합니다.)

### 변경 후의 코드

```java

```