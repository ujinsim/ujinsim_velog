<h1 id="타입확장">타입확장</h1>
<blockquote>
<p>타입 확장은 기존 타입을 사용해서 새로운 타입을 정의하는 것을 말한다. 기본적으로 타입스크립트에서는<code>interface</code>, <code>type</code> 키워드를 사용해서 타입을 정의한다. 뿐만 아니라 타입을 받아오는 <code>제네릭</code>과 <code>클래스</code>에서도 타입을 확장하거나 제한할 수 있다</p>
</blockquote>
<h2 id="1-타입type을-확장하거나-제한하는-경우">1. 타입(Type)을 확장하거나 제한하는 경우</h2>
<blockquote>
<p>TypeScript의 type 키워드를 사용하여 정의된 고정된 타입입니다.</p>
</blockquote>
<p>확장 혹은 제한 방식 : <strong>유니온 타입(Union Type)</strong>, <strong>인터섹션 타입(Intersection Type)</strong>, <strong>타입 별칭(Type Alias)</strong> 등을 사용</p>
<h3 id="언제-타입을-사용해야-하나">언제 타입을 사용해야 하나?</h3>
<ul>
<li><strong>고정된 타입 조합</strong>을 만들고 싶을 때 (유니온, 인터섹션).</li>
<li><strong>단순히 속성을 추가하거나 제거</strong>하여 새로운 타입을 만들고 싶을 때.</li>
<li><strong>타입 유틸리티</strong> (<code>Partial</code>, <code>Pick</code>, <code>Omit</code>, <code>Exclude</code> 등)로 타입을 조작하고 싶을 때.</li>
</ul>
<pre><code class="language-tsx">type Animal = {
  name: string;
};

type Dog = Animal &amp; { breed: string }; // 인터섹션 타입으로 Animal을 확장
type Pet = Animal | Dog; // 유니온 타입으로 Animal과 Dog를 조합

// 타입 제한: 특정 속성 제거
type Person = {
  name: string;
  age: number;
  location: string;
};

type PersonWithoutLocation = Omit&lt;Person, 'location'&gt;; // location 속성 제거</code></pre>
<h2 id="2-인터페이스interface를-확장하거나-제한하는-경우">2. 인터페이스(Interface)를 확장하거나 제한하는 경우</h2>
<blockquote>
<p>인터페이스는 TypeScript의 컨텍스트 내에서만 존재하는 가상 구조입니다. TypeScript 컴파일러는 <strong>타입 체크 목적으로만 인터페이스를 사용합니다.</strong></p>
</blockquote>
<ul>
<li><strong>인터페이스의 확장방식</strong> : 상속 , 선언적 확장</li>
</ul>
<blockquote>
<p><strong>✚  인터페이스는 추상클래스</strong>로 사용됩니다. 메소드의 형태만 선언해서 인터페이스를 정의하고, 이후에 클래스를 정의할 때 implements 키워드를 사용하면서 이 인터페이스를 지정하면, 이 클래스는 추상함수로 선언된 메소드를 받아들여야 합니다.</p>
</blockquote>
<h3 id="언제-인터페이스를-사용해야-하나">언제 인터페이스를 사용해야 하나?</h3>
<ul>
<li><strong>객체의 구조를 정의</strong>하고 이를 확장해야 할 때.</li>
<li><strong>확장 가능성이 있는 타입</strong>을 정의할 때, 특히 외부 모듈과 협업할때 주로 사용</li>
<li><strong>클래스와 함께 사용</strong>하여 구조를 공유하거나 상속 구조를 만들 때.</li>
</ul>
<pre><code class="language-tsx">interface Animal {
  name: string;
}

interface Dog extends Animal {
  breed: string;
}

const myDog: Dog = {
  name: &quot;Buddy&quot;,
  breed: &quot;Golden Retriever&quot;,
};

// 인터페이스 확장
interface Person {
  name: string;
  age: number;
}

// 선언적 확장: 기존 인터페이스에 속성 추가
interface Person {
  location: string;
}

const person: Person = {
  name: &quot;Alice&quot;,
  age: 30,
  location: &quot;New York&quot;,
};
</code></pre>
<h2 id="3-제네릭generic을-사용하여-확장하거나-제한하는-경우">3. 제네릭(Generic)을 사용하여 확장하거나 제한하는 경우</h2>
<blockquote>
<p>제네릭은 타입을 파라미터화하여 <strong>유연하게 여러 타입을 처리</strong>할 수 있도록 합니다.</p>
</blockquote>
<ul>
<li>제네릭의 타입 확장과 제한 :<code>extends</code> 키워드</li>
</ul>
<h3 id="언제-제네릭을-사용">언제 제네릭을 사용?</h3>
<ul>
<li><strong>다양한 타입을 처리해야 할 때</strong>: 함수, 클래스, 인터페이스 등에서 다형성 제공.</li>
<li><strong>타입을 명확히 제약하거나 확장</strong>해야 할 때 (<code>extends</code>, <code>T extends keyof U</code> 등).</li>
<li><strong>반환 타입과 입력 타입이 <code>동적으로 결정</code></strong>되어야 할 때.</li>
</ul>
<pre><code class="language-tsx">// 제네릭으로 타입을 제한
function getValue&lt;T extends { name: string }&gt;(obj: T): string {
  return obj.name;
}

getValue({ name: &quot;Alice&quot; }); // 정상
// getValue({ age: 30 }); // 오류: 'name' 속성이 없기 때문

// 제네릭을 사용한 인터페이스
interface Box&lt;T&gt; {
  value: T;
}

const numberBox: Box&lt;number&gt; = { value: 123 };
const stringBox: Box&lt;string&gt; = { value: &quot;Hello&quot; };</code></pre>
<h2 id="4-클래스-상속-extends을-통한-타입-확장">4. 클래스 상속 (extends)을 통한 타입 확장</h2>
<blockquote>
<p>클래스 상속은 새로운 클래스를 정의할 때 기존 클래스의 속성과 메서드를 재사용하여 확장하는 가장 기본적인 방법</p>
</blockquote>
<h3 id="사용-상황">사용 상황:</h3>
<ul>
<li>기본 클래스의 기능을 확장하거나, 기본 기능을 유지하면서 새로운 기능을 추가해야 할 때.</li>
<li>여러 클래스 간의 공통 기능을 부모 클래스로 추출하여 재사용할 때.</li>
</ul>
<pre><code class="language-tsx">interface Movable {
  move(): void;
}

interface Barkable {
  bark(): void;
}

class Dog implements Movable,Barkable {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  move() {
    console.log(`${this.name} is moving.`);
  }

  bark() {
    console.log(`${this.name} barks.`);
  }
}

const dog = new Dog('Buddy');
dog.move(); // Buddy is moving.
dog.bark(); // Buddy barks.
</code></pre>
<h3 id="추상-클래스-abstract를-통한-타입-확장과-제한">추상 클래스 (abstract)를 통한 타입 확장과 제한</h3>
<blockquote>
<p>추상 클래스는 공통된 기능을 가진 클래스들의 기본 형태를 정의하고, 구체적인 구현은 서브클래스에서 하도록 강제하는 방식입니다.</p>
</blockquote>
<h3 id="사용-상황-1">사용 상황:</h3>
<ul>
<li>기본적인 메서드와 속성을 제공하면서, 구체적인 구현은 자식 클래스에 맡기고 싶을 때.</li>
<li>인스턴스화를 허용하지 않으면서, 상속을 <code>통해 공통 기능을 공유</code>해야 할 때.</li>
</ul>
<pre><code class="language-tsx">abstract class Animal {
  abstract makeSound(): void;

  move() {
    console.log('Moving...');
  }
}

class Dog extends Animal {
  makeSound() {
    console.log('Woof!');
  }
}

const dog = new Dog();
dog.makeSound(); // Woof!
dog.move(); // Moving...</code></pre>
<h2 id="상황으로-생각하기">상황으로 생각하기</h2>
<h3 id="상황">상황</h3>
<p>도서관에서 책을 분류하고 관리하는 시스템을 설계함. </p>
<pre><code class="language-tsx">📚 장르 : 문학, 과학, 역사, 미술 
💝 사은품 종류 : 책갈피, 포스터, 스티커 
💰 결재 상태 : 결재 됨, 안됨</code></pre>
<p><strong>[타입 영역]</strong> </p>
<p>도서관의 책은 다양한 <strong>장르</strong>가 있으며 책마다 <strong>사은품</strong>이 있을 수도 있고 없을 수도 있다</p>
<p><strong>[처리 영역]</strong> </p>
<p>사은품이 있는 경우→ <strong>결제 시 계산 포스기에서 사은품을 선택할 수 있는 옵션</strong>이 나타나야 한다</p>
<p>계산이 완료된 책→ <strong>계산 완료된 상품</strong>으로 표시되어야 하며, 이 정보는 포스 시스템에 반영</p>
<p>사용자는 결제 방법(카드, 현금 등)을 선택할 수 있으며, 결제 완료 후 사은품에 존재 여부에 따라 추가적인 결제 흐름과 마지막엔 가격이 나온다</p>
<p><strong>타입 영역</strong> </p>
<p>장르와 사은품 결재 상태는 특정형태로 유지되어서 타입을 사용</p>
<pre><code class="language-tsx">// 책의 장르를 정의
type Genre = '문학' | '과학' | '역사' | '미술';

// 사은품 종류를 정의
type GiftType = '책갈피' | '포스터' | '스티커';

// 결제 상태를 정의
type PaymentStatus = 'paid' | 'unpaid';
</code></pre>
<p><strong>인터페이스(Interface) 영역</strong></p>
<p>인터페이스는 책의 구조를 정의하며, 사은품의 유무와 결제 상태와 같은 속성을 추가로 확장하여 사용할 수 있다. </p>
<pre><code class="language-tsx">// 책의 구조 정의 - 사은품과 결제 상태를 포함하여 타입 제한
interface Book {
  title: string;
  genre: Genre;
  price: number;
  gift?: GiftType; // 사은품은 있을 수도, 없을 수도 있음
  status: PaymentStatus;
}

// 결제 처리와 관련된 인터페이스
interface Payable {
  calculatePrice(): number;
  showPaymentOptions(): void;
}</code></pre>
<p><strong>제네릭 클래스 영역</strong> </p>
<p>제네릭을 사용하여 사은품의 유무와 결제 여부를 처리한다. : 제네릭을 통해 다양한 결제 상태와 사은품 여부를 쉽게 확장하거나 조합</p>
<p>추상 클래스는 기본적으로 책과 관련된 메서드를 정의하며, 실제 계산 로직은 서브클래스에서 구현</p>
<pre><code class="language-tsx">// 추상 클래스 정의 - 결제 로직
// payable + Book타입을 확장하는 T
abstract class LibraryItem&lt;T extends Book&gt; implements Payable {
  protected book: T;

  constructor(book: T) {
    this.book = book;
  }

  // 입장 조건: 모든 서브클래스는 이 메서드를 구현해야 함
  abstract calculatePrice(): number;

  showPaymentOptions(): void {
    console.log('카드와 신용카드로 결제가 가능합니다 ');
  }

  showPaidStatus() {
    if (this.book.status === 'paid') {
      console.log('결제가 이미 완료됨');
    } else {
      console.log('결제 아직 안됨');
    }
  }
}

// 제네릭 클래스 - 사은품과 결제 여부에 따른 처리
class ProcessBook&lt;T extends Book&gt; extends LibraryItem&lt;T&gt; {
  // 입장 조건: Book 타입을 확장하는 T 타입을 처리
  calculatePrice(): number {
    if (this.book.gift) {  //추상클래스 사용
      console.log(`사은품포함 : ${this.book.gift}`);
      return this.book.price + 5; // 사은품이 있을 경우 추가 비용
    }
    return this.book.price;
  }//사은품에 따른 가격 수정

  displayBookDetails() {
    console.log(`Title: ${this.book.title}, Genre: ${this.book.genre}`);
    this.showPaidStatus();
  }
}

// 특정 책 인스턴스 생성 및 처리
const myBook: Book = {
  title: '노인과바다',
  genre: '문학',
  price: 20,
  gift: '책갈피',
  status: 'paid',
};

//LibraryItem의 생성자 호출: ProcessBook 클래스는 LibraryItem 추상 클래스를 상속받아
//ProcessBook의 생성자는 먼저 LibraryItem의 생성자를 호출하여 this.book을 초기화
//LibraryItem의 생성자에서 전달된 myBook -&gt; this.book에 할당. 
const processedBook = new ProcessBook(myBook);
processedBook.displayBookDetails(); // 책 정보 및 결제 상태 표시
console.log(`Total price: ${processedBook.calculatePrice()}원`); // 총 금액 계산
</code></pre>
<h2 id="🤔-퀴즈">🤔 퀴즈</h2>
<p><strong>타입(Type) 확장 방식</strong>으로 사용되는 것 중 맞는 것은?</p>
<ol>
<li>extends 키워드</li>
<li>유니온 타입</li>
<li>추상 클래스 </li>
</ol>
<ul>
<li>정답<ol>
<li>유니온 타입</li>
</ol>
</li>
</ul>
<p><strong>제네릭(Generic)의 타입에서</strong> <code>extends</code> 키워드의 역할은 무엇인가요?</p>
<ul>
<li><p>정답</p>
<p>  제네릭 타입의 범위를 제한하거나 특정 조건을 부과합니다.</p>
</li>
</ul>