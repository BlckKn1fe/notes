# JUnit

以下内容主要针对 JUnit 4，对于一些变动比较大的地方会加入 Junit 5 的内容

JUnit 4 的 POM 依赖

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.1</version>
    <scope>test</scope>
</dependency>
```

JUnit 5 的 POM 依赖

```xml
<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter-api</artifactId>
  <version>5.10.3</version>
  <scope>test</scope>
</dependency>

<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter-params</artifactId>
  <version>5.10.3</version>
  <scope>test</scope>
</dependency>

<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter-engine</artifactId>
  <version>5.10.3</version>
  <scope>test</scope>
</dependency>
```



## Annotation



### @Test

`@Test` 用在 `public void` 的方法上，被标记的 Method 会被自动 Run；该注解可以设置 `expected` 属性，用于测试 Exception

```java
public class ExceptionTest {
    @Test(expected = NullPointerException.class)
    public void nullPointerExceptionTest() {
        int[] arr = null;
        Arrays.sort(arr);
    }
}
```

该注解还有一个 `timeout` 的属性可以设置，用来测试 Performance，如果超时则 Fail

```java
public class PerformanceTest {
    @Test(timeout = 100)
    public void test() {
        int[] arr = {4, 2, 5, 1};
        for (int i = 0; i < 1000000; i++) {
            arr[1] = i;
            Arrays.sort(arr);
        }
    }
}
```



### @Before/@After 等

`@Before/@BeforeEach` 用在 `public void` 的方法上，被标记的 Method 会在其他被 `@Test` 标记的方法执行**之前**被运行；

`@After/@AfterEach` 用在 `public void` 的方法上，被标记的 Method 会在其他被 `@Test` 标记的方法执行**之后**被运行；

`@BeforeClass/@BeforeAll` 用在 `public void` 的方法上，被标记的 Method 只会在测试运行的**最开始执行一次**；

`@AfterClass/@AfterAll` 用在 `public void` 的方法上，被标记的 Method 只会在测试运行的**结束时执行一次**；



## Parameterized Test

通过 Annotation 来设置 Parameterized Unit Test method，JUnit 4 和 JUnit 5 的实现方法有很大不同，下面展示 JUnit 4 的方法

```java
@RunWith(Parameterized.class)
public class StringHelperParameterizedTest {

    StringHelper helper;

    String expectedOutput;
    String input;

    // 第一个参数对应数组中第一个元素，第二个参数对应数组中第二个元素
    public StringHelperParameterizedTest(String expectedOutput, String input) {
        this.input = input;
        this.expectedOutput = expectedOutput;
    }

    @Parameterized.Parameters
    public static Collection<String[]> parameters() {
        String[][] expectedOutputAndInput = {
            {"CD", "AACD"},
            {"CD", "ACD"},
            {"CDEF", "CDEF"},
            {"CDAA", "CDAA"},
            {"", "AA"},
        };

        return Arrays.asList(expectedOutputAndInput);
    }

    @Before
    public void setUp() throws Exception {
        helper = new StringHelper();
    }

    @Test
    public void testTruncateAInFirst2Positions() {
        assertEquals(this.expectedOutput, helper.truncateAInFirst2Positions(this.input));

    }

}
```

需要注意的几点：

1. Class 上需要标记 `RunWith(Parameterized.class)`
2. 准备参数的 Method 上需要标记 `@Parameterized.Parameters`，且方法必须为 `static` ，且返回一个 `Collection/Array`
3. Constructor 中形参的顺序需要和，准备参数的 Method 中的参数，的顺序对应，或直接使用 `@Parameter` 注解来做标记

这里有一个局限性，就是如果想要测试两个 Method 的话，就会出现两个 Source of Argument



在 JUnit 5 中，有很多更灵活的方法来提供参数

```java
public class MathUtilsTest {

    @ParameterizedTest(name = "{index} => add({0}, {1}) = {2}")
    @CsvSource({
        "1, 1, 2",
        "2, 3, 5",
        "4, 5, 9",
        "-1, -1, -2",
        "-2, 2, 0"
    })
    void testAdd(int a, int b, int expected) {
        MathUtils mathUtils = new MathUtils();
        assertEquals(expected, mathUtils.add(a, b));
    }
}
```

还有更多其他方法来提供参数，比如 `@ValueSource`，`@MethodSource`，`@CsvFileSource` 等，具体参考官方文档

https://junit.org/junit5/docs/current/user-guide/#writing-tests-parameterized-tests-sources





























