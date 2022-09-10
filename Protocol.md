# Protocol(프로토콜)

## 프로토콜이란?
- 특정 역할을 하기 위한 메소드, 프로퍼티, 기타 요구사항 등의 청사진

### 프로토콜의 사용
- 구조체, 클래스, 열거형은 프로토콜을 채택해서 특정 기능을 실행하기 위한 프로토콜의 요구사항을 실제로 구현할 수 있다
- 프로토콜은 정의를 하고 제시를 할 뿐 스스로 기능을 구현하지는 않는다(조건만 정의)
- 하나의 타입으로 사용되기 때문에 아래와 같이 타입 사용이 허용되는 모든 곳에 프로토콜을 사용할 수 있다
- 함수, 메소드, 이니셜라이저의 파라미터 타입 혹은 리턴 타입
- 상수, 변수, 프로퍼티의 타입
- 배열, 딕셔너리의 원소타입

기본 형태
```
protocol 프로토콜이름 {
    // 프로토콜 정의
}
```
구조체, 클래스, 열거형 등에서 프로토콜을 채택하려면 타입 이름 이름 뒤에 콜론":"을 붙여준 후 채택할 프로토콜 이름을 쉼표","로 구분하여 명시해준다.(SubClass의 경우 SuperClass를 가장 앞에 명시한다)
```
struct SomeStruct: AProtocol, AnotherProtocol {
    // 구조체 정의
}

// 상속받는 클래스의 프로토콜 채택
class SomeClass: SuperClass, AProtocol, AnotherProtocol {
    // 클래스 정의
} 
```

### 프로퍼티 요구사항(Property Requirments)
- 프로토콜에서는 프로퍼티가 저장 프로퍼티인지 연산프로퍼티인지 명시하지 않고, 이름과 타입 그리고 gettable,settable한지 명시한다(프로퍼티는 항상 var로 선언해야 한다)
```
protocol Student {
    var height: Double { get set }
    var name: String { get }
    static var schoolNumber: Int { get set }
}
```
- 해당 프로토콜은 학생의 키, 이름, 학번을 정의하였고, 이를 체택하는 타입은 해당 프로퍼티를 구현해줘야한다
```
class Aiden: Student {
    var roundingHeight: Double = 0.0
    var height: Double {
        get {
            return roundingHeight
        }
        set {
            roundingHeight = 183.0
        }
    }
    var name: String = "Aiden"
    static var schoolNumber: Int = 20112330
}

let aiden = Aiden()
print(aiden.height, aiden.name, Aiden.schoolNumber)
// 0.0 Aiden 20112330
aiden.height = 183.0
print(aiden.height, aiden.name, Aiden.scoolNumber)
// 183.0 Aiden 20112330
```
- 프로퍼티는 저장 프로퍼티나 연산 프로퍼티 둘다 사용해서 구현할 수 있다

### 메소드 요구사항(Method Requirements)
: 프로토콜에서는 인스턴스 메소드와 타입 메소드를 정의할 수 있다 , 메소드 파라미터의 기본 값은 프로토콜 안에서 사용할 수 없다

- 메소드를 정의할때 함수명과 반환값을 지정할 수 있고, {}는 적지 않는다
- mutating 키워드를 사용해 인스턴스에서 변경 가능하다는것을 표시할 수 있다(값 타입에서만 사용 가능)
```
protocol Person {
    static func breathing()
    func sleeping(time: Int) -> Bool
    mutating func running()
}
```
```
struct Aiden: Person {
    var heartRate = 100
    static func bareathing() {
        print("숨을 쉽니다")
    }
    func sleeping(time: Int) -> Bool {
    if time >= 23 {
        return true
    } else {
        return false
    }
}

    mutating func running() {
        heartRate += 20
    }
}

print(Aiden.breathing())
// 숨을 쉽니다

var aiden = Aiden()
print(aiden.sleeping(time: 23))
// true

print(aiden.heartRate)
// 100
aiden.running()
print(aiden.heartRate)
// 120
```

### 변경 가능한 메소드 요구사항(Mutating Method Requirements)
mutating 키워드를 사용해 인스턴스에서 변경 가능하다는 것을 표시할 수 있다(mutating 키워드는 값타입 형태에만 사용)
- 프로토콜에 mutating을 명시한 경우 이 프로토콜을 따르는 클래스 형을 구현할 때는 메소드에 mutating을 명시하지 않아도 된다

```
protocol Togglable {
    mutating func toggle()
}
```
프로토콜을 따는 값ㅏ입 형에서 toggle() 메소드를 변경해 사용할 수 있다
```
enum OnOffSwitch: Togglable {
    case off, on
    mutating func toggle() {
        switch self {
        case .off:
            self = .on
        case .on:
            self = .off
        }
    }
}
var lightSwitch = OnOffSwitch.off
lightSwitch.toggle()
// lightSwitch is now equal to .on 
``` 

### 타입으로써의 프로토콜(Protocols as Types)
프로토콜도 하나의 타입으로 사용된다(타입 사용이 모든 곳에 프로토콜 사용가능)
- 함수, 메소드, 이니셜라이저의 파라미터 타입 혹은 리턴 타입
- 상수, 변수, 프로퍼티의 타입
- 컨테이너의 배열, 사전 등의 아이템 타입

```
class Dice {
    let sides: Int
    let generator: RandomNumberGenerator
    init (sides: Int, generator: RandomNumberGenerator) {
        self.sides = sides
        self.generator = generator
    }
    func roll() -> Int {
        return Int(genertor.random() Double(sides)) + 1
    }
}
```
RandomNumberGenerator를 generator 상수의 타입으로 그리고 이니셜라이저의 파라미터 형으로 사용했다
위에서 선언한 Dice를 초기화 할 때 generator 파라미터 부분에 RandomNumberGeneratorr 프로토콜을 따르는 따르는 인스턴스를 넣는다
```
var d6 = Dice(sides: 6, generator: LinearCongruentialGenerator())
for _ in 1...5 {
    print("Random dice roll is \(d6.roll())")
}
// Random dice roll is 3
// Random dice roll is 5
// Random dice roll is 4
// Random dice roll is 5
// Random dice roll is 4
```

### 위임(Delegation)
위임은 클래스 혹은 구조체 인스턴스에 특정 행위에 대한 책임을 넘길 수 있게 해주는 디자인패턴중 하나

```
protocol DiceGame {
    var dice: Dice { get }
    func play()
}
protocol DiceGameDelegate: AnyObject {
    func gameDidStart(_ game: DiceGame)
    func game(_ game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int)
    func gameDidEnd(_ game: DiceGame)
}
```
DiceGame 프로토콜을 선언하고 DiceGameDelegate에 선언해서 실제 DiceGame의 행위에 관련된 구현을 DiceGameDelegate를 따르는 인스턴스에 위임한다
DiceGameDelegate를 AnyObject로 선언하면 클래스만 이 프로토콜을 따를 수 있게 만들 수 있다

```
class SnakesAndLadders: DiceGame {
    let finalSquare = 25
    let dice = Dice(sides: 6, generator: LinearCongruentialGenerator())
    var square = 0
    var board: [Int]
    ini() {
        board = Array(reqeating: 0, count: finalSquare + 1)
        board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02;
        board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08;
    }
    weak var delegate: DiceGameDelegate?
    func play() {
        square = 0
        delegate?.gameDiceStart(self)
        gameLoop: While square != finalSquare {
            let diceRoll = dice.roll()
            delegate?.game(self, didStartNewTurnWithDiceRoll: diceRoll)
            switch square + diceRoll {
                case finalSquare:
                    break gameLoop
                case let newSquare where newSquare > finalSquare:
                    continue gameLoop
                default:
                    square += diceRoll
                    square += board[square]
            }
        }
        delegate?.gameDidEnd(self)
    }
}
```
SnakesAndLadders는 DiceGame를 따르고
DiceGameDelegate를 따르는 델리게이트 delegate를 갖는다
게임을 실행 했을때
delegate?.gameDidStart(self), 
delegate?.game(self, didStartNewTurnWithDiceRoll: diceRoll),
delegate?.gameDidEnd(self)를 실행한다
delegate는 게임을 진행시키는데 반드시 필요한건 아니라 옵셔널로 정의됨
