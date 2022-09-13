# 자동 참조 카운트(Automatic Reference Counting)

- Swift에서는 앱의 메모리 사용을 관리하기 위해 ARC을 사용한다
- 자동으로 참조 횟수를 관리하기 때문에 대부분의 경우에 개발자는 메모리 관리에 신경쓸 필요가 없다(ARC가 알아서 사용하지 않는 인스턴스를 메모리에서 해지)
- 참조 횟수는 클래스 타입의 인스턴스에만 적용되고 값 타입인 구조체 열거형 등에는 적용되지 않음

### ARC의 동작(How ARC Works)
- 클래스의 새 인스턴스를 만들때 마다 ARC는 인스턴스 정보를 담는데 필요한 적정한 크기의 메모리를 할당한다
- 이 메모리는 그 인스턴스에 대한 정보와 관련된 저장 프로퍼티 값도 갖고 있다
- 인스턴스가 더 이상 사용되지 않을때 ARC는 그 인스턴스가 차지하고 있는 메모리를 해지해서 다른 용도로 사용할수 있도록 공간을 확보해 둔다
- 하지만 만약 ARC가 아직 사용중인 인스턴스를 메모리에서 내렸는데 인스턴스의 프로퍼티에 접근한다면 앱은 아마 크래시가 발생하게 된다
- ARC에서는 아직 사용중인 인스턴스를 해지하지 않기위해 많은 프로퍼티, 상수, 변수가 그 인스턴스에 대한 참조를 갖고 있는지 추적한다
- ARC는 최소 하나라도 그 인스턴스에 대한 참조가 있는 경우 그 인스턴스를 메모리에서 해지하지 않는다 

### ARC의 사용(ARC in Action)
- 하나의 클래스를 선언하고 클래스의 인스턴스가 생성될 때와 해지될때 print로 로그를 찍게 구현한 클래스
```
class Person {
    let name: String
    init(name: String) {
        self.name = name
        print("\(name) is being initialized")
    }
    deinit {
        print("\(name) is being deinitialized")
    }
}
```
위에 선언한 person 클래스 타입을 갖는 reference 변수 3개를 선언한다
이 변수는 모두 옵셔널 변수이다(초기값으로 모두 nil을 갖고 있다)
```
var referencel1: Person?
var referencel2: Person?
var referencel3: Person?
```
하나의 변수에 Person인스턴스를 생성에 참조하도록 한다

```
reference1 = Person(name: "John Appleseed")
// Prints "John Appleseed is being initialized"
```
나머지 두 변수를 첫 번째 변수를 참조하도록 한다

```
reference2 = reference1
reference3 = reference1
```
이 경우 reference2, reference3 모두 처음에 reference1이 참조하고 있는 같은 Person 인스턴스를 참조하게 된다
이 시점에 Person 인스턴스에 대한 참조 횟수는 3이 된다
그리고 나서 reference1, reference2 두 변수의 참조를 해지한다
그렇게 되면 Person 인스턴스는 해지 되지는 않는다
```
reference1 = nil
reference12 = nil
```
Person 인스턴스를 참조하고 있는 나머지 변수 reference3의 참조를 해지하면 더이상 Person 인스턴스르 참조하고 있는것이 없으므로 ARC가 Person 인스턴스를 메모리에서 해지하게 된다

```
reference3 = nil
// Prints "John Appleseed is being deinitialized"
```
위에 해지 로그가 찍히는 것으로 Person 인스턴스가 메모리에서 내려 갔음을 확인할 수 있다