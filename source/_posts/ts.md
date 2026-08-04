---
title: ts
date: 2020-07-28 14:33:58
tags: ts
categories: 
    - 前端
    - ts
---

# ts类型

- number: 数字类型
  
````typescript
let n:number = 1
````
  
- string: 字符串类型
  
````typescript
let str:string = "str"
````
  
- array: 数组类型
  
````typescript
let arr1:string[] = ["arr1]
let arr2:Array<number> =  ["arr2]
let arr3:any[] = [1,"w"]
````

- tuple: 元组类型
  
````typescript
let tuple:[string,number,boolean]=["str",1,false]
//是数组类型的一种
````
  
- enum: 枚举类型

````typescript
enum Color { res,blue = 5,yellow}
let red:Color = Color.red,blue:Color = Color.blue,yellow:Color = Color.yellow
console.log(red,blue,yellow,Color.red) // 0,5,6,0
//默认打印索引值，有值时为对应值，后面无值时打印跟随前面的值+1
````

- any: 任意类型

````typescript
let any:any = 1
any = true
// 可用来处理确定不了值，或者dom等对象
````
  
- null,undefined

````typescript
let under:number | undefined
// 用来初始不指定值不会出错
let null:number | null
// 用来值后来变为空后不会出错
````

- void

````typescript
function run():void{
    console.log("nothing")
}
// 一般用来方法无返回值
````
- never

````typescript
let ne:never=(()=>{
    throw new Error('fail')
})()
// 表示其他类型，null,undefined是其的子类型，表示从不会出现的值
````
# ts函数
## 可选参数

````typescript
function run(name:String,age?:number){
    console.log((name,age))
}
````
## 默认参数

````typescript
function run(name:String,age:number=20){
    console.log((name,age))
}
````
## 剩余参数

```typescript
function run(sum:number,...result:number[]):number{
    for(let i:number of result)
        sum+=i
    return sum
}
run(1,2,3,4)
```

## 函数重载

> 函数重载，主要用来检查类型，找到第一个匹配的类型传入
> 
````typescript
function run(name:string):string;
function run(age:number):string;
function run(str:any):string{
    if(typeof str === 'number'){
        return `我的年龄是${str}`
    }else{
        return `我的姓名是${str}`
    }
}
console.log(run(123))
````
# ts类

## 类的定义
````typescript
class Person {
    name: string

    constructor(name:string="张三") {
        this.name=name
    }
    
    run():void{
        console.log(`${this.name}在跑步`)
    }
}
````
## 类的继承
> extends的方法
````typescript
class Person {
    name: string
    protected age: number | undefined
    private sex: string | undefined

    constructor(name: string = "张三", age?: number, sex?: string) {
        this.name = name
        this.age = age
        this.sex = sex
    }

    run(): void {
        console.log(`${this.name}在跑步`)
    }

    getName(): string {
        return this.name
    }

    setName(name: string): void {
        this.name = name
    }
}

let p = new Person("李四")
p.setName("王二")
p.run()

class newPerson extends Person {
    constructor(name?: string) {
        super(name);
    }

    run(): void {
        console.log(`${this.name}在跑步,子类`)
        // super.run() // 父类的方法
    }
}

let n = new newPerson()
n.run()
````

## 类的修饰符
// public 默认，类中，子类，类外部都可以访问
// protected 类中，子类可以访问，外部无法访问
// private 类型可以访问，子类，外部无法访问
// 其实都可以方案，不过在编译时会报错

````typescript
// 定义同上
p.name //外部访问方法
n.run()  //子类访问方法
````
## 类里的静态属性和静态方法
> 所有对象共享

````typescript
class Person1 {
    static type: string = "person"
    constructor() {
    }
    static getType():void {
        console.log(Person1.type)
    }
}

Person1.getType()
console.log(Person1.type, 111)
````
##  类里面的多态
>即方法的重复实现
````typescript
class Animal{
    type:string
    constructor(type:string) {
        this.type=type
    }
    eat(){
        console.log("不知道你需要吃什么")
    }
}
class Dog extends Animal{
    eat() {
        console.log("需要吃狗粮")
    }
}
class Cat extends Animal{
    eat() {
        console.log("需要吃猫粮")
    }
}
new Cat("张无忌").eat()
````
## 抽象类的抽象方法
> 抽象类不可以直接被实例化，抽象类的子类必须实现对应的抽象方法
````typescript
abstract class Animal1 {
    type: string

    constructor(type: string) {
        this.type = type
    }

    abstract eat(): void
}
````


# ts接口

## 属性接口

### 对json进行约束

````typescript
// 必须一致
interface person{
    name:string;
    age:number
}
// 可选属性，可传可不传
interface person{
    name:string;
    age:number;
    sex?:string; // 可选属性
}
````

### 函数类型接口
>对函数传入的类型和返回的数值进行约束
````typescript
interface encrypt {
    (name: string, age: number): string
}

let md5: encrypt = function (name: string, age: number): string {
    console.log(name + age)
    return name+age
}
md5("张三", 13)
````

### 可索引的接口

>个人觉得没什么用

````typescript
interface arr {
    [index: number]: string | number
}

interface obj {
    [index: string]: string
}
let arr1: arr = ["123", 456]
let obj1: obj = {name: "张三"}
console.log(arr1, obj1)
````
###  类 类型的接口
> 对类的约束
````typescript
interface animal{
    name:string,
    eat(str:string):void //函数必须实现，但是里面的参数可以不传
}
class dog implements animal{
    name:string
    constructor(name:string) {
    this.name=name
}
eat(){
    console.log(this.name,"在吃饭")
}
}
````
 ### 接口的扩展，接口的继承

````typescript
class Person {
    name:String
}
interface animal {
    name: string,

    eat(str: string): void
}

interface person1 extends animal {
    work(): void
}

class doctor extends Person implements person1 {

    constructor(name: string) {
        super(name);
    }

    eat(str: string) {

    }

    work() {

    }
}
````

## 只读属性

>一些对象属性只能在对象刚刚创建的时候修改其值。 你可以在属性名前用 readonly来指定只读属性:

    interface Point {
        readonly x: number;
        readonly y: number;
    }
    
> 你可以通过赋值一个对象字面量来构造一个Point。 赋值后， x和y再也不能被改变了。
   
    let p1: Point = { x: 10, y: 20 };
    p1.x = 5; // error!
    
>TypeScript具有ReadonlyArray<T>类型，它与Array<T>相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改：
    
    let a: number[] = [1, 2, 3, 4];let ro: ReadonlyArray<number> = a;
    ro[0] = 12; // error!
    ro.push(5); // error!
    ro.length = 100; // error!
    a = ro; // error!

>上面代码的最后一行，可以看到就算把整个ReadonlyArray赋值到一个普通数组也是不可以的。 但是你可以用类型断言重写：

    a = ro as number[];

### readonly vs const

> 最简单判断该用readonly还是const的方法是看要把它做为变量使用还是做为一个属性。 做为变量使用的话用const，若做为属性则使用readonly。

## 额外的属性检查

>我们在第一个例子里使用了接口，TypeScript让我们传入{ size: number; label: string; }到仅期望得到{ label: string; }的函数里。 我们已经学过了可选属性，并且知道他们在“option bags”模式里很有用。
>然而，天真地将这两者结合的话就会像在JavaScript里那样搬起石头砸自己的脚。 比如，拿 createSquare例子来说：

    interface SquareConfig {
        color?: string;
        width?: number;
    }
    function createSquare(config: SquareConfig): { color: string; area: number } {
        // ...
    }
    let mySquare = createSquare({ colour: "red", width: 100 });

>注意传入createSquare的参数拼写为colour而不是color。 在JavaScript里，这会默默地失败。
>你可能会争辩这个程序已经正确地类型化了，因为width属性是兼容的，不存在color属性，而且额外的colour属性是无意义的。
>然而，TypeScript会认为这段代码可能存在bug。 对象字面量会被特殊对待而且会经过 额外属性检查，当将它们赋值给变量或作为参数传递的时候。 如果一个对象字面量存在任何“目标类型”不包含的属性时，你会得到一个错误。
    
    // error: 'colour' not expected in type 'SquareConfig'let mySquare = createSquare({ colour: "red", width: 100 });

>绕开这些检查非常简单。 最简便的方法是使用类型断言：

    let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig);

>然而，最佳的方式是能够添加一个字符串索引签名，前提是你能够确定这个对象可能具有某些做为特殊用途使用的额外属性。 如果 SquareConfig带有上面定义的类型的color和width属性，并且还会带有任意数量的其它属性，那么我们可以这样定义它：

    interface SquareConfig {
        color?: string;
        width?: number;
        [propName: string]: any;
    }
>我们稍后会讲到索引签名，但在这我们要表示的是SquareConfig可以有任意数量的属性，并且只要它们不是color和width，那么就无所谓它们的类型是什么。
>还有最后一种跳过这些检查的方式，这可能会让你感到惊讶，它就是将这个对象赋值给一个另一个变量： 因为squareOptions不会经过额外属性检查，所以编译器不会报错。
    
    let squareOptions = { colour: "red", width: 100 };let mySquare = createSquare(squareOptions);

>要留意，在像上面一样的简单代码里，你可能不应该去绕开这些检查。 对于包含方法和内部状态的复杂对象字面量来讲，你可能需要使用这些技巧，但是大部额外属性检查错误是真正的bug。 就是说你遇到了额外类型检查出的错误，比如“option bags”，你应该去审查一下你的类型声明。 在这里，如果支持传入 color或colour属性到createSquare，你应该修改SquareConfig定义来体现出这一点。

## 函数类型

>接口能够描述JavaScript中对象拥有的各种各样的外形。 除了描述带有属性的普通对象外，接口也可以描述函数类型。
>为了使用接口表示函数类型，我们需要给接口定义一个调用签名。 它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。

    interface SearchFunc {
      (source: string, subString: string): boolean;
    }

>这样定义后，我们可以像使用其它接口一样使用这个函数类型的接口。 下例展示了如何创建一个函数类型的变量，并将一个同类型的函数赋值给这个变量。
    
    let mySearch: SearchFunc;
    mySearch = function(source: string, subString: string) {
      let result = source.search(subString);
      return result > -1;
    }
    
>对于函数类型的类型检查来说，函数的参数名不需要与接口里定义的名字相匹配。 比如，我们使用下面的代码重写上面的例子：

    let mySearch: SearchFunc;
    mySearch = function(src: string, sub: string): boolean {
      let result = src.search(sub);
      return result > -1;
    }
>函数的参数会逐个进行检查，要求对应位置上的参数类型是兼容的。 如果你不想指定类型，TypeScript的类型系统会推断出参数类型，因为函数直接赋值给了 SearchFunc类型变量。 函数的返回值类型是通过其返回值推断出来的（此例是 false和true）。 如果让这个函数返回数字或字符串，类型检查器会警告我们函数的返回值类型与SearchFunc接口中的定义不匹配。
    
    let mySearch: SearchFunc;
    mySearch = function(src, sub) {
        let result = src.search(sub);
        return result > -1;
    }
## 可索引的类型
>与使用接口描述函数类型差不多，我们也可以描述那些能够“通过索引得到”的类型，比如a[10]或ageMap["daniel"]。 可索引类型具有一个 索引签名，它描述了对象索引的类型，还有相应的索引返回值类型。 让我们看一个例子：


    interface StringArray {
      [index: number]: string;
    }
    let myArray: StringArray;
    myArray = ["Bob", "Fred"];
    let myStr: string = myArray[0];

>上面例子里，我们定义了StringArray接口，它具有索引签名。 这个索引签名表示了当用 number去索引StringArray时会得到string类型的返回值。
>TypeScript支持两种索引签名：字符串和数字。 可以同时使用两种类型的索引，但是数字索引的返回值必须是字符串索引返回值类型的子类型。 这是因为当使用 number来索引时，JavaScript会将它转换成string然后再去索引对象。 也就是说用 100（一个number）去索引等同于使用"100"（一个string）去索引，因此两者需要保持一致。

    class Animal {
        name: string;
    }class Dog extends Animal {
        breed: string;
    }
    // 错误：使用数值型的字符串索引，有时会得到完全不同的Animal!interface NotOkay {
        [x: number]: Animal;
        [x: string]: Dog;
    }

>字符串索引签名能够很好的描述dictionary模式，并且它们也会确保所有属性与其返回值类型相匹配。 因为字符串索引声明了 obj.property和obj["property"]两种形式都可以。 下面的例子里， name的类型与字符串索引类型不匹配，所以类型检查器给出一个错误提示：

    interface NumberDictionary {
      [index: string]: number;
      length: number;    // 可以，length是number类型
      name: string       // 错误，`name`的类型与索引类型返回值的类型不匹配
    }

>最后，你可以将索引签名设置为只读，这样就防止了给索引赋值：

    interface ReadonlyStringArray {
        readonly [index: number]: string;
    }let myArray: ReadonlyStringArray = ["Alice", "Bob"];
    myArray[2] = "Mallory"; // error!
>你不能设置myArray[2]，因为索引签名是只读的。

## 类类型
### 实现接口

>与C#或Java里接口的基本作用一样，TypeScript也能够用它来明确的强制一个类去符合某种契约。
   
     interface ClockInterface {
        currentTime: Date;
    }
    class Clock implements ClockInterface {
        currentTime: Date;
        constructor(h: number, m: number) { }
    }
>你也可以在接口中描述一个方法，在类里实现它，如同下面的setTime方法一样：

    interface ClockInterface {
        currentTime: Date;
        setTime(d: Date);
    }
    class Clock implements ClockInterface {
        currentTime: Date;
        setTime(d: Date) {
            this.currentTime = d;
        }
        constructor(h: number, m: number) { }
    }
>接口描述了类的公共部分，而不是公共和私有两部分。 它不会帮你检查类是否具有某些私有成员。
类静态部分与实例部分的区别
当你操作类和接口的时候，你要知道类是具有两个类型的：静态部分的类型和实例的类型。 你会注意到，当你用构造器签名去定义一个接口并试图定义一个类去实现这个接口时会得到一个错误：

    interface ClockConstructor {
        new (hour: number, minute: number);
    }
    class Clock implements ClockConstructor {
        currentTime: Date;
        constructor(h: number, m: number) { }
    }
>这里因为当一个类实现了一个接口时，只对其实例部分进行类型检查。 constructor存在于类的静态部分，所以不在检查的范围内。
因此，我们应该直接操作类的静态部分。 看下面的例子，我们定义了两个接口， ClockConstructor为构造函数所用和ClockInterface为实例方法所用。 为了方便我们定义一个构造函数 createClock，它用传入的类型创建实例。

    interface ClockConstructor {
        new (hour: number, minute: number): ClockInterface;
    }interface ClockInterface {
        tick();
    }
    function createClock(ctor: ClockConstructor, hour: number, minute: number): ClockInterface {
        return new ctor(hour, minute);
    }
    class DigitalClock implements ClockInterface {
        constructor(h: number, m: number) { }
        tick() {
            console.log("beep beep");
        }
    }class AnalogClock implements ClockInterface {
        constructor(h: number, m: number) { }
        tick() {
            console.log("tick tock");
        }
    }
    let digital = createClock(DigitalClock, 12, 17);let analog = createClock(AnalogClock, 7, 32);

>因为createClock的第一个参数是ClockConstructor类型，在createClock(AnalogClock, 7, 32)里，会检查AnalogClock是否符合构造函数签名。

## 继承接口

>和类一样，接口也可以相互继承。 这让我们能够从一个接口里复制成员到另一个接口里，可以更灵活地将接口分割到可重用的模块里。

    interface Shape {
        color: string;
    }
    interface Square extends Shape {
        sideLength: number;
    }
    let square = <Square>{};
    square.color = "blue";
    square.sideLength = 10;

>一个接口可以继承多个接口，创建出多个接口的合成接口。

    interface Shape {
        color: string;
    }
    interface PenStroke {
        penWidth: number;
    }
    interface Square extends Shape, PenStroke {
        sideLength: number;
    }
    let square = <Square>{};
    square.color = "blue";
    square.sideLength = 10;
    square.penWidth = 5.0;
## 混合类型

>先前我们提过，接口能够描述JavaScript里丰富的类型。 因为JavaScript其动态灵活的特点，有时你会希望一个对象可以同时具有上面提到的多种类型。
一个例子就是，一个对象可以同时做为函数和对象使用，并带有额外的属性。

    interface Counter {
        (start: number): string;
        interval: number;
        reset(): void;
    }
    function getCounter(): Counter {
        let counter = <Counter>function (start: number) { };
        counter.interval = 123;
        counter.reset = function () { };
        return counter;
    }
    let c = getCounter();
    c(10);
    c.reset();
    c.interval = 5.0;
>在使用JavaScript第三方库的时候，你可能需要像上面那样去完整地定义类型。

## 接口继承类

>当接口继承了一个类类型时，它会继承类的成员但不包括其实现。 就好像接口声明了所有类中存在的成员，但并没有提供具体实现一样。 接口同样会继承到类的private和protected成员。 这意味着当你创建了一个接口继承了一个拥有私有或受保护的成员的类时，这个接口类型只能被这个类或其子类所实现（implement）。
当你有一个庞大的继承结构时这很有用，但要指出的是你的代码只在子类拥有特定属性时起作用。 这个子类除了继承至基类外与基类没有任何关系。 例：

    class Control {
        private state: any;
    }
    interface SelectableControl extends Control {
        select(): void;
    }
    class Button extends Control implements SelectableControl {
        select() { }
    }
    class TextBox extends Control {
        select() { }
    }
    // 错误：“Image”类型缺少“state”属性。class Image implements SelectableControl {
        select() { }
    }
    class Location {
    
    }
>在上面的例子里，SelectableControl包含了Control的所有成员，包括私有成员state。 因为 state是私有成员，所以只能够是Control的子类们才能实现SelectableControl接口。 因为只有 Control的子类才能够拥有一个声明于Control的私有成员state，这对私有成员的兼容性是必需的。
在Control类内部，是允许通过SelectableControl的实例来访问私有成员state的。 实际上，SelectableControl接口和拥有select方法的Control类是一样的。 Button和TextBox类是SelectableControl的子类（因为它们都继承自Control并有select方法），但Image和Location类并不是这样的。

# 泛型
> 在一些函数的类型，不仅支持当前的类型，实现类型的扩展,实现了any实现不了的指定类型校验

## 泛型函数
````typescript
function return_self<T>(value: T): T {
    return value
}

return_self<number>(1111)
return_self<string>("1111")
````


## 类的泛型

````typescript
class min_class<T,R> {
    public list: T[] = []

    add(value: T): T {
        return value
    }
}

let min = new min_class<number,string>()

let list: (number | string)[] = [12, "12"]
````
## 泛型接口

### 函数接口泛型

````typescript
interface classT {
    <T>(str: T): T
}

let ab: classT = function <T>(str: T) {

}
ab<number>(123)

interface classTT<T> {
    (str: T): T
}

function get_any<T>(str: T): T {
    return str
}

let any: classTT<number> = get_any
any(123)
````
# 命名空间
> 反正变量名冲突，在外部使用需要使用export
> 可以理解为小模块

````typescript

namespace A {
    interface Animal {
        name: String,
        eat(): void
    }

    class Dog implements Animal {
        name: String

        constructor(name: string) {
            this.name = name
        }

        eat(): void {
            console.log(this.name, "在吃东西")
        }
    }

    export class black_dog extends Dog {
        sex: String

        constructor(sex: string, name: string) {
            super(name);
            this.sex = sex
        }
    }
}
let black_dog = new A.black_dog("gon", "小黑")

````

# 装饰器

> 是一种特殊类型的声明，它能够被附加的类声明，方法，属性或参数上，可以修改类的行为

>通俗的讲，装饰器就是一个方法，可以注入到类，方法，属性参数上来扩展，属性，方法，参数的功能

>常见的装饰器有：类装饰器，属性装饰器，方法装饰器，参数装饰器

>装饰器的写法：普通装饰器（无法传参），装饰器工厂（可传参）

>属性>方法>方法参数>类，同种类的先下后上，先后后前

##  类装饰器
> 类装饰器在类声明之前被声明，类装饰器应用于类构造函数，可以用来监视，修改或者替换类定义
### 普通装饰器
````typescript
function constructor_1(params: any) {
    console.log(params)
    params.prototype.eat=function (){
        console.log("我是类中的方法")
    }
    // return class extends params {  /*此方法可重载类*/
    //
    // }
}
@constructor_1
class constructor_animal{
    constructor() {
    }
}
let people_2:any=new constructor_animal()
people_2.eat()
````

### 装饰器工厂
````typescript
function constructor_2(params: string) {
    return function (target: any) {
    console.log(params, target)
    target.prototype.eat=function (){
        console.log("我是类中的方法")
    }
    }
}

@constructor_2("这是传入的参数")
class constructor_people {
    constructor() {
    }

}
let people_3:any=new constructor_people()
people_3.eat()
````


## 属性装饰器
>接受两个参数 
>>1：对于静态成员来说是类的构造函数，对于实例成员来说是累的成员对象
>>2:成员的名字
````typescript
function http_log(params:string){
    return function (target:any,attr:any){
        console.log(params,target,attr)
        target[attr]=params
    }
}
class http_client{
    @http_log("blog.dingyahui.top")
    public url:string | undefined
    @http_log("http")
    public static type:string | undefined
}
let new_url = new http_client()
console.log(new_url.url)
````


## 方法装饰器
> 它会被应用到方法的属性描述符上，可以用来监视，修改或者替换方法定义
> 参数：
>> 1. 对于静态成员来说是类的构造函数，对于实例成员来说是类的原型对象
>> 2. 成员的名字
>> 3. 成员的属性描述符
````typescript
function logMethod(params:string){
    return function (target:any,name:string,desc:any){
        console.log(params,target,name,desc)
        let method = desc.value
        desc.value=function (params:string){
        console.log("这方法什么都不是的修改版")
        method.call(this,params) //加上这句可以执行函数原来的方法
    }
    }
}
class httpMethod{
    @logMethod("这个是方法")
    log(params:string){
    console.log("这方法什么都不是",params)
    }
}
let new_method = new httpMethod()
new_method.log("方法")
````

## 方法参数装饰器
> 在运行时当做函数被调用，可以使用参数装饰器为类的原型增加一些元素数据
> 参数：
>> 1. 对于静态成员来说是类的构造函数，对于实例成员来说是类的原型对象
>> 2. 方法的名字
>> 3. 参数在参数列表的中的索引

````typescript
function logParams(params:string){
    return function (target:any,name:string,index:any){
        console.log(params,target,name,index,"------params")
    }
}
class httpParams{
    log(@logParams("这个是参数") params:string){
        console.log("这参数什么都不是",params)
    }
}
let new_Params = new httpParams()
new_Params.log("参数")
````


# Utility Types

## Partial<Type>

> 构造具有以下所有属性的类型：Type设置为可选。此实用程序将返回表示给定类型的所有子集的类型。

> 使对应的属性变为可选

```typescript
interface Todo {
  title: string;
  description: string;
}

function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
  return { ...todo, ...fieldsToUpdate };
}

const todo1 = {
  title: "organize desk",
  description: "clear clutter",
};

const todo2 = updateTodo(todo1, {
  description: "throw out trash",
});
```

## Required<Type>

>Type设置为必需。相反的Partial.

>使属性变为必须

```typescript
interface Props {
  a?: number;
  b?: string;
}

const obj: Props = { a: 5 };

const obj2: Required<Props> = { a: 5 };
Property 'b' is missing in type '{ a: number; }' but required in type 'Required<Props>'.

```

## Readonly<Type>
>构造具有以下所有属性的类型：Type设为readonly，这意味着不能重新分配构造类型的属性。

>使属性变为只读，类似于Object.freeze()

```typescript
interface Todo {
  title: string;
}

const todo: Readonly<Todo> = {
  title: "Delete inactive users",
};

todo.title = "Hello";
Cannot assign to 'title' because it is a read-only property.
```

## Record<Keys,Type>

>构造一个对象类型，其属性键为Keys其属性值为Type。此实用程序可用于将类型的属性映射到另一类型。

>限制对象的键值和属性

```typescript
interface CatInfo {
  age: number;
  breed: string;
}

type CatName = "miffy" | "boris" | "mordred";

const cats: Record<CatName, CatInfo> = {
  miffy: { age: 10, breed: "Persian" },
  boris: { age: 5, breed: "Maine Coon" },
  mordred: { age: 16, breed: "British Shorthair" },
};

cats.boris;
 
const cats: Record<CatName, CatInfo>
```

## Pick<Type, Keys>

>通过选择属性集来构造类型。Keys(字符串文字或字符串文字的联合)Type.

>相当于值使用对应的联合字符串的属性
```typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Pick<Todo, "title" | "completed">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};

todo;
 
const todo: TodoPreview
```

## Omit<Type, Keys>

>通过从Type然后移除Keys(字符串文字或字符串文字的联合)。

>派出对应字符串联合的键值，返回属性

```typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
  createdAt: number;
}

type TodoPreview = Omit<Todo, "description">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
  createdAt: 1615544252770,
};

todo;
 
const todo: TodoPreview

type TodoInfo = Omit<Todo, "completed" | "createdAt">;

const todoInfo: TodoInfo = {
  title: "Pick up kids",
  description: "Kindergarten closes at 5pm",
};

todoInfo;
   
const todoInfo: TodoInfo
```

## Exclude<Type, ExcludedUnion>

>通过排除Type所有可指派给ExcludedUnion.

>派出type中一些可能的属性,返回属性

```typescript
type T0 = Exclude<"a" | "b" | "c", "a">;
     
type T0 = "b" | "c"
type T1 = Exclude<"a" | "b" | "c", "a" | "b">;
     
type T1 = "c"
type T2 = Exclude<string | number | (() => void), Function>;
     
type T2 = string | number
```

## Extract<Type, Union>

>通过从Type所有可指派给Union.

>返回共同可能的类型，返回类型

```typescript
type T0 = Extract<"a" | "b" | "c", "a" | "f">;
     
type T0 = "a"
type T1 = Extract<string | number | (() => void), Function>;
     
type T1 = () => void
```

## NonNullable<Type>

>通过排除构造类型null和undefined从…Type.

>去除其中的null 和 underfined属性

```typescript
type T0 = NonNullable<string | number | undefined>;
     
type T0 = string | number
type T1 = NonNullable<string[] | null | undefined>;
     
type T1 = string[]
```

## Parameters<Type>

>从函数类型的参数中使用的类型构造元组类型。Type.

>通过函数的参数生成对应的元组类型

```typescript
declare function f1(arg: { a: number; b: string }): void;

type T0 = Parameters<() => string>;
     
type T0 = []
type T1 = Parameters<(s: string) => void>;
     
type T1 = [s: string]
type T2 = Parameters<<T>(arg: T) => T>;
     
type T2 = [arg: unknown]
type T3 = Parameters<typeof f1>;
     
type T3 = [arg: {
    a: number;
    b: string;
}]
type T4 = Parameters<any>;
     
type T4 = unknown[]
type T5 = Parameters<never>;
     
type T5 = never
type T6 = Parameters<string>;
Type 'string' does not satisfy the constraint '(...args: any) => any'.
     
type T6 = never
type T7 = Parameters<Function>;
Type 'Function' does not satisfy the constraint '(...args: any) => any'.
  Type 'Function' provides no match for the signature '(...args: any): any'.
     
type T7 = never
```

## ConstructorParameters<Type>

>从构造函数类型的类型构造元组或数组类型。它生成包含所有参数类型(或类型)的元组类型。never如果Type不是一种功能)。

>从构造函数的类型生成元组或数组类型

```typescript
type T0 = ConstructorParameters<ErrorConstructor>;
     
type T0 = [message?: string]
type T1 = ConstructorParameters<FunctionConstructor>;
     
type T1 = string[]
type T2 = ConstructorParameters<RegExpConstructor>;
     
type T2 = [pattern: string | RegExp, flags?: string]
type T3 = ConstructorParameters<any>;
     
type T3 = unknown[]

type T4 = ConstructorParameters<Function>;
Type 'Function' does not satisfy the constraint 'abstract new (...args: any) => any'.
  Type 'Function' provides no match for the signature 'new (...args: any): any'.
     
type T4 = never
```

## ReturnType<Type>

>构造由函数的返回类型组成的类型。Type.

>根据函数的返回类型生成类型

```typescript
declare function f1(): { a: number; b: string };

type T0 = ReturnType<() => string>;
     
type T0 = string
type T1 = ReturnType<(s: string) => void>;
     
type T1 = void
type T2 = ReturnType<<T>() => T>;
     
type T2 = unknown
type T3 = ReturnType<<T extends U, U extends number[]>() => T>;
     
type T3 = number[]
type T4 = ReturnType<typeof f1>;
     
type T4 = {
    a: number;
    b: string;
}
type T5 = ReturnType<any>;
     
type T5 = any
type T6 = ReturnType<never>;
     
type T6 = never
type T7 = ReturnType<string>;
Type 'string' does not satisfy the constraint '(...args: any) => any'.
     
type T7 = any
type T8 = ReturnType<Function>;
Type 'Function' does not satisfy the constraint '(...args: any) => any'.
  Type 'Function' provides no match for the signature '(...args: any): any'.
     
type T8 = any
```

## InstanceType<Type>
>构造函数的实例类型构成的类型。Type.

>根据对应的实力类型生成类型、

```typescript
class C {
  x = 0;
  y = 0;
}

type T0 = InstanceType<typeof C>;
     
type T0 = C
type T1 = InstanceType<any>;
     
type T1 = any
type T2 = InstanceType<never>;
     
type T2 = never
type T3 = InstanceType<string>;
Type 'string' does not satisfy the constraint 'abstract new (...args: any) => any'.
     
type T3 = any
type T4 = InstanceType<Function>;
Type 'Function' does not satisfy the constraint 'abstract new (...args: any) => any'.
  Type 'Function' provides no match for the signature 'new (...args: any): any'.
     
type T4 = any
```

## ThisParameterType<Type>

>类的类型。这函数类型的参数，或未知数如果函数类型没有this参数。

>根据对应函数的类型返回t对应参数的类型

```typescript
function toHex(this: Number) {
  return this.toString(16);
}
function numberToString(n: ThisParameterType<typeof toHex>) {
  return toHex.apply(n);
}
```

## OmitThisParameter<Type>
>移除this参数来自Type。如果Type没有显式声明this参数，则结果是Type。否则，使用no的新函数类型。this参数是从Type。泛型被删除，并且只有最后一个重载签名被传播到新的函数类型中。

>相当于一个更改执行的类型，如bind
```typescript
function toHex(this: Number) {
  return this.toString(16);
}

const fiveToHex: OmitThisParameter<typeof toHex> = toHex.bind(5);

console.log(fiveToHex());
```

## ThisType<Type>

>此实用程序不返回转换的类型。相反，它是上下文的标记。this类型。注意，--noImplicitThis必须启用标志才能使用此实用程序。

>声明了对应的this的包含的参数类型

```typescript
type ObjectDescriptor<D, M> = {
  data?: D;
  methods?: M & ThisType<D & M>; // Type of 'this' in methods is D & M
};

function makeObject<D, M>(desc: ObjectDescriptor<D, M>): D & M {
  let data: object = desc.data || {};
  let methods: object = desc.methods || {};
  return { ...data, ...methods } as D & M;
}

let obj = makeObject({
  data: { x: 0, y: 0 },
  methods: {
    moveBy(dx: number, dy: number) {
      this.x += dx; // Strongly typed this
      this.y += dy; // Strongly typed this
    },
  },
});

obj.x = 10;
obj.y = 20;
obj.moveBy(5, 5);
```
在上面的示例中，methods对象的参数中的makeObject具有上下文类型，其中包括ThisType<D & M>因此.的类型这方法中的methods对象是{ x: number, y: number } & { moveBy(dx: number, dy: number): number }。注意methods属性的推断目标和源。this键入方法。

这个ThisType<T>中声明的空接口。lib.d.ts。除了在对象文本的上下文类型中识别之外，接口的作用就像任何空接口。

## 内部字符串操作类型
```typescript
Uppercase<StringType>
Lowercase<StringType>
Capitalize<StringType>
Uncapitalize<StringType>
```