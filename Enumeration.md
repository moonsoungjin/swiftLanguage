# 열거형(Enumeration)

- 열거형은 관련된 값으로 이루어진 그룹을 공통으의 형으로(type) 선언해 형 안전성(type-safety)을 보장하는 방법

## 기본 형태
- enum도 타입 이름이므로 이름은 UpperCamelCase로 적는다
- 각 case는 lowerCamelCase로 적는다
- 각각의 case가 고유의 형태로 취급이 된다
``` 
enum Weekday {
    case mon
    case tue
    case wed
    case thr, fri, sat, sun
}
```
- 한 줄씩 선언을 해도 되고, 한줄에 쉼표(,)로 구분해서 한번에 작성해도 상관없다<br>


eunm을 호출할 때에는 Int나 String 처럼 사용할 수 있다


```

var day: Weekday = Weekday.mon

```

호출한 이후부터는 열거형(enum)의 이름을 제외하고 작성하여도 무방하다

```
var day: Weekday = Weekday.mon
day = .tue
```

switch-case문을 활용하면 열거형 타입을 잘 활용할 수 있다.
열거형 타입은 한정되어 있다고 컴파일러가 인지하기 때문에 case에 해당하는 모든 경우를 구현한다면, default를 적지 않아도 된다! 하지만 case에서 하나라도 빼고 작성한다면 default가 꼭 필요하다

```
switch day {
    case .mon .tue .wed .thr:
        print("출근 ㅜㅜ")
    case .fri:
        print("불금!!")
    case .sat .sun:
        print("주말~~!~!")
}
```

## 원시값 활용하기

- 만약 c언어처럼 정수형 타입을 사용하고 싶다면 원시형을 선언해 주면 된다

```
enum Data: Int {
    case apple = 0
    case banana
    case grape
    //case lemon = 0 // case는 항상 다른 값을 가져야 한다(같은 값 X)
}
```

- 물론 정수타입 뿐만 아니라 String 등의 타입도 가능하다(hashable을 따르는 모든 타입들)
```
enum Color: String {
    case apple: "빨강"
    case banana: "노랑"
    case grape
}
```

- 이 때, 초기화가 된 값들은 호출 했을 때, 해당 값들을 불러오지만, 포도와 같이 초기화 되지 않은 값들은 case의 이름을 가져온다
```
print("사과의 색깔? \(Color.apple.rawValue)")
// 사과의 색깔? 빨강
print("포도의 색깔? \(Color.grape.rawValue)")
// 포도의 색깔? 포도
```

- 이렇게 rawValue에 값이 없는경우가 있기 때문에 변수로 사용하려면 옵셔널 타입으로 반환해 주어야 한다.
```
let fruit: Fruit? = Fruit(rawValue: 0)
```
- 아래처럼 if-let 구문을 활용해서 optional binding을 활용할 수 있다
```
if let fruit: Fruit(rawValue: 5) {
    print("색깔은 \(fruit)!")
} else {
    print("해당 과일이 없습니다.")
}
```
## 열거형 타입에 함수 넣기
- 마지막으로 Swift에서는 열거형 타입에 함수를 넣을 수 있다.
```
enum Month {
    case mar, apr, may
    case jun, jul, ang
    case sep, oct, nov
    case dec, jan, fed

    func printMessage() {
        swift self {
            case .mar, .apr, .may:
                print("봄!")
            case .jun, .jul, .aug:
                print("여름!")
        }
    }
}
```
