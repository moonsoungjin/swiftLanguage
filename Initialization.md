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

### 기본 프로퍼티(Default Property Values)
- 프로퍼티의 선언과 동시에 값을 할당하면 그 값을 초기 값으로 사용할 수 있다
- 항상 같은 초기 값을 갖는 다면 기본 프로퍼티를 사용하는 것이 좋다
- 프로퍼티에 타입을 선언하지 않아도 컴파일러는 초기 값을 참조해서 타입을 추론할 수 있다(이 기본값은 상속시 함께 상속 된다)
```
struct Fahrenheit {
    var temperature = 32.0
}
```
위 코드는 프로퍼티를 선언과 동시에 초기 값을 할당한 예제

### 옵셔널 프로퍼티 타입(Optional Property Types)
- 프로퍼티의 최초 값이 없고 나중에 추가될 수 있는 값을 옵셔널로 선언해 사용할 수 있다(옵셔널 프로퍼티는 자동으로 nil로 초기화 된다)
```
class SurveryQuestion {
    var text: String
    var response: String?
    init(text: String) {
        self.text = text
    }
    func ask() {
        print(text)
    }
}

let cheeseQuestion = SurveyQuestion(text: "Do you like cheese?")
cheeseQuestion.ask()
// prints "Do you like cheese?"
cheeseQuestion.response = "Yes, I do like cheese."
```

### 기본 이니셜라이저(Default Initializers)
- 만약 모든 프로퍼티의 초기 값이 설정돼 있고, 하나의 초기자도 정의하지 않았다면 Swift는 모든 프로퍼티를 기본 값으로 초기화 하는 기본 초기화를 제공해 준다
- 아래의 코드는 이니셜라이저를 정의하지 않았지만 Swift가 제공하는 기본 이니셜라이저 ShoppingListItem() 를 사용할 수 있음을 보여주는 예제이다
```
class ShoppingListItem {
    var name: String?
    var quantity = 1
    var purchased = fales
}
var item = ShoppingListItem()
```

#### 구조체 타입을 위한 맴버쪽 이니셜라이저(Memberwise Initializers for Structure Types)
- 기본 이니셜라이저와 다르게 멤버쪽 이니셜라이저는 프로퍼티가 기본 값이 없어도 커스텀 이니셜라이저를 정의하지 않았다면 멤버쪽 이니셜라이저를 제공해 준다
- 이 초기자는 선언한 모든 프로퍼티를 인자로 사용한다
```
struct Size {
    var width = 0.0, height = 0.0
}
let twoByTwo = Size(width: 2.0, height: 2.0)
```

### 클래스 상속과 초기화(Class Inheritance and Initialzation)
- 모든 클래스의 저장 프로퍼티와 superclass로부터 상속받은 모든 프로퍼티는 초기화 단계에서 반드시 초기값이 할당돼야 한다
- Swift에서는 클래스 타입에서 모든 프로퍼티가 초기 값을 갖는 것을 보장하기 위해 2가지 방법을 지원한다

#### 지정 초기화와 편리한 초기자(Designated Initializres and Convenience Initializers)
- 지정 초기화는 클래스의 주(primary) 초기자이다
- 지정 초기자는 클래스의 모든 프로퍼티를 초기화 한다
- 클래스 타입은 반드시 한개 이상의 지정 초기자가 있어야 한다
- 편리한 초기화는 초기화 단계에서 미리 지정된 값을 사용해서 최소한의 입력으로 초기화를 할 수 있도록 해주는 초기화이다
- 편리한 초기자 내에서 반드시 지정 초기자가 호출돼야 한다

#### 지정 초기자와 편리한 초기자의 문법(Syntax for Designated and Convenience Initializers)
<br>
클래스의 지정 초기자의 문법은 값 타입 초기자와 같다


```
init(parameters) {
    statements
}
```
편리한 초기자는 기본 초기자와 문법이 같지만 init 앞에 convenience 키워드를 붙여 준다
```
convenience init(parameters) {
    statements
}
```
