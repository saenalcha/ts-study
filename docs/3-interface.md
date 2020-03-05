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



