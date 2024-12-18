<h3 id="typescript에서-타입과-값의-구분">TypeScript에서 타입과 값의 구분</h3>
<ol>
<li><p><strong>타입 공간</strong></p>
<ul>
<li><p>타입 공간의 심벌은 타입을 정의하고 참조할 때 사용됩니다. 이 공간에서 심벌은 타입을 의미하며, <strong>컴파일 시점에 타입 검사를 위해 사용</strong></p>
<p>  <code>number</code>, <code>string</code>, <code>boolean</code>, <code>User</code> 등</p>
</li>
</ul>
</li>
<li><p><strong>값 공간</strong> </p>
<ul>
<li><p>값 공간의 심벌은 실제 런타임에 사용되는 값입니다. 이 공간의 심벌은 코드가 실행될 때 <strong>실제 메모리에서 사용</strong></p>
<p>   숫자 <code>5</code>, 문자열 <code>&quot;hello&quot;</code>, 객체 <code>{ name: &quot;Alice&quot; }</code> 등</p>
</li>
</ul>
</li>
</ol>
<p> <strong>타입과 값의 구분 코드</strong></p>
<pre><code class="language-tsx">// 타입 공간의 심벌
type User = {
  name: string;
  age: number;
};

// 값 공간의 심벌
const user: User = {
  name: &quot;youjin&quot;,
  age: 21,
};

// 타입 공간의 심벌 사용 예시
function greet(user: User): string {
  return `Hello, ${user.name}`;
}

// 값 공간의 심벌 사용 예시
console.log(greet(user)); // 출력: Hello, youjin
</code></pre>
<p><code>User</code>는 타입 공간에서 사용되며, 데이터의 구조를 정의한다</p>
<p><code>user</code>는 값 공간에서 사용 되는 데이터이다</p>
<p><strong>타입과 값의 구분 코드</strong></p>
<pre><code class="language-tsx">// 타입 공간의 심벌
type User = {
  name: string;
  age: number;
};

// 값 공간의 심벌
const user: User = {
  name: &quot;youjin&quot;,
  age: 21,
};

// 타입 공간의 심벌 사용 예시
function greet(user: User): string {
  return `Hello, ${user.name}`;
}

// 값 공간의 심벌 사용 예시
console.log(greet(user)); // 출력: Hello, youjin
</code></pre>
<p><code>User</code>는 타입 공간에서 사용되며, 데이터의 구조를 정의한다</p>
<p><code>user</code>는 값 공간에서 사용 되는 데이터이다</p>
<p><strong>타입과 값의 구분을 위한 규칙</strong></p>
<pre><code class="language-tsx">type T1 = 'string literal';
type T2 = 123;

const V1 = 'string literal';
const V2 = 123;</code></pre>
<p><code>T1</code>과 <code>T2</code>는 타입을 정의하고, </p>
<p><code>V1</code>과 <code>V2</code>는 실제 값을 저장하는 심벌입니다</p>
<p><strong>typeof 연산자의 타입 공간과 값 공간에서의 사용 차이</strong></p>
<pre><code class="language-tsx">type T1 = typeof V1; // 타입: 'string'
type T2 = typeof V2; // 타입: 'number'

const v1 = typeof V1; // 값: &quot;string&quot;
const v2 = typeof V2; // 값: &quot;number&quot;</code></pre>
<ul>
<li><code>type T1 = typeof V1;</code>는 타입 공간에서의 사용으로, V1의 타입을 참조합니다.</li>
<li><code>const v1 = typeof V1;</code>는 값 공간에서의 사용으로, V1의 실제 타입을 문자열로 반환합니다.</li>
</ul>
<h3 id="클래스의-타입과-값-구분">클래스의 타입과 값 구분</h3>
<ol>
<li><p><strong>타입 공간에서의 클래스</strong></p>
<ul>
<li><p><strong>타입으로서의 클래스</strong>는 해당 클래스의 인스턴스 타입을 정의합니다. 즉, 클래스를 타입처럼 사용하여 변수의 타입을 지정하거나, 제네릭 타입 제약조건으로 사용할 수 있습니다.</p>
<p>타입 선언, 함수 파라미터의 타입으로 사용.</p>
</li>
</ul>
</li>
<li><p><strong>값 공간에서의 클래스</strong></p>
<ul>
<li><p><strong>값으로서의 클래스</strong>는 실제로 런타임 시점에 존재하는 생성자 함수로, 객체를 생성하거나 <code>instanceof</code> 연산을 통해 인스턴스인지 검사할 때 사용됩니다.</p>
<p><code>new</code> 키워드를 사용해 객체 생성, <code>instanceof</code>로 타입 검사.</p>
</li>
</ul>
</li>
</ol>
<pre><code class="language-tsx">class Person {
  name: string;
  constructor(name: string) {
    this.name = name;
  }

  greet() {
    console.log(`Hello, my name is ${this.name}`);
  }
}

// 타입 공간에서 사용
let personType: Person; // 타입으로 사용: Person 클래스의 인스턴스를 기대함

// 값 공간에서 사용
const personValue = new Person(&quot;Name&quot;); // 값으로 사용: 객체 생성
personValue.greet(); // 출력: Hello, my name is Name
console.log(personValue instanceof Person); // true</code></pre>
<p><strong>타입 공간 (let personType: Person)</strong>: Person은 타입으로 사용되어 personType 변수는 Person 타입의 인스턴스만 가짐</p>
<p><strong>값 공간 (new Person(&quot;Name&quot;))</strong>: Person은 생성자로 사용되어 새로운 객체를 만든다</p>
<pre><code class="language-tsx">// 기본 클래스 Person 정의
class Person {
  name: string;
  constructor(name: string) {
    this.name = name;
  }

  greet() {
    console.log(`Hello, my name is ${this.name}`);
  }
}

// Person을 확장하는 Age 클래스
class Age extends Person {
  age: number;
  constructor(name: string, age: number) {
    super(name);
    this.age = age;
  }

  greetWithAge() {
    console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
  }
}

// 타입 공간에서의 사용 예시
let personType: Person; // Person 타입의 인스턴스를 기대
let ageType: Age; // Age 타입의 인스턴스를 기대

// personType은 Person의 인스턴스를 참조
personType = new Person(&quot;심유진&quot;);
personType.greet(); // 출력: Hello, my name is 심유진

// ageType은 Age의 인스턴스를 참조
ageType = new Age(&quot;심슨&quot;, 25);
ageType.greetWithAge(); // 출력: Hello, my name is 심슨 and I am 25 years old.

// personType에 Age 인스턴스를 할당하는 것도 가능 (Age는 Person을 확장하므로)
personType = new Age(&quot;영희&quot;, 30);
personType.greet(); // 출력: Hello, my name is 영희

// 합집합 타입 사용
type PersonOrAge = Person | Age;

let personOrAge: PersonOrAge;

// Person 타입 할당
personOrAge = new Person(&quot;Alice&quot;);
personOrAge.greet(); // 출력: Hello, my name is Alice

// Age 타입 할당
personOrAge = new Age(&quot;Bob&quot;, 40);
if (personOrAge instanceof Age) {
  personOrAge.greetWithAge(); // 출력: Hello, my name is Bob and I am 40 years old.
} else {
  personOrAge.greet(); // 해당 부분은 Age가 아닌 Person 인스턴스인 경우에만 실행
}

// 값 공간에서의 사용
const personValue = new Person(&quot;Tom&quot;); // 값으로 사용: 객체 생성
personValue.greet(); // 출력: Hello, my name is Tom
console.log(personValue instanceof Person); // true

const ageValue = new Age(&quot;Jenny&quot;, 28); // Age 객체 생성
ageValue.greetWithAge(); // 출력: Hello, my name is Jenny and I am 28 years old.
console.log(ageValue instanceof Age); // true
console.log(ageValue instanceof Person); // true (Age는 Person을 확장했기 때문에)
</code></pre>
<h3 id="두-공간타입-값에서-다른-의미를-가지는-코드-패턴">두 공간(타입, 값)에서 다른 의미를 가지는 코드 패턴</h3>
<h3 id="1-this-키워드">1. <strong>this 키워드</strong></h3>
<ul>
<li><strong>값으로 사용</strong>: 현재 객체를 참조</li>
<li><strong>타입으로 사용</strong>: 현재 클래스 타입을 참조하며, 다형성(polymorphic this)으로 사용된다</li>
</ul>
<pre><code class="language-jsx">class Animal {
  name: string;
  constructor(name: string) {
    this.name = name;
  }

  // 값으로 사용: 현재 객체를 참조
  setName(name: string): this {
    this.name = name; // this는 현재 객체를 참조
    return this;
  }
}

class Dog extends Animal {
  breed: string;

  constructor(name: string, breed: string) {
    super(name);
    this.breed = breed;
  }

  // 타입으로 사용: this 타입을 반환하여 메서드 체인 
  setBreed(breed: string): this {
    this.breed = breed;
    return this;
  }
}

const myDog = new Dog(&quot;Rex&quot;, &quot;Labrador&quot;)
  .setName(&quot;Buddy&quot;) // 메서드 체인
  .setBreed(&quot;Golden Retriever&quot;);

console.log(myDog); // Dog { name: 'Buddy', breed: 'Golden Retriever' }

//setName 메서드를 호출하면 Animal 타입이 아닌 Dog 타입의 인스턴스가 반환
//때문에 setName 메서드 호출 후에 setBreed 메서드를 호출 가능</code></pre>
<h3 id="2-와--연산자">2. <strong>&amp;와 | 연산자</strong></h3>
<ul>
<li><strong>값으로 사용 :</strong> 비트 연산자로 AND와 OR 연산을 수행한다</li>
<li><strong>타입으로 사용 :</strong> 타입 시스템에서 인터섹션(&amp;)과 유니온(|) 타입을 정의한다</li>
</ul>
<pre><code class="language-jsx">// 값으로 사용: 비트 연산
const bitwiseAnd = 5 &amp; 3; // 0101 &amp; 0011 = 0001 (1)
const bitwiseOr = 5 | 3;  // 0101 | 0011 = 0111 (7)
console.log(bitwiseAnd, bitwiseOr); // 출력: 1 7

// 타입으로 사용: 인터섹션(&amp;)과 유니온(|)
type Cat = { purrs: boolean };
type Dog = { barks: boolean };
type Pet = Cat &amp; Dog; // 인터섹션 타입: 두 타입의 모든 속성을 가짐

let myPet: Pet = { purrs: true, barks: true }; // Cat과 Dog의 속성을 모두 가짐

type AnimalType = Cat | Dog; // 유니온 타입: Cat 또는 Dog 중 하나의 속성을 가짐

let anotherPet: AnimalType = { purrs: false }; // Cat 타입으로 사용 가능</code></pre>
<h3 id="3-const">3. <strong>const</strong></h3>
<ul>
<li><strong>값으로 사용</strong>: 변수를 선언할 때 사용하며, 값은 변경되지 xx</li>
<li><strong>타입으로 사용 (as const)</strong>: 리터럴 값의 타입을 정확하게 고정시키거나 추론된 타입을 변경</li>
</ul>
<pre><code class="language-jsx">// 값으로 사용: 변경 불가능한 변수 선언
const MAX_COUNT = 100;
// MAX_COUNT = 200; // 오류: 상수는 재할당할 수 없음

// 타입으로 사용: 리터럴 값을 고정하여 추론
const pet = {
  type: &quot;cat&quot;,
  color: &quot;black&quot;,
} as const; // pet의 타입이 { type: &quot;cat&quot;; color: &quot;black&quot;; }으로 고정됨

function describePet(pet: { type: &quot;cat&quot; | &quot;dog&quot;; color: string }) {
  console.log(`This ${pet.type} is ${pet.color}.`);
}

describePet(pet); // &quot;This cat is black.&quot;</code></pre>
<h3 id="4-extends">4. <strong>extends</strong></h3>
<ul>
<li><strong>값으로 사용 (class A extends B)</strong>: 클래스 상속을 통해 B 클래스의 기능을 A 클래스에 확장</li>
<li><strong>타입으로 사용 (interface A extends B)</strong>: 인터페이스 상속을 통해 A 인터페이스가 B의 타입을 확장</li>
<li><strong>제네릭 타입 한정자 (Generic)</strong>: 제네릭 타입에서 <code>T</code>가 특정 타입을 확장하도록 제한</li>
</ul>
<pre><code class="language-jsx">// 값으로 사용: 클래스 상속
class Vehicle {
  speed: number = 0;
  drive() {
    console.log(`Driving at ${this.speed} km/h`);
  }
}

class Car extends Vehicle {
  setSpeed(speed: number) {
    this.speed = speed;
  }
}

const myCar = new Car();
myCar.setSpeed(60);
myCar.drive(); // 출력: Driving at 60 km/h

// 타입으로 사용: 인터페이스 상속
interface Animal {
  legs: number;
}

interface Bird extends Animal {
  canFly: boolean;
}

const sparrow: Bird = { legs: 2, canFly: true };

// 제네릭 타입 한정자로 사용
function logLength&lt;T extends { length: number }&gt;(item: T) {
  console.log(item.length);
}

logLength([1, 2, 3]); // 출력: 3
</code></pre>
<h3 id="5-in">5. <strong>in</strong></h3>
<ul>
<li><strong>값으로 사용 (for (key in object))</strong>: 객체의 속성을 순회할 때 사용한다</li>
<li><strong>타입으로 사용 (매핑된 타입)</strong>: 타입 시스템에서 키의 집합을 기반으로 새로운 타입을 정의할 때 사용한다</li>
</ul>
<pre><code class="language-tsx">// in 연산자의 값과 타입 공간에서의 사용

// 값 공간에서의 사용: 객체 순회
const obj = { a: 1, b: 2, c: 3 };
for (const key in obj) {
  console.log(key); // &quot;a&quot;, &quot;b&quot;, &quot;c&quot;
}

// 타입 공간에서의 사용: 매핑된 타입
type Keys = 'name' | 'age'; //'name' 또는 'age'의 문자열 리터럴 타입을 가짐
type User = { [K in Keys]: string }; // User 타입은 { name: string; age: string; }
const user: User = { name: &quot;Alice&quot;, age: &quot;30&quot; };
</code></pre>
<h3 id="퀴즈">퀴즈</h3>
<ol>
<li>다음 코드에서 타입 공간과 값 공간의 심벌을 구분해주세요 </li>
</ol>
<pre><code class="language-jsx">class Car {
  model: string;
  constructor(model: string) {
    this.model = model;
  }
}

type Vehicle = Car; //1

const myCar = new Car(&quot;Tesla&quot;); //2

console.log(myCar instanceof Car); //3</code></pre>
<ul>
<li><p>정답</p>
<p>  <strong>type Vehicle = Car;</strong>:</p>
<ul>
<li><p><strong>타입 공간</strong>: Vehicle은 type 키워드를 사용하여 Car 타입을 별칭으로 지정한 심벌입니다</p>
</li>
<li><p><em>const myCar = new Car(&quot;Tesla&quot;);*</em>:</p>
</li>
<li><p><strong>값 공간</strong>: myCar는 실제로 메모리에 할당되는 Car 클래스의 인스턴스입니다ㅁ</p>
</li>
<li><p><em>console.log(myCar instanceof Car);*</em></p>
</li>
<li><p><strong>값 공간</strong>: instanceof는 값 공간에서만 동작하는 연산자로, myCar가 Car의 인스턴스인지 검사합니다. 여기서 Car는 값 공간에서 생성자 함수로 사용됐습니다</p>
</li>
</ul>
</li>
</ul>
<ol start="2">
<li>instanceof가 타입을 검사하는지 값을 검사하는지 말해보세요 </li>
</ol>
<pre><code class="language-jsx">class Shape {
  sides: number;
  constructor(sides: number) {
    this.sides = sides;
  }
}

const square = new Shape(4);

console.log(square instanceof Shape); 
</code></pre>
<ul>
<li><p>정답</p>
<h3 id="값-공간에서의-검사"><strong>값 공간에서의 검사</strong>:</h3>
<p>  square instanceof Shape는 square가 Shape 클래스의 인스턴스인지 확인한다</p>
<p>  instanceof는 square 객체의 <strong>프로토타입 체인</strong>을 검사하여 Shape의 프로토타입이 포함되어 있는지 확인함</p>
</li>
</ul>