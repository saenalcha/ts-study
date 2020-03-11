# interface

https://www.typescriptlang.org/play/


인터페이스는 객체의 속성과 메서드를 정의해 사용자 타입을 만드는 방법이다.\
객체가 인터페이스에 맞춰져 있으면 인터페이스를 구현했다고 한다.\
인터페이스는 `interface` 키워드로 정의한다. 

```
interface IComplexType {
    id: number;
    name: string;
}
```

```
let complexType : IComplexType
complexType = { id: 1, name: "test" }
```

모든 속성을 넣지 않은 채 객체를 만들면 에러가 난다.
```
let complexType : IComplexType
complexType = { id: 1 }
```
이렇게
```
Property 'name' is missing in type '{ id: number; }' but required in type 'IComplexType'.(2741)
input.ts(3, 5): 'name' is declared here.
```


## 선택적 속성

```
interface IOptionalProp {
    id: number
    name?: string
}
```
name 속성 뒤에 ? 기호로 선택적 속성을 표시했다. 
```
let idOnly : IOptionalProp = { id: 1 }
let idAndName : IOptionalProp = { id: 2, name : "idAndName" }

idAndName = idOnly
```
마지막 줄을 보면 두 변수 모두 IOptionalProp 인터페이스를 구현하고 있으면 서로 간의 할당이 가능하다. 


## 인터페이스 컴파일
인터페이스는 타입스크립트의 컴파일 시점 언어 기능이다.\
타입스크립트 컴파일러는 프로젝트에 포함된 인터페이스로 js 코드를 생성하지 않는다.\
인터페이스는 컴파일 단계에서 타입 확인을 위해 컴파일러에서만 사용한다. 

** 인터페이스 앞에 두문자 I 를 사용한다 **


# 클래스
클래스는 데이터와 동작을 포함하는 객체를 정의한다. 

```
class SimpleClass {
    id: number
    print() : void {
        console.log(`SimpleClass.print() called`)
    }
}
```
```
let mySimpleClass = new SimpleClass()
mySimpleClass.print()
```
결과:
```
SimpleClass.print() called
```

## 클래스 속성
```
class SimpleClass {
    id: number
    print() : void {
        console.log(`SimpleClass has id : ${this.id}`)
    }
}
```
```
let mySimpleClass = new SimpleClass()
mySimpleClass.id = 1001
mySimpleClass.print()
```

## 인터페이스 구현
클래스는 함수 본문을 구현해야하고, 인터페이스는 함수 정읠르 설명하기만 한다.

```
class ClassA {
    print() {
        console.log('ClassA.Print()')
    }
}

class ClassB {
    print() {
        console.log(`ClassB.print()`)
    }
}
```
더 쉽게 만들 수 있다.\
```
interface IPrint {
    print()
}

function printClass( a : IPrint ) {
    a.print()
}
```
어떤 변수는 printClass의 인자로 전달하려면 print라는 함수가 있어야 한다.
```
class ClassA implements IPrint {
    print() { console.log('ClassA.print()') }
}

class ClassB implements IPrint {
    print() { console.log('ClassB.print()') }
}
```
printClass 함수에서 2개의 클래스를 모두 사용할 수 있도록 클레스의 정의를 수정했다.
implements 키워드로 IPrint 인터페이스를 구현했다.
```
let classA = new ClassA()
let classB = new ClassB()

printClass(classA)
printClass(classB)
```
printClass 함수는 IPrint 인터페이스를 구현하는 모든 객체를 인자로 받게 돼 있어 두 클래스 타입에 대해 정상적으로 동작한다. \
인터페이스는 클래스의 동작을 설명하는 방법이다. \
**인터페이스는 클래스의 특정 동작에 대해 반드시 구현해야하는 요구 조건이 되기도 한다.**

## 클래스 생성자
```
class ClassWithConstructor {
    id: number
    name: string
    constructor(_id: number, _name: string) {
        this.id = _id
        this.name = _name
    }
}
```
```
let classWithConstructor = new ClassWithConstructor(2020, "saenal")
console.log(`classWithConstructor.id = ${classWithConstructor.id}`)
console.log(`classWithConstructor.name = ${classWithConstructor.name}`)
```

## 클래스 함수
```
class ComplexType implements IComplexType {
    id: number
    name: string
    constructor(idArg: number, nameArg: string)
    constructor(idArg: string, nameArg: string)
    constructor(idArg: any, nameArg: any) {
        this.id = idArg
        this.name = nameArg
    }
    print(): string {
        return "id:" + this.id + " name:" + this.name
    }
    usingTheAnyKeyword(arg1: any): any {
        this.id = arg1
    }
    usingOptionalParameters(optionalArg1: number) {
        if (optionalArg1) {
            this.id = optionalArg1
        }
    }
    usingDefaultParameters(defaultArg1: number = 0) {
        this.id = defaultArg1
    }
    usingRestSyntax(...argArray: number []) {
        if (argArray.length > 0) {
            this.id = argArray[0]
        }
    }
    usingFunctionCallbacks( callback: (id: number) => string ) {
        callback(this.id)
    }
}
```
```
let ct_1 = new ComplexType(1, "ct_1")
let ct_2 = new ComplexType('abc', "ct_2")
let ct_3 = new ComplexType(true, "test")
```
3번째 변수에서 컴파일 오류가 난다.
```
No overload matches this call.
  Overload 1 of 2, '(idArg: number, nameArg: string): ComplexType', gave the following error.
    Argument of type 'true' is not assignable to parameter of type 'number'.
  Overload 2 of 2, '(idArg: string, nameArg: string): ComplexType', gave the following error.
    Argument of type 'true' is not assignable to parameter of type 'string'.(2769)
```
함수 오버로드 규칙에 따라 마지막 시그니쳐가 any라도 그건 any가 아니다.\
```
let ct_2 = new ComplexType('abc', 'ct_2')
ct_2.print()

```
```
class ComplexType implements IComplexType {
    id: number
    name: string
    constructor(idArg: number, nameArg: string)
    constructor(idArg: string, nameArg: string)
    constructor(idArg: any, nameArg: any) {
        this.id = idArg
        // careful - 숫자 타입에 문자열을 할당함
        this.name = nameArg
    }
}
```
이런 경우 타입 가드를 사용해야한다. 
```
constructor(idArg: any, nameArg: any) {
    if (typeof idArg === 'number') {
        this.id = idArg
    }
    // 주의 - 문자열을 숫자 타입에 할당한다
    this.name = nameArg
}
```
아래는 모두 유효하다./
```
// 2개 다 유효
ct_1.usingTheAnyKeyword(true)
ct_1.usingTheAnyKeyword({ id: 1, name: 'string' })

// 2개 다 유효
ct_1.usingTheAnyKeyword(1)
ct_1.usingTheAnyKeyword()

// 2개 다 유효
ct_1.usingDefaultParameters(2)
ct_1.usingDefaultParameters()

// 2개 다 유효
ct_1.usingRestSyntax(1,2,3)
ct_1.usingRestSyntax(1,2,3,4,5)

function myCallbackFunction(id: number): string {
    return id.toString()
}
ct_1.usingFunctionCallbacks(myCallbacFunction)
```

## 인터페이스 함수 정의
인터페이스 정의에 새로 추가된 함수에 대한 정의를 추가해야한다.
```
interface IComplexType {
    id: number
    name: string
    print(): string
    usingTheAnyKeyword(arg1: any): any
    usingOptionalParameters(optionalArg1?: number)
    usingDefaultParameters(defaultArg1?: number)
    usingRestSyntax(...argArray: number [])
    usingFunctionalCallbacks(callback: (id: number) => string)
}
```
* 인터페이스 함수 정의는 클래스 함수 정의와 다르다
* 변수나 값을 가질 수 없다
* 클래스 함수와 유하사지만, 생성자 함수를 포함할 수 없다

```
interface IComplexType {
    constructor(arg1: any, arg2: any)
}
```
인터페이스에 생성자 함수를 포함시키면 컴파일 오류가 발생한다./

## 클래스 수정자
public으로 설정한 클래스 속성은 어디서나 접근할 수 있다.
```
class ClassWithPublicProperty {
    public id: number
}

let publicAccess = new ClassWithPublicProperty()
publicAccess.id = 10
```
```
class ClassWithPrivateProperty {
    private id: number
    constructor(_id: number) {
        this.id = _id
    }
}

let privateAccess = new ClassWithPrivateProperty(10)
privateAccess.id = 20

이렇게 오류가 난다
// Property 'id' is private and only accessible within class 'ClassWithPrivateProperty'.(2341)
```
**클래스 함수의 기본값은 public이다**

## 생성자 접근 제어자 Constructor Access Modifier
```
class classWithAutomaticProperties {
    constructor(public id: number, private name: string) {
    }
}

let myAutoClass = new classWithAutomaticProperties(1, 'className')
console.log(`myAutoClass id: ${myAutoClass.id}`)
console.log(`myAutoClass.name: ${myAutoClass.name}`)

// Property 'name' is private and only accessible within class 'classWithAutomaticProperties'.(2341)
// 컴파일하면 오류가 발생한다
// 이 축약 구문은 생성자 함수에만 사용할 수 있다.
```
**축약 기법으로 멤버 변수를 자동으로 만들 수 있지만, 코드 읽기가 어려워지므로, 사용하지 말자**

## 읽기 전용 속성 read-only property
한 번 설정하면 클래스 안에서도 수정할 수 없다
```
class ClassWithReadOnly {
    readonly name: string
    constructor(_name: string) {
        this.name = _name
    }
    setReadOnly(_name: string) {
        // 컴파일 에러 생성
        this.name = _name
    }
}

// Cannot assign to 'name' because it is a read-only property.(2540)
```

## 클래스 속성 접근자 Class property Accessors

```
class ClassWithAccessors {
    private _id : number
    get id() {
        console.log(`inside get id()`)
        return this._id
    }
    set id(value: number) {
        console.log(`inside set id()`)
        this._id = value
    }
}

let classWithAccessors = new ClassWithAccessors()
classWithAccessors.id = 2
console.log(`id property is set to ${classWithAccessors.id}`)
```
*ES5 에만 있는 기능이기 떄문에 IE8 같은 데서는 js 런타임 오류가 난다*

## 정적 함수
인스턴스를 만들지 않고 호출 가능한 클래스 함수
```
class StaticClass {
    static printTwo() {
        console.log(`2`)
    }
}

StaticClass.printTwo()
```

## 정적 속성
```
class StaticProperty {
    static count = 0
    updateCount() {
        StaticProperty.count ++
    }
}

let firstInstance = new StaticProperty()

console.log(`StaticProperty.count = ${StaticProperty.count}`)
firstInstance.updateCount()
console.log(`StaticProperty.count = ${StaticProperty.count}`)

let secondInstance = new StaticProperty()
secondInstance.updateCount()
console.log(`StaticProperty.count = ${StaticProperty.count}`)
// 클래스 인스턴스 사이에서 공유된다.
// 결과:
// StaticProperty.count = 0
// StaticProperty.count = 1
// StaticProperty.count = 2
```

## 네임스페이스
```
namespace FirstNameSpace {
    class NotExported {
    }
    export class NameSpaceClass {
        id: number
    }
}

let firstNameSpace = new FirstNameSpace.NameSpaceClass()
let notExported = new FirstNameSpace.NotExported()

// export 키워드를 사용하지 않으면 오류가 발생한다
// Property 'NotExported' does not exist on type 'typeof FirstNameSpace'.(2339)

namespace SecondNameSpace {
    export class NameSpaceClass {
        name: string
    }
}
// 다른 클래스로 보기 때문에 오류가 발생하지 않는다
let secondNameSpace = new SecondNameSpace.NameSpaceClass()
```
# 상속 inheritance
## 인터페이스 상속

```
interface IBase {
    id: number
}

interface IDerivedFromBase extends IBase {
    name: string
}

class InterfaceInheritanceClass implements IDerivedFromBase {
    id: number
    name: string
}
```

## 클래스 상속
```
class BaseClass implements IBase {
    id: number
}

class DerivedFromClass extends BaseClass implements IDerivedFromBase {
    name: string
}
타입스크립트는 다중 상속을 지원하지 않는다
```

```
인터페이스는 여러 개를 구현할 수 있다
interface IFirstInterface {
    id: number
}
interface ISecondInterface {
    name: string
}
class MultipleInterfaces implements
    IFirstInterface, ISecondInterface {
        id: number
        name: string
}
```

## super 키워드
타입스크립트에서는 super 키워드로 기반 클래스의 같은 이름을 가진 함수를 호출할 수 있다.
```
class BaseClassWithConstructor {
    private id: number
    constructor(_id: number) {
        this.id = _id
    }
}

class DerivedClassWithConstructor extends
    BaseClassWithConstructor {
        private name: string
        constructor(_id: number, _name: string) {
            super(_id)
            this.name = _name
        }
}
```

## 함수 오버로딩
super 키워드로 기반 클래스의 함수 호출이 가능하다
```
class BaseClassWithFunction {
    public id: number
    getProperties() : string {
        return `id: ${this.id}`
    }
}

class DerivedClassWithFunction extends
    BaseClassWithFunction {
        public name: string
        getProperties() : string {
            return `${super.getProperties()}`
            + ` , name: ${this.name}`
        }
    }
}

let derivedClassWithFunction = new DerivedClassWithFunction()
derivedClassWithFunction.id = 1
derivedClassWithFunction.name = "derivedName"
console.log(derivedClassWithFunction.getProperties())
// 결과:
// id: 1 , name: derivedName
```

## protected 클래스 멤버

```
class ClassUsingProtected {
    protected id : number
    public getId() {
        return this.id
    }
}

class DerivedFromProtected extends
    ClassUsingProtected {
        constructor() {
            super()
            this.id = 0
    }
}

let derivedFromProtected = new DerivedFromProtected()
derivedFromProtected.id = 1 // 여기서 컴파일 에러 발생
console.log(`getId returns: ${derivedFromProtected.getId()}`)

// 컴파일 에러 발생
// Property 'id' is protected and only accessible within class 'ClassUsingProtected' and its subclasses.(2445)
```

## 추상 클래스
인터페이스를 만들지 못함. 함수 구현이 가능.
```
class Employee {
    public id: number
    public name: string
    printDetails() {
        console.log(`id: ${this.id}`)
        + `, name ${this.name}` )
    }
}

class Manager {
    public id: number
    public name: string
    public Employees: Employee[]
    printDetails() {
        console.log(`id: ${this.id} `
        + `, name ${this.name}, `
        + ` employeeCount ${this.Employees.length}`)
    }
}

abstract class AbstractEmployee {
    public id: number
    public name: string
    abstract getDetails(): string
    public printDetails() {
        console.log(this.getDetails())
    }
}

class NewEmployee extends AbstractEmployee {
    getDetails(): string {
        return `id : ${this.id}, name : ${this.name}`
        }
    }
}

class NewManager extends NewEmployee {
    public Employees: NewEmployee[]
    getDetails(): string {
        return super.getDetails()
        + `, employeeCount ${this.Employees.length}`
    }
}
// 
let employee = new NewEmployee()
employee.id = 1
employee.name = 'Employee Name'

employee.printDetails()
// 결과: id : 1, name : Employee Name

//
let manager = new NewManager()
manager.id = 2
manager.name = 'Manager Name'
manager.Employees = new Array()

manager.printDetails()
// 결과: id : 2, name : Manager Name, employeeCount 0
```
추상 클래스와 상속을 사용하면 코드가 명확해지고 재사용성이 높아진다. /
추상화, 상속, 다형성, 캡슐화는 개체지향 디자인의 기본 개념들이다. /
살펴본 대로 타입 스크립트는 객체지향 디자인 원칙들을 통합해 깨끗하고 좋은 자바스크립트 코드를 작성하게 해준다. 

## 자바스크립트 클로저
```
function TestClosure(value) {
    this._value = value
    function printValue() {
        console.log(this._value)
    }
    return printValue
}

var myClosure = TestClosure(12)
myClosure

var BaseClassWithConstructor = (function () {
    function BaseClassWithConstructor(_id) {
        this.id = _id
    }
    return BaseClassWithConstructor
})()
```

# 인터페이스, 클래스 , 상속 - 팩토리 패턴
팩토리 클래스가 사용 가능한 여러 클래스 중 제공되는 정보에 알맞은 인터페이스를 반환하는 패턴

### IPerson 인터페이스
```
// 원서에 있는데 한글판에 빠져있음 🤷‍♂️😾😈👺🤡🤬😨
enum PersonCategory {
    Infant,
    Child,
    Adult
}

interface IPerson {
    Category: PersonCategory
    canSignContracts(): boolean
    printDetails()
}

abstract class Person implements IPerson {
    Category: PersonCategory
    private DateOfBirth: Date
    constructor(dateOfBirth: Date) {
        this.DateOfBirth = dateOfBirth
    }
    abstract canSignContracts(): boolean
    printDetails() : void {
        console.log(`Person : `)
        console.log(`Date of Birth : `
        + `${this.DateOfBirth.toDateString()}`)
        console.log(`Category : `
        + `${PersonCategory[this.Category]}`)
        console.log(`Can sign : `
        + `${this.canSignContracts()}`)
    }
}
```

### 특별 클래스
```
class Infant extends Person {
    constructor(dateOfBirth: Date) {
        super(dateOfBirth)
        this.Category = PersonCategory.Infant
    }
    canSignContracts(): boolean { return false }
}

class Child extends Person {
    constructor(dateOfBirth: Date) {
        super(dateOfBirth)
        this.Category = PersonCategory.Child
    }
    canSignContracts(): boolean { return false }
}

class Adult extends Person {
    constructor(dateOfBirth: Date) {
        super(dateOfBirth)
        this.Category = PersonCategory.Adult
    }
    canSignContracts(): boolean { return true }
}
```

### 팩토리 클래스
```
class PersonFactory {
    getPerson(dateOfBirth: Date): IPerson {
        let dateNow = new Date()
        let currentMonth = dateNow.getMonth() + 1
        let currentDate = dateNow.getDate()

        let dateTwoYearsAgo = new Date(
            dateNow.getFullYear() - 2,
            currentMonth, currentDate)
        
        let date18YearsAgo = new Date(
            dateNow.getFullYear() - 18,
            currentMonth, currentDate)
        if (dateOfBirth >= dateTwoYearsAgo) {
            return new Infant(dateOfBirth)
        }
        if (dateOfBirth >= date18YearsAgo) {
            return new Child(dateOfBirth)
        }
        return new Adult(dateOfBirth)
    }
}
```

### 팩토리 클래스 사용
```
let factory = new PersonFactory()
let p1 = factory.getPerson(new Date(2015, 0, 20))
p1.printDetails()
let p2 = factory.getPerson(new Date(2000, 0, 20))
p2.printDetails()
let p3 = factory.getPerson(new Date(1969, 0, 20))
p3.printDetails()

결과:
Person : 
Date of Birth : Tue Jan 20 2015
Category : Child
Can sign : false
Person : 
Date of Birth : Thu Jan 20 2000
Category : Adult
Can sign : true
Person : 
Date of Birth : Mon Jan 20 1969
Category : Adult
Can sign : true
```
* Infant, Child, Adult 클래스는 분류와 계약서 사인 가능 여부만 신경쓴다.
* Person 추상 기반 클래스는 IPerson 인터페이스와 관련된 로직만 신경쓴다.
* PersonalFactory 클래스는 생년월일과 관계된 로직에만 신경 쓴다. 

객체 지향 디자인 패턴은 유지보수 가능하며 확장 가능한 좋은 코드를 작성하는데 도움이 된다.  
_good, extensible, and maintainable code_
