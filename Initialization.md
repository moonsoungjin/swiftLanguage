# 초기화(Initialization)

### 초기화(Initialization)란?
- 클래스, 구조체, 열거형 인스턴스를 사용하기 위해 준비 작업을 하는 단계
- 이 단계에서 각 저장 프로퍼티의 초기 값을 설정한다
- 초기화 과정은 Initialization를 정의하는 것으로 구현 가능
- swift의 Initialization는 값을 반환하지 않음
- 초기화와 반대로 여러 값과 자원의 해지를 위해 deinitialization도 사용 가능

### 저장 프로퍼티를 위한 초기값 설정(Setting Initial Values for Stored Properties)
- 인스턴스의 저장 프로퍼티는 사용하기 전에 반드시 특정 값으로 초기화 돼야함
- 이 값은 기본 값을 설정할 수 있고, 특정 값도 설정할 수 있다
- initializer에서 프로퍼티에 값을 직접 설정하면 프로퍼티 옵저버가 호출되지 않고 값 할당이 수행된다

#### 이니셜라이저(Initializers)
- 이니셜라이저는 특정 타입의 인스턴스를 생성한다
- 이니셜라이저의 가장 간단한 형태는 파라미터가 없고 init 키워드르 사영하는 것이다

<pre>
<code>
init() {
    // perform some initialization here
}
예)
struct Fahrenheit {
    var temperature: Double
    init() {
        temperature = 32.0
    }
}
var f = Fahrenheit()
print("The default temperature is \(f.temperature)° Fahrenheit")
// Prints "The default temperature is 32.0° Fahrenheit"
</pre>
</code>