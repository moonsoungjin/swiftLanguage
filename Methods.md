# 매소드(Methods)
- 특정 타입의 클래스, 구조체, 열형과 관련한 메소드드라고 한다
- 특정 타입의 인스턴스에서 실행할 수 있는 메소드를 인스턴스 메소드, 특정 형과 관련된 메소드를 타입 메소드라고 한다
- 타입 메소드는 Objective-C에서 클래스 메소드와 유사하다
- Swift에서 메소드가 C나 Objective-C 메소드 간의 가장 큰 차이는 바로 Objective-C에서는 클래스 타입에서만 메소드를 선언할 수 있다는 것이다<br>
Swift에서는 클래스 타입 뿐만아니라 구조체, 열거형에서도 메소드를 선언해 사용할 수 있다

### 인스턴스 메소드(Instance Methods)
인스턴스 메소드는 특정 클래스, 구조체, 열거형의 인스턴스에 속한 메소드이다
- 이 메소드를 통해 인스턴스 내의 값을 제어하거나 변경할 수 있다
- 인스턴스 메소드는 이름 그래로 그 인스턴스가 속한 특정 타입의 인스턴스에서만 실행 가능하다

```
class Counter {
    var count = 0
    func increment() {
        count += 1
    }
    func increment(by amount: Int) {
        count += amount
    }
    func reset() {
        count = 0
    }
}
```

Counter 클래스를 선언하고 인스턴스 메소드로 각각 increment(), increment(by amount: Int), reset() 를 정의해 인스턴스 내의 count property를 변경하는 기능을 수행한다

```
let counter = Counter() // 초기 count 값은 0 입니다
counter.increment() // count 값이 1로 변경 됐습니다
counter.increment(by: 5) // count 값은 현재 6입니다
counter.reset() // count 값은 0이 됩니다    
```

### self 프로퍼티(The self Property)
- 모든 프로퍼티는 암시적으로 인스턴스 자체를 의미하는 self라는 프로퍼티를 갖는다
- 인스턴스 메소드 안에서 self 프로퍼티를 이용해 인스턴스 자체를 참조하는데 사용할 수 있는다
```
func increment() {
    self.count += 1
}
```

- 앞의 예제에서는 increment() 메소드에서는 count += 1 이라 표현했지만 사실은 self.count += 1의 의미이다
- 이것이 가능한 이유는 Swift에서 특정 메소드에서 해당 인스턴스에 등록된 메소드나 프로퍼티를 호출하면 현재 인스턴스의 메소드나 프로퍼티를 사용하는 것으로 자동으로 가정하기 때문이다
- 위 규칙이 적용되지 않는 예외적인 상황이 있다(바로 인자 이름이 프로퍼티 이름과 같은 경우 이다)
- 이 경우에는 프로퍼티에 접근하기 위해 명시적으로 self키워드를 사용해야 한다

```
struct Point {
    var x = 0.0, y = 0.0
    func isToTheRightOf(x: Double) -> Bool {
        return self.x . x   // self.x를 이용해 프로퍼티 x와 인자 x를 구분
    }
}
let somePoint = point(x: 4.0, y: 5.0)
if somePoint.isToTheRightOf(x: 1.0) {
    print("This point is to the right of the line where x == 1.0")
}
// "This point is to the right of the line where x == 1.0" 출력
```

위 isToTheRightOf(x: Double)메소드에서 self.x를 사용해 프로퍼티 x와 인자 x를 구분함
만약 프로퍼티와 인자 이름이 같을때 self 키워드를 사용하지 않으면 Swift는 자동으로 인자 이름으로 가정한다

### 타입 메소드(Type Methods)
- 인스턴스 메소드는 특정 타입의 인스턴스에서 호출되고, 타입 메소드는 특정 타입 자체에서 호출해 사용함
- 타입 메소드의 선언은 메소드 키워드 func앞에 static 혹은 class 키워드를 추가하면된다
- static 메소드와 class 메소드의 차이점은 static 메소드 서브클래스에서 오버라이드 할 수 없는 타입 메소드 이고, class 메소드는 서브클래스에서 오버라이즈 할 수 있는 타입 메소드 라는것 이다
- Objective-C에서는 클래스 타입에서만 타입 메소드를 선언할 수 있던 것에 반해 Swift에서는 클래스, 구조체, 열거형에서 모두 타입 메소드를 사용할 수 있다
- 타입 메소드도 인스턴스 메소드와 같이 점(.)문법으로 호출할 수 있다(인스턴스에서 호출하는 것이 아니라 타입 이름에서 메소드를 호출한다)
```
class someClass {
    class func someTypeMethod() {
        // 타입 메소드 구현
    }
}
someClass.someTypeMethod()  // 타입 메소드 호출!
```
- 타입 메소드 안에서도 self 키워드를 사용할 수 있다
- 타입 메소드에서의 self는 인스턴스가 아니라 타입 자신을 의미한다
- 타입 메소드 안에서 다른 타입 메소드를 사용하는 것이 가능하다

```
struct LevelTracker {
    static var highestUnlockedLevel = 1
    var currentLevel = 1

    static func unlock(_ level: Int) {
        if level > highestUnlockedLevel { highestUnlockedLevel = level }
    }

    static func isUnlocked(_ level: Int) -> Bool {
        return level <= highestUnlockedLevel
    }

    @discardableResult
    mutating func advance(to level: Int) -> Bool {
        if LevelTracker.isUnlocked(level) {
            currentLevel = level
            return true
        } else {
            return false
        }
    }
}
```
타입 메소드 2개와 mutating 메소드 1개를 선언해 현재 레벨과, 최고 레벨, 다음 레벨 열기, 다음 레벨로 넘어가기 기능을 구현했다(게임에서 최고 레벨이 어디인지 추적하고 만약 그 레벨이 열려 있다면 그쪽으로 다른 사용자가 바로 넘어갈 수 있는(advance) 기능 제공)
advance 메소드 앞에 붙은 @discardableResult키워드는 리턴 값이 있는 메소드에서 리턴 값을 사용하지 않으면 컴파일러 경고가 발생하는데, 그 경고를 발생하지 않도록 도와준다
