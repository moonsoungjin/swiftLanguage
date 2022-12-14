# 제네릭(Generic)
- 제네릭이란 타입에 의존하지 않는 범용 코드를 작성할때 사용한다
- 제네릭을 사용하면 중복을 피하고, 코드를 유연하게 작성할 수 있다
- Swift에서 가장 강력한 기능 중 하나로 Swift표준 라이브러리의 대다수는 제네릭으로 선언되어있다
- Array와 Dictoinary 또한 제네릭 타입이다

### 제네릭이 풀려고 하는 문제
- 아래와 같이 두 Int 값을 바꾸는 함수가 있다
- 인자로 넣은 두 개의 파라미터는 inout파라미터이다, 그래서 두 값의 원본을 변경하게 된다

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```
선언한 함수 swapTwoInts를 선언하면 입력한 두 Int값이 변경된 것을 확인할 수 있다

```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// Prints "someInt is now 107, and anotherInt is now 3"
```
Int형이 아니라 String 값을 변경하려면 swapTwoStrings라는 함수를 새로 선언해야 한다<br>
값이 Double이면 이것도 마찬가기로 swapTwoDouble라는 새 함수를 선언해야 한다

```swift
func swapTwoStrings(_ a: inout String, _ b: inout String) {
    let temporaryA = a
    a = b
    b = temporaryA
}

func swapTwoDoubles(_ a: inout Double, _ b: inout Double) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```
각 함수는 인자값의 타입만 다르고 함수의 내용은 동일하다<br>
제네릭을 사용하면 이렇게 인자값의 타입만 다르고 수행하는 기능이 동일한 것을 하나의 함수로 만들 수 있다

- 위 세 함수에서 모두 a와 b의 타입이 동일해야 한다 만약 a와 b가 동일하지 않다면 두 값을 바꿔치기 할 수 없다 swift는 타입-세이프 언어라 String과 Double를 바꾸려고 하면 컴파일 에러가 발생한다
<br><br>
### 제네릭 함수(Generic Functions)

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```
Swift는 실제 실행하는 타입 T가 어떤 타입인지 보지 않는다
swapTwoValues 함수가 실행되면 T에 해당하는 값을 함수에 넘긴다
```swift
func swapTwoInts(_  a: inout Int, _ b: inout Int)
func swapTwoValues<T>(_ a: inout T, _ b: inout T)
```
실행 시키면 기대했던 대로 동작하는 것을 확인할 수 있다
```swift
var someInt = 3
var anotherInt = 107
swapTwoValues(&someInt, &anotherInt)
// someInt is now 107, and anotherInt is now 3

var someString = "hello"
var antherString = "world"
swapTwoValues(&someString, &anotherString)
// someString is now "world", and anotherString is now "helli"
```
제네릭의 예로 생성한 swapTwoValues 함수는 Swift에서 swap 기본적으로 제공한다
<br><br>
### 타입 파라미터(Type Parameters)
위에서 사용한 플레이스 홀더 T는 타입 파라키터의 예이다
타입 파라미터는 플레이스 홀더 타입의 이름을 명시하고 함수명 바로 뒤에 적어준다. 그리고 와 같이 꺽쇄로 묶어 준다. 타입 파라미터를 한번 선언하면 이 것을 함수의 타입으로 사용할 수 있다. 복수의 타입 파라미터를 사용할때는 와 같이 콤마로 구분해준다.
<br><br>
### 파라미터 이름짓기(Naming Type Parameters)
Dictionary의 Key, Value와 같이 엘리먼트 간의 서로 상관관계가 있는 경우 의미가 있는 이름을 파라미터 이름으로 붙이고 그렇지 않은 경우는 T,U,V와 같은 단일 문자로 파라미터 이름을 짓는다
- 항상 파라미터 이름은 T나 MyTypeParameter와 같이 대문자 카멜 케이스로 이름 짓는다. 대문자로 된 이름은 값(vlaue)이 아니라 타입(type)을 의미한다
<br><br>
### 제네릭 타입(Generic Types)
제네릭 함수에 추가로 Swift에서는 제네릭 타입을 정의할 수 있다. 이후 섹션에서는 Stack이라는 제네릭 콜렉션 타입을 어떻게 구현하는지 보여줄 예정이다
- 스택은 UINavigationController 에서 사용하는 자료구조이다 네이게이션 계층에 뷰 컨트롤러를 pushViewController와 popViewController를 이용해 추가하거나 뺄 수 있다
<br><br>
### 제네릭 타입의 확장(Extending a Generic Type)
익스텐션을 이용해 제네릭 타입을 확장할 수 있다. 이때 원래 선언한 파라미터 이름을 사용한다. 여기서 Element 하는 파라미터를 사용한다
```swift
extensuion Stack {
    var topItem: Element? {
        return items.isEmpty ? nil : items[items.count - 1]
    }
}
```
익스텐션으로 추가한 topItem 프로퍼티에 접근한다
```swift
if let topItem = stackOfStrings.topItem {
    print("The top item on the stack is \(topItem).")
}
// Prints "The top item on the stack is tres."
```
실행 결과 스택의 최상단 값을 확인할 수 있다