# 클로저(Closures)
- 클로저는 일정 기능을 하는 코드를 하나의 블록으로 모아놓은 것을 말한다

### 기본 클로저
- 기본 클로저 표현
```
{ (매개변수들) -> 반환 타입 in
    실행코드
}
```
- 클로저 예시
```
let names: [String] = ["zooneon", "mike", "minsoo", "bear"]

let reversed: [String] = names.sorted(by: { (first: String, second: String) -> Bool in return first > second 
})

print(reversed) // ["zooneon", "mike", "minsoo", "bear"]
```

### 후행 클로저
- 클로저가 조금 길어지거나 가독성이 떨어진다 싶을때 후행 클로저 기능을 사용하면 좋다
- 후행 클로저는 맨 마지막 전달인자로 전달되는 클로저에만 해당된다
- 단 하나의 클로저만 전달인자로 전달하는 경우에는 소괄호를 생략 해줄 수 있다
- 매개변수에 클로저가 여러 개 있는 경우, 다중 후행 클로저 문법을 사용할 수 있다

```
let names: [String] = ["zooneon", "mike", "minsoo", "bear"]

// 후행 클로저 사용
let reversed: [String] = names.sorted() { (first: String, second: String) -> Bool in return first > second 
}

// 소괄호까지 생략 가능
let revserved: [String] = names.sorted { (first: String, second: String) -> Bool in return first > second 
}

func doSomething(do: (String) -> Void, onSuccess: (Any) -> Void, onFailure: (Error) -> Void) {
    // do something...
}

// 다중 후행 클로저 사용
doSomething { (something: String) in
    // do closure
} onSuccess: { (result: Any) in 
    // success closure
} onFailure: { (error: Error) in
    // failure closure
}
```

### 탈출 클로저
- 함수의 전달인자 클로저가 함수 종료 후에 호출될 때 클로저가 함수를 탈출한다고 표현한다
- @escping 키워드를 사용하여 클로저가 탈출하는 것을 허용한다고 명시해줄 수 있다
- @escping 키워드를 따로 명시하지 않는다면 매개변수로 사용되는 클로저는 기본으로 비탈출 클로저이다
- 클로저가 함수 외부에 정의된 변수나 상수에 저장되어 함수가 종료된 후에 사용될 경우 함수를 탈출해 있어야 한다. (eg: 비동기 작업)
- 함수의 전달인자로 전달받은 클로저를 다시 반환할 때도 클로저가 함수를 탈출해 있어야 한다

```
typealias VoidVoidClosure = () -> Void
let AClosure: VoidVoidClosure = {
    print("Closure A")
}
let BClosure: VoidVoidClosure = {
    print("Closure B")
}

func returnOneClosure(A: @escaping VoidVoidClosure, B: @escaping VoidVoidClosure, shouldReturnAClosure: Bool) -> VoidVoidClosure {
    return shouldReturnAClosure ? A : B
}

let returnedClosure: VoidVoidClosure = returnOneClosure(A: AClosure, B: BClosure, shouldReturnAClosure: true)

returnedClosure()    //Closure A

var closures: [VoidVoidClosure] = []

func appendClosure(closure: @escaping VoidVoidClosure) {
    closures.append(closure)
}
```
-> 탈출할 수 있는 조건이 명확한 경우 @escping 키워드를 사용하지 않으면 컴파일 오류가 발생한다
```
typealias VoidVoidClosure = () -> Void

func functionWithEscapingClosure(completionHandler: @escaping VoidVoidClosure) -> VoidVoidClosure {
    return completionHandler
}

class SomeClass {
    var x = 10
    
    func runEscapingClosure() -> VoidVoidClosure {
        return functionWithEscapingClosure { self.x = 100 }
    }
}

let instance: SomeClass = SomeClass()
let returnedClosure: VoidVoidClosure = instance.runEscapingClosure()
returnedClosure()
print(instance.x)    //100
```
-> 탈출 클로저임을 명시한 경우, 클로저 내부에서 내부에서 해당 타입의 프로퍼티나 메서드 등에 접근하려면 self키워드를 명시적으로 사용해야 한다

#### withoutActuallyEscaping
- 비탈출 클로저로 전달한 클로저가 탈출 클로저인 척 해야 하는 경우에 사용한다

```
let numbers: [Int] = [2, 4, 6, 8]

let evenNumberPredicate = { (number: Int) -> Bool in
    return number % 2 == 0
}

let oddNumberPredicate = { (number: Int) -> Bool in
    return number % 2 == 1
}

func hasElements(in array: [Int], match predicate: (Int) -> Bool) -> Bool {
    return withoutActuallyEscaping(predicate, do: { escapablePredicate in
        return (array.lazy.filter { escapablePredicate($0) }.isEmpty == false)
    })
}

let hasEvenNumber = hasElements(in: numbers, match: evenNumberPredicate)
let hasOddNumber = hasElements(in: numbers, match: oddNumberPredicate)

print(hasEvenNumber)    //true
print(hasOddNumber)    //false
```
-> lazy 컬렉션에 있는 filter 메서드의 매개변수로 비탈출 클로저를 전달하는데, lazy 컬렉션은 비동기 작업을 할 때 사용하기 때문에 filter 메서드가 요구하는 클로저는 탈출 클로저 이다(withoutActuallyEscaping 함수 사용) 