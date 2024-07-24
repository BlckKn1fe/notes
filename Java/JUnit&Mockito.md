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



## Basic Test



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



## Suit Test

Suit Test 可以选择 a set of unit test 同时运行，在 JUnit 4 中使用方法如下

```java
@RunWith(Suite.class)
@Suite.SuiteClasses({
    ArrayCompareTest.class,
    ExceptionTest.class
})
public class AllTest { }
```

在 JUnit 5 中提供了更多了配置 Suit 的方式，并且注解清晰明确

https://junit.org/junit5/docs/current/user-guide/#junit-platform-suite-engine-example



# Mockito 4

官方文档：

https://javadoc.io/doc/org.mockito/mockito-core/4.11.0/org/mockito/Mockito.html



## Stub

有关 Stub 的概念，引入 ChatGPT 给出的回复

> 在单元测试中，stubs 是一种模拟对象，用于提供控制测试环境所需的固定行为或数据。与 mocks 不同，stubs 通常不关注方法的调用情况，**而是用于提供预定义的响应或数据**。

通过 Mockito 的 mock 方法可以定义某个 method 的具体返回值，下面用一段代码举例

```java
public class User {
    private String firstName;
    private String lastName;
	// Constructor, Getters, Setters...
}

public interface UserRepository {
    User findById(int userId);
}

public class UserService {
    private UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public String getUserFullName(int userId) {
        User user = userRepository.findById(userId);
        return user.getFirstName() + " " + user.getLastName();
    }
}
```

当我们想测试 `UserService` 的返回结果的时候

```java
public class UserServiceTest {
	
    // Set fields...SetUp...

    @Test
    public void testGetUserFullName() {
        // 设置 stub 的行为
        User user = new User("John", "Doe");
        when(userRepository.findById(1)).thenReturn(user);

        // 调用被测方法
        String fullName = userService.getUserFullName(1);

        // 验证结果
        assertEquals("John Doe", fullName);
    }
}
```

Stub Method 有更多用法，比如 `thenReturn` 可以进行链式调用，定义某个 Method 第 N 次调用时候返回的 value

```java
@Test
public void ListTest() {
    List<Integer> list = mock(List.class, "Integer");
    when(list.size()).thenReturn(1).thenReturn(10);

    assertEquals(1, list.size());
    assertEquals(10, list.size());
}
```

他还可以使用 `thenThrow` 来抛出异常，`thenAnswer` 来更加高度的定义被测试方法的行为



在 `when` method 中，可以手动给予一个 fixed argument，也可以使用 Mockito 提供的 Argument Matcher，比如说 List 中的 `get` 方法需要一个 Integer 作为 input，假设需求是无论传给 `get` 任何整数的时候，都 return 一个 `“value”` 字符串

```java
@Test
public void ListTest() {
    List<String> list = mock(List.class);
    when(list.get(anyInt())).thenReturn("value");

    assertEquals("value", list.get(0));
    assertEquals("value", list.get(99));
}
```

Mockito 还提供更多的 Argument Matcher

https://javadoc.io/doc/org.mockito/mockito-core/4.11.0/org/mockito/ArgumentMatchers.html



## Verify

Verify 可以用来判断某个方法，有没有被正常的调用、调用（至少/至多）几次、或者从来没有被调用过。

比如 List 方法的 add，可能有调用过 `list.add(“Hello”)`，但是没有调用过 `list.add(“Bye”)` ，就可以通过 Verify 来验证，更多的例子和使用参考官方文档：

https://javadoc.io/doc/org.mockito/mockito-core/4.11.0/org/mockito/Mockito.html#4





## ArgumentCatpor

它可以用在 Verify 的过程里，获取某个 method 被调用的时候所传给的参数

假如我们以下代码

```java
public class UserController {
    private EmailService emailService;

    public UserController(EmailService emailService) {
        this.emailService = emailService;
    }

    public void welcomeNewUser(String userEmail) {
        String message = "Welcome to our service!";
        emailService.sendEmail(userEmail, message);
    }
}
```

然后我们想测试 `sendEmail` 有没有被正确的调用并且获取我们给的参数

```java
public class UserControllerTest {

    @Test
    public void testWelcomeNewUser() {
        // 创建 EmailService 的 mock 对象
        EmailService emailService = mock(EmailService.class);
        UserController userController = new UserController(emailService);

        // 调用方法
        userController.welcomeNewUser("test@example.com");

        // 捕获参数
        ArgumentCaptor<String> recipientCaptor = ArgumentCaptor.forClass(String.class);
        ArgumentCaptor<String> messageCaptor = ArgumentCaptor.forClass(String.class);

        // 验证 sendEmail 方法被调用并捕获参数
        verify(emailService).sendEmail(recipientCaptor.capture(), messageCaptor.capture());

        // 断言捕获的参数值
        assertEquals("test@example.com", recipientCaptor.getValue());
        assertEquals("Welcome to our service!", messageCaptor.getValue());
    }
}
```

它还可以 Capture 对象类型的参数，更多使用参考文档

https://javadoc.io/doc/org.mockito/mockito-core/4.11.0/org/mockito/Mockito.html



## Hamcrest Matcher

Hamcrest 提供了更加强大的、灵活的判定规则，比如在 Assert 的时候可以判断 Value 是否大于、小于某个范围；判断集合大小、集合中是否存在某些 Value 等。

Hamcrest 使用 `assertThat` 方法

```java
@Test
public void testHamcrest() {
    // Collection
    List<Integer> list = Arrays.asList(101, 102, 103, 104, 105);

    assertThat(list, hasSize(5));
    assertThat(list, hasItems(101, 102));
    assertThat(list, everyItem(greaterThan(100)));
    assertThat(list, everyItem(lessThan(110)));

    // String
    String str = "Hello";
    assertThat(str, hasLength(5));
    assertThat(str, not(emptyOrNullString()));

    // Array
    Integer[] arr = {1, 2, 3};
    assertThat(arr, arrayWithSize(3));
    assertThat(arr, arrayContaining(1, 2, 3));
    assertThat(arr, arrayContainingInAnyOrder(2, 3, 1));
}
```

官方 Java API Doc：

https://hamcrest.org/JavaHamcrest/javadoc/2.2/



## Mockito Annotation

有四个比较方便的注解：`@Mock`，`@InjectMocks`，`@Captor`，`@RunWith(MockitoJUnitRunner.class)`，这些注解会自动的 Inject 依赖，和 Spring 中的 `@Autowire` 差不多

```java
@RunWith(MockitoJUnitRunner.class)
public class MockitoAnnotationTest{
    
    @Mock
    UserRepository userRepositoryMock;
    
    @InjectMocks  // 它会自动找到 UserRepository 的 Bean 并且注入进来，如果有多个相同依赖，可以考虑手动注入
    UserService userService;
    
    @Captor
    ArgumentCaptor<Integer> userIdArgCaptor
    
}
```

如果说，又想使用 Parameterized Test 又想使用 Mockito Annotation 的话，可以通过 `@Before` 注解来实现

```java
@Before
public void setUp() {
    try (AutoCloseable mocks = MockitoAnnotations.openMocks(this)) {
		// Other initialization code...
    } catch (Exception e) {
        System.out.println(e.getMessage());
    }
}
```

还有一个办法是添加下面这个注解在 Class 的 fileds 中，要求 JUnit 版本在 4.7 及以上

```java
@Rule
public MockitoRule mockitoRule = MockitoJUnit.rule();
```



## Spy

`spy` 和 `mock` 方法都是 Mockito 提供的，`mock` 说的简单点就是完全的屏蔽掉原来 Class 的 Logic，然后自己重新定义每个 method；但是 `spy` 会保留原本的 Class 的逻辑的同时，并不影响我们 stub 其中的某个 method，是一种 Partial Mock

```java
@Test
public void spyTest() {
    List list = spy(ArrayList.class);
    // 保留原有逻辑
    list.add(1);
    assertThat(list.size(), equalTo(1));
	
    // Stub
    when(list.size()).thenReturn(10);
    assertThat(list.size(), equalTo(10));
}
```

不太推荐使用 Spy，主要是因为它可能会造成测试的复杂性和可靠性。



## Mock Static

Mockito 从 3.4 之后就支持 mock static method 了，之前需要用 PowerMock 这个框架，但是这个框架于两年多之前就停止更新了，并且它似乎与 Mockito 4 之后的版本有冲突

Mockito 中测试静态方法使用方法大致如下

```java
@Test
public void testStaticMethod() {
    // 也可以创建 StatiMock 对象之后，在后面手动 close 掉
    try (MockedStatic<MyStaticClass> mockedStatic = Mockito.mockStatic(MyStaticClass.class)) {
        // 这里本质是传一个 Answer 的实现
        staticMock.when(() -> MyStaticClass.staticMethod().thenReturn("mocked result");
        assertEquals("mocked result", MyStaticClass.staticMethod());
    }
}
```



## Mock Private/Constructor

Mockito 暂时不支持，有新的方案再更新



























