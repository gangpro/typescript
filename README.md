# [TypeScript] README
> SKILL : `typescript`
> <br>
> TypeScript Docs : [TypeScript Docs](https://www.typescriptlang.org/docs/)
> <br>
> TypeScript Playground : [TypeScript Playground](https://www.typescriptlang.org/play)
> <br>
> YouTube Class: [코딩앙마](https://youtu.be/5oGAkQsGWkc)



## 목차
1. [기본 타입](#기본-타입)
2. [인터페이스](#인터페이스interface)
3. [함수](#함수function)
4. [리터럴 타입](#리터럴-타입literal-types)
5. [유니온 타입](#유니온-타입union-types)
6. [교차 타입](#교차-타입intersection-types)
7. [클래스 타입](#클래스class)
8. [제네릭](#제네릭generics)
9. [유틸리티 타입](#유틸리티-타입utility-types)


## 기본 타입
```typescript
/**
 * function
 */
function add(num1: number, num2: number) {
    console.log(1 + 2);
}
// add(1, 2)

function showItems(arr: number[]) {
    arr.forEach((item) => {
        console.log(item);
    })
}
// showItems([1, 2, 3]);

/**
 * TypeScript Type
 * boolean, string, number, string[], number[], Array<string>, Array<number>
 * void, never, enum, null, undefined
 */
let gender: boolean = true;
let car: string = 'bmw';
let age: number = 30;

let arr1: number[] = [1, 2, 3];
let arr2: Array<number> = [1, 2, 3];

let arr3: string[] = ['a', 'b', 'c'];
let arr4: Array<string> = ['a', 'b', 'c'];

let tuples: [string, number];
tuples = ['a', 1];
// tuples[0].toUpperCase(); // O
// tuples[1].toUpperCase(); // X

// void : 반환하지 않을 때
function sayHello(): void {
    console.log("Hello");
    //return 'Hi';
}
// sayHello();

// never : 끝나지 않을 때
function showError(): never {
    throw new Error();
}
//showError();

function inLoop(): never {
    while (true) {
        // do Something..
    }
}

// enum : 비슷한 값들끼리 묶어줌
enum OsNum {
    Window,
    Ios = 10,
    Android
}
// console.log(OsNum[10]);
// console.log(OsNum['Ios']);

enum OsString {
    Window = 'win',
    Ios = 'ios',
    Android = 'and'
}

let myOs: OsString; // 선언
myOs = OsString.Ios;
console.log(myOs);

// null, undefined
let isNull: null = null;
let isUndefined: undefined = undefined;

```

## 인터페이스(interface)
```typescript
/**
 * interface
 */
// user 객체 생성 // 객체는 object 타입으로 정의할 수 있음
let user: object;
user = {
    name: 'gangpro',
    age: 30
}
// console.log(user.name); // error
// console.log(user.age); // error

// 프로퍼티를 정의해서 객체로 표현할 때는 interface를 사용한다.
type Score = 'A' | 'B' | 'C' | 'F';

interface User1 {
    name: string;
    age: number;
    gender?: string; // ? : optional
    readonly birthYear: number; // readonly : 읽기전용
    // grade1? : string,
    // grade2? : string,
    // grade3? : string,
    // grade4? : string,
    // [grade: number]: string,
    [grade: number]: Score, // 문자열 리터럴 타입
}

let user1: User1 = {
    name: 'gangpro',
    age: 30,
    birthYear: 2000, // interface에 readonly로 정의했기 때문에 생성할 때만 할당 가능하고 이후 수정 불가능
    1: 'A',
    2: 'B',
    // 3: 'D', // error : Socre 타입에 D는 없기 때문에
}

// user1.age = 20;
// user1.gender = 'male'; // error : interface에 정의되어 있지 않기 때문에 // interface에 옵셔널하게 처리 가능
// user1.birthYear = 2002; // error : interface에 readonly로 정의했기 때문에 수정 불가능



/**
 * interface function
 */
interface Add {
    (num1: number, num2: number): number;
}
const add: Add = function (x, y) {
    return x + y;
}
// console.log(add(1, 2));

interface IsAdult {
    (age: number): boolean;
}
const sungin: IsAdult = (age) => {
    return age > 19;
}
// console.log(sungin(20));



/**
 * interface class : implement 키워드
 */
interface Car {
    color: string;
    wheels: number;
    start(): void;
}

class Bmw implements Car {
    color;
    constructor(c: string) {
        this.color = c;
    }
    wheels = 4;
    start() {
        console.log('go..');
    }
}

const carB = new Bmw('green');
console.log(carB);
carB.start();



/**
 * interface class : extends 키워드 : 한개의 경우
 */
interface Kia extends Car {
    door: number;
    stop(): void;
}

const carK: Kia = {
    color: 'red',
    wheels: 4,
    start() {
        console.log('start KIA');
    },
    door: 6,
    stop() {
        console.log('stop KIA');
    }
}



/**
 * interface class : extends 키워드 : 여러개의 경우
 */
interface Truck {
    color: string;
    wheels: number;
    start(): void;
}

interface Toy {
    name: string;
}

interface ToyTruck extends Truck, Toy {
    price: number;
}

```



## 함수(function)
```typescript
/**
 * function
 */
function add1(num1: number, num2: number): number {
    return num1 + num2;
}

function add2(num1: number, num2: number): void {
    console.log(num1 + num2);
}

function isAdult(age: number): boolean {
    return age > 19;
}

// 선택적 파라미터
function hello1(name?: string) {
    return `Hello ${name || 'world'}`;
}
console.log(hello1());

function hello2(name = 'HI') {
    return `Hello & ${name}`;
}
console.log(hello2());

// 선택적 파라미터는 앞에 쓸 수 없다. 
function hello3(age?: number, name: string): string {
// function hello3(age?: number, name: string): string {
    if(age !== undefined) {
        return `Hello ${name}. You are ${age}.`;
    }
    else {
        return `Hello ${name}`;
    }
}

// 선택적 파라미터는 앞에 쓸 수 없다. 굳이 쓰려면 아래 처럼..
function hello4(age: number | undefined, name: string): string {
// function hello3(age?: number, name: string): string {
    if(age !== undefined) {
        return `Hello ${name}. You are ${age}.`;
    }
    else {
        return `Hello ${name}`;
    }
}

// 
function addAll(...nums: number[]) {
    return nums.reduce((result, num) => result + num, 0)
}
console.log(addAll(1, 2, 3));
console.log(addAll(1,2,3,4,5,6,7,8,9,10));

/**
 * this
 */
interface User {
    name: string
}

const Kang: User = { name: 'KANG' };

function showName(this: User) {
    console.log(this.name);
}

const showMe = showName.bind(Kang);
showMe(); // KANG


function showName2(this: User, age: number, gender: 'M' | 'F') {
    console.log(this.name, age, gender);
}

const showMe2 = showName2.bind(Kang);
showMe2(30, 'M'); // KANG, 30, 'M'

/**
 * overloading
 */
interface User2 {
    name: string,
    age: number
}

function join(name: string, age: string): string; // age 가 string 일 때 string 반환
function join(name: string, age: number): User2; // age 가 number 일 때 User2 반환
function join(name: string, age: number | string): User2 | string {
    if(typeof age === 'number') {
        return {
            name,
            age
        }
    }
    else {
        return `나이는 숫자로 입력해주세요.`;
    }
}
const kang: User2 = join('KANG', 30);
const sam: string = join('sam', '30');
```



## 리터럴 타입(literal types)
```typescript
/**
 * literal Types
 */
const userName1 = 'Bob'; // const userName1: 'Bob'
let userName2 = 'Tom'; // let userName2: string

type Job = 'developer' | 'designer' | 'teacher';

interface User {
    name: string;
    job: Job;
}

const user: User = {
    name: 'KANG',
    // job: 'student' // error
    job: 'developer'
}
```



## 유니온 타입(union types)
or 의미
```typescript
/**
 * union Types
 */
interface Car {
    name: 'car';
    color: string;
    start(): void;
}

interface Mobile {
    name: 'mobile';
    color: string;
    call(): void;
}

function getGift(gift: Car | Mobile) {
    console.log(gift.color);
    // gift.start(); // error
    if(gift.name === 'car') {
        gift.start();
    }
    else if(gift.name === 'mobile') {
        gift.call();
    }
}
```



## 교차 타입(intersection types)
and 의미
```typescript
/**
 * intersection Types
 */
interface Car {
    name: string;
    start(): void;
}

interface Toy {
    name: string;
    color: string;
    price: number;
}

const toyCar: Toy & Car = {
    name: '타요',
    start() {},
    color: 'red',
    price: 1000
}
```



## 클래스(class)
```typescript
/**
 * class
 */
class Car {
    color: string; // 멤버 변수(member variable) : 메소드 밖에서 선언된 변수 // 메소드 안에서 선언된 변수는 지역 변수(local variable)
    constructor(color: string) {
        this.color = color;
    }
    start() {
        console.log('start');
    }
}

const bmw = new Car('red');
```

```typescript
/**
 * class
 */
class Car {
    // public
    constructor(public color: string) {
        this.color = color;
    }
    start() {
        console.log('start');
    }
}

const bmw = new Car('red');
```

### 접근 제한자(Access modifiers) : public
자식 클래스, 클래스 인스턴스 모두 접근 가능
```typescript
class Car {
    public name: string = 'Car'; //= name: string = 'Car';
    color: string;
    constructor(color: string) {
        this.color = color;
    }
    start() {
        console.log('start >>>');
    }
}

class Bmw extends Car {
    constructor(color: string) {
        super(color);
    }
    showName() {
        console.log(super.name);
    }
}

const whosCar = new Bmw('block');
// public으로 선언한 name을 클래스 인스턴스로 접근 가능
console.log(whosCar.name); // ok
```

### 접근 제한자(Access modifiers) : private 1
해당 클래스 내부에서만 접근 가능
```typescript
class Car {
    // name이 private이기 때문에 Car 클래스 내부에서만 사용가능하므로 Bmw 클래스 > showName()에서 error 
    //= #name: string = 'Car';
    private name: string = 'Car';
    color: string;
    constructor(color: string) {
        this.color = color;
    }
    start() {
        console.log('start >>>');
        console.log(this.name);
        // console.log(this.#name); // Car 클래스에서 #name: string = 'Car';를 사용한 경우에 this.#name으로 써야한다.
    }
}

class Bmw extends Car {
    constructor(color: string) {
        super(color);
    }
    showName() {
        console.log(super.name); // error
    }
}
```
### 접근 제한자(Access modifiers) : private 2
해당 클래스 내부에서만 접근 가능
```typescript
class Car {
    // name이 private이기 때문에 Car 클래스 내부에서만 사용가능하므로 Bmw 클래스 > showName()에서 error 
    #name: string = 'Car';
    color: string;
    constructor(color: string) {
        this.color = color;
    }
    start() {
        console.log('start >>>');
        console.log(this.#name); // Car 클래스에서 #name: string = 'Car';를 사용한 경우에 this.#name으로 써야한다.
    }
}

class Bmw extends Car {
    constructor(color: string) {
        super(color);
    }
    showName() {
        console.log(super.name); // error
    }
}
```

### 접근 제한자(Access modifiers) : protected
자식 클래스에서 접근 가능
```typescript
class Car {
    protected name: string = 'Car';
    color: string;
    constructor(color: string) {
        this.color = color;
    }
    start() {
        console.log('start >>>');
        console.log(this.name); // 자식 클래스에서 참조 가능
    }
}

class Bmw extends Car {
    constructor(color: string) {
        super(color);
    }
    showName() {
        console.log(super.name); // public처럼 protected도 자식 클래스에서 접근 가능
    }
}

const whosCar = new Bmw('block');
// protected로 선언한 name은 자식 클래스에서 참조할 수 있으나
// class 인스턴스로 참조할 수 없음
console.log(whosCar.name); // error 
```

### public & readonly 1
```typescript
class Car {
    public name: string = 'Car';
    color: string;
    constructor(color: string) {
        this.color = color;
    }
    start() {
        console.log('start >>>');
        console.log(this.name);
    }
}

class Bmw extends Car {
    constructor(color: string) {
        super(color);
    }
    showName() {
        console.log(super.name);
    }
}

const whosCar = new Bmw('block');
console.log(whosCar.name); // Car
whosCar.name = 'Truck';
console.log(whosCar.name); // Truck
```

### public & readonly 2
```typescript
class Car {
    readonly name: string = 'Car';
    color: string;
    constructor(color: string) {
        this.color = color;
    }
    start() {
        console.log('start >>>');
        console.log(this.name);
    }
}

class Bmw extends Car {
    constructor(color: string) {
        super(color);
    }
    showName() {
        console.log(super.name);
    }
}

const whosCar = new Bmw('block');
console.log(whosCar.name); // Car
whosCar.name = 'Truck'; // error
```
### public & readonly 3
```typescript
class Car {
    readonly name: string = 'Car';
    color: string;
    constructor(color: string, name: string) {
        this.color = color;
        this.name = name;
    }
    start() {
        console.log('start >>>');
        console.log(this.name);
    }
}

class Bmw extends Car {
    constructor(color: string, name: string) {
        super(color, name);
    }
    showName() {
        console.log(super.name);
    }
}

const whosCar = new Bmw('block', 'Truck');
console.log(whosCar); // Car
```

### static
static을 쓰면 정적 멤버 변수를 만들 수 있다.
```typescript
class Car {
    readonly name: string = 'Car';
    color: string;
    static wheels = 4;
    constructor(color: string, name: string) {
        this.color = color;
        this.name = name;
    }
    start() {
        console.log('start >>>');
        console.log(this.name);
        //console.log(this.wheels); // error : static으로 선언된 멤버 변수나, 정적 멤버 변수는 this가 아니라 클래스 명으로 사용해야 한다.
        console.log(Car.wheels); // ok
    }
}

class Bmw extends Car {
    constructor(color: string, name: string) {
        super(color, name);
    }
    showName() {
        console.log(super.name);
    }
}

console.log(Car.wheels);
```

### 추상 class 1
```typescript
abstract class Car {
    color: string;
    constructor(color: string) {
        this.color = color;
    }
    start() {
        console.log('start >>>');
    }
}

const car = new Car('red'); // error : 추상 class는 new로 객체를 만들 수 없다. 아래처럼 상속을 통해서만 사용 가능.

class Bmw extends Car {
    constructor(color: string) {
        super(color);
    }
}
```

### 추상 class 2
```typescript
abstract class Car {
    color: string;
    constructor(color: string) {
        this.color = color;
    }
    start() {
        console.log('start >>>');
    }
    abstract doSomething(): void;
}

// error : Car 추상 class 내부에 추상 method는 반드시 상속받은 곳에서 반드시 구현해줘야함.
// 그래서 구체적인 기능은 상속받은 곳에서 따로 진행
class Bmw extends Car {
    constructor(color: string) {
        super(color);
    }
    doSomething() {
        alert('OK');
    }
}
```

## 제네릭(generics)
```typescript
// 제네릭을 이용하면 class, function, interface를
// 다양한 type으로 재사용 할 수 있다.
// 선언 할 때는 타입 파라미터만 적어주고 생성하는 시기에 타입을 정해준다.
function getSize(arr: number[] | string[] | boolean[]): number {
    return arr.length;
}

const arr1 = [1, 2, 3];
console.log(getSize(arr1)); // 3

const arr2 = ['a', 'b', 'c'];
console.log(getSize(arr2));

const arr3 = [true, true, false];
console.log(getSize(arr3));
```

### function에서 generics
```typescript
// 타입이 증가할 때마다 getSize에 union 타입을 계속 적는 것 보다는 generic을 사용하는게 좋음.
// <T> 타입 파라미터라고 한다. // X, A, ... 어떤걸 써도 상관은 없음.
function getSize<T>(arr: T[]): number {
    return arr.length;
}

const arr1 = [1, 2, 3];
getSize<number>(arr1); //= function getSize<number>(arr: number[]): number { ... }

const arr2 = ['a', 'b', 'c'];
getSize<string>(arr2); //= function getSize<string>(arr: string[]): number { ... }

const arr3 = [true, true, false];
getSize(arr3); //= function getSize<boolean>(arr: boolean[]): number { ... }

const arr4 = [1, 2, 3];
getSize<number | string>(arr4); //= function getSize<number | string>(arr: (number | string)[]): number { ... }
```

### interface 에서 generics 1
```typescript
 // interface generic ex
 interface Mobile<T> {
     name: string;
     price: number;
    //  option: any;
     option: T;
 }

// object 생성
//= const m1: Mobile<object> = {
const m1: Mobile<{color: string, coupon: boolean}> = {
    name: 'iPhone13',
    price: 100,
    option: {
        color: 'black',
        coupon: false
    }
}

const m2: Mobile<string> = {
    name: 'iPhone14',
    price: 130,
    option: 'new one'
}
```

### interface 에서 generics 2
```typescript
interface User {
    name: string;
    age: number;
}

interface Car {
    name: string;
    color: string;
}

interface Book {
    price: number;
}

const user: User = { name: 'KANG', age: 30 };
const car: Car = { name: 'volvo', color: 'black' };
const book: Book = { price: 1000 };


function showName<T extends { name: string }>(data: T): string {
    return data.name;
}

showName(user);
showName(car);
showName(book); // error : Book interface 안에 name 없기 때문에
```



## 유틸리티 타입(utility types)
### keyof
```typescript
interface User {
    id: number;
    name: string;
    age: number;
    gender: 'M' | 'F';
}

// keyof 키워드를 사용하면 
// User interface에서 키 값들을 union 형태로 받을 수 있다.
type UserKey = keyof User; //= 'id' | 'name' | 'age' | 'gender';

const userkey1: UserKey = ''; // error
const userkey2: UserKey = 'id'; // ok
const userkey3: UserKey = 'mobile'; // error
```

### Partial``<T>``
```typescript
// Partial<T>
// 객체 속성(property)를 모두 optional로 바꿔 줌
// 그래서 일부만 사용 가능함.
interface User {
    id: number;
    name: string;
    age: number;
    gender: 'M' | 'F';
}

// error
let admin1: User = {
    id: 1,
    name: 'KANG'
}

// ok
let admin2: Partial<User> = {
    id: 1,
    name: 'KANG',
    // job: 'developer' // error : User에 없는 property를 사용하면 에러
}

//= Partial<User> 
// interface User {
//     id?: number;
//     name?: string;
//     age?: number;
//     gender?: 'M' | 'F';
// }
```

### Required``<T>``
```typescript
// Partial<T>의 반대 Required<T>
// Required<T>
// 객체 속성(property)를 모두 필수로 바꿔 줌
interface User {
    id: number;
    name: string;
    age?: number;
}

// error : age? 가 필수 property가 되어 버림
let admin1: Required<User> = {
    id: 1,
    name: 'KANG'
}

// ok
let admin2: Required<User> = {
    id: 1,
    name: 'KANG',
    age: 30
}
```
### Readonly``<T>``
```typescript
// Readonly<T>
// 읽기 전용으로 바꿔 줌 
interface User {
    id: number;
    name: string;
    age?: number;
}

let admin1: User = {
    id: 1,
    name: 'KANG'
}

admin1.id = 0; // ok
```

```typescript
interface User {
    id: number;
    name: string;
    age?: number;
}

let admin1: Readonly<User> = {
    id: 1,
    name: 'KANG'
}

admin1.id = 0; // error : 처음에 할당만 가능하고 수정은 불가능하게 됨.
```
### Record``<K, T>`` 1
```typescript
// Record<K, T>
// K : keys
// T : type
interface Score {
    '1': 'A' | 'B' | 'C' | 'D';
    '2': 'A' | 'B' | 'C' | 'D';
    '3': 'A' | 'B' | 'C' | 'D';
    '4': 'A' | 'B' | 'C' | 'D';
}

const score: Score = {
    1: 'A',
    2: 'B',
    3: 'C',
    4: 'D'
}
```

```typescript
const score: Record<'1' | '2' | '3' | '4', 'A' | 'B' | 'C' | 'D'> = {
    1: 'A',
    2: 'B',
    3: 'C',
    4: 'D'
}
```

```typescript
type Grade = '1' | '2' | '3' | '4';
type Score = 'A' | 'B' | 'C' | 'D';
const score: Record<Grade, Score> = {
    1: 'A',
    2: 'B',
    3: 'C',
    4: 'D'
}
```

### Record``<K, T>`` 2
```typescript
// Record<K, T>
// K : keys
// T : type
interface User {
    id: number;
    name: string;
    age: number;
}

function isValid(user: User) {
    const result: Record<keyof User, boolean> = {
        id: user.id > 0,
        name: user.name !== '',
        age: user.age > 0
    };
    return result;
}
```
### Pick``<T, K>``
```typescript
// Pick<T, K>
// T : type
// K : keys
interface User {
    id: number;
    name: string;
    age: number;
    gender: 'M' | 'F';
}

const admin: Pick<User, 'id' | 'name'> = {
    id: 0,
    name: 'KANG'
}
```
### Omit``<T, K>``
```typescript
// Pick<T, K>의 반대 Omit<T, K>
// Omit<T, K>
// T : type
// K : keys
interface User {
    id: number;
    name: string;
    age: number;
    gender: 'M' | 'F';
}

const admin: Omit<User, 'age' | 'gender'> = {
    id: 0,
    name: 'KANG'
}
```
### Exclude``<T1, T2>``
```typescript
// Omit<T, K>와 비슷한 Exclude<T1, T2>
// Exclude<T1, T2>
// T : type
// T1에서 T2를 제외하고 사용하는 방법
// Omit과 다른점은 Omit은 properties를 제거하는 방법이고, Exclude는 type으로 제거하는 방법
type T1 = string | number | boolean;
type T2 = Exclude<T1, number>; //= T2 = string
type T3 = Exclude<T1, number | string>; //= T3 = boolean
```
### NonNullable``<Type>``
```typescript
// NonNullable<Type>
// null, undefiend를 제외한 타입을 생성
type T1 = string | null | undefined | void;
type T2 = NonNullable<T1>; //= type T2 = string | void
```