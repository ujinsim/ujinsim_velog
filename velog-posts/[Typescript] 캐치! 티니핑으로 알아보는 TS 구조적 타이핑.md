<p>구조적 타이핑 : 어 난 티니핑 멤버 아닌데.. </p>
<p><a href="https://toss.tech/article/typescript-type-compatibility%EA%B8%80%EA%B3%BC">https://toss.tech/article/typescript-type-compatibility글과</a> Effective 타입스크립트 책을 보고 정리 하였습니다 ! </p>
<hr />
<h2 id="덕-타이핑-duck-typing"><strong>덕 타이핑 (Duck Typing)</strong></h2>
<p>덕 타이핑은 &quot;만약 어떤 것이 오리처럼 걷고, 헤엄치고, 꽥꽥거린다면 그것은 오리일 것이다&quot;라는 철학에서 유래된 개념입니다. 즉, <strong>객체가 어떤 인터페이스나 타입을 명시적으로 구현하지 않더라도, 해당 객체가 요구되는 기능을 모두 구현하고 있다면 그 객체를 사용할 수 있다</strong>는 의미입니다.</p>
<p>타입스크립트에서도, 객체가 특정 속성과 메서드를 가지고 있다면 타입 선언 없이도 그 객체가 예상대로 동작할 수 있습니다. 이것이 타입스크립트의 <strong>구조적 서브타이핑</strong> 방식과 연결됩니다.</p>
<h2 id="핑-타이핑-ping-typing"><strong>핑 타이핑 (Ping Typing)</strong></h2>
<p>덕 타이핑에서 아이디어를 차용해, <strong>어떤 글자 뒤에 &quot;핑&quot;을 붙이면 그 글자가 &lt;&lt;캐치! 티니핑&gt;&gt;의 멤버가 된다는 발상</strong>을 농담처럼 표현해보았습니다</p>
<h2 id="만약-어떤-글자-뒤에-핑이라고-붙이면--캐치-티니핑의-멤버가-된다--→-핑-타이핑--">“만약 어떤 글자 뒤에 핑이라고 붙이면 &lt;&lt; 캐치! 티니핑&gt;&gt;의 멤버가 된다… ? → 핑 타이핑 ? “</h2>
<pre><code class="language-python">function isTiniPing(name: string): boolean {
  return name.endsWith('핑');
}

const name = &quot;하츄핑&quot;;
if (isTiniPing(name)) {
  console.log(`${name}은(는) 티니핑 멤버입니다!`);
}</code></pre>
<hr />
<p><strong>🤔 그렇다면 서브타이핑의 종류에는 어떤 것이 있을까요 ??</strong> </p>
<h2 id="서브타이핑의-종류">서브타이핑의 종류</h2>
<p>타입의 계충 구조에서 한 타입이 다른 타입의 부분 집합일 때 발생하는 타입 관계</p>
<h3 id="1-명목적-서브타이핑nominal-subtyping-→-java-c">1. 명목적 서브타이핑(nominal subtyping) → java, C#</h3>
<blockquote>
<p>타입 정의 시에 상속 관계임을 명확히 명시한 경우에만 타입 호환을 허용하는 것입니다.</p>
</blockquote>
<ul>
<li>개발자의 명확한 의도를 반영할 수 있다</li>
<li>오류 발생할 가능성 배제</li>
</ul>
<h3 id="2-구조적-서브타이핑structural-subtpying-→-ts">2. 구조적 서브타이핑(structural subtpying) → TS</h3>
<blockquote>
<p>상속 관계가 명시되어 있지 않더라도 객체의 프로퍼티를 기반으로 사용처에서 사용함에 문제가 없다면 타입 호환을 허용하는 방식</p>
</blockquote>
<ul>
<li>객체의 프로퍼티를 체크해주는 과정 수행</li>
<li>상속관계를 명시해줄 필요가 없음</li>
</ul>
<hr />
<p><strong>🤔 타입스크립트에서 사용하는 구조적 타이핑의 예시를 알아보겠습니다</strong> </p>
<h2 id="구조적-타이핑의-예시">구조적 타이핑의 예시</h2>
<h3 id="-봉인된-타입과-정확한-타입">: 봉인된 타입과 정확한 타입</h3>
<p>자바스크립트와 타입스크립트는 기본적으로 타입 시스템이 <strong>열려있는(open)</strong> 상태</p>
<p>즉, 객체가 타입에 명시된 속성 외에도 추가적인 속성을 가질 수 있으며, 타입 체크는 해당 속성들만 맞는지를 확인합니다</p>
<ul>
<li><strong>해당 속성들만 맞는지만 확인</strong></li>
</ul>
<pre><code class="language-jsx">interface Vector2D {
  x: number;
  y: number;
}

function calculateLength(v: Vector2D) {
  return Math.sqrt(v.x * v.x + v.y * v.y);
}

interface NamedVector {
  name: string;
  x: number;
  y: number;
}

//calculateLength 함수에 NamedVector 타입의 객체 v를 전달할 수 있다.
// 왜냐하면, v가 Vector2D에서 요구하는 x와 y 속성을 가지고 있음.
const v: NamedVector = { x: 3, y: 4, name: &quot;zee&quot; };
console.log(calculateLength(v)); // 5
</code></pre>
<ul>
<li><strong>객체가 2개의 속성밖에 못가지지만 , 3개의 속성을 추가하여 무시됨</strong></li>
</ul>
<pre><code class="language-jsx">interface Vector2D {
  x: number;
  y: number;
}

const vector = { x: 3, y: 4, z: 5 }; // z는 추가적인 속성

function calculateLength(v: Vector2D) {
  return Math.sqrt(v.x * v.x + v.y * v.y);
}

console.log(calculateLength(vector)); // 5, z는 무시됨
</code></pre>
<ul>
<li><strong>객체가 여러 속성을 가질 수 있지만, 필요한 속성만 이용함</strong></li>
</ul>
<pre><code class="language-jsx">interface Point3D {
  x: number;
  y: number;
  z: number;
}

function calculate2DLength(point: { x: number; y: number }) {
  return Math.sqrt(point.x * point.x + point.y * point.y);
}

const point3D: Point3D = { x: 5, y: 12, z: 7 };

console.log(calculate2DLength(point3D)); // 13
</code></pre>
<blockquote>
<p>type이름이 티니핑인데 속성이 눈코입이면 눈코입 속성을 가진 다른 캐릭터 또한 들어올 수 있게됩니다  때문에 type 속성에는 들어오지만</p>
</blockquote>
<ul>
<li>명확한 경우</li>
<li>애매한 경우</li>
</ul>
<p>가 공존할 수 있게 됩니다 👉 그래서 타입 구분을 통해 이를 해결할 수 있습니다</p>
<hr />
<h2 id="타입-구분-방법">타입 구분 방법</h2>
<h3 id="1-특정-경우에만-해당하는-변수를-타입으로-지정">1. 특정 경우에만 해당하는 변수를 타입으로 지정</h3>
<p>: 캐릭터 속성에서 티니핑 타입을 정의하여 구분할 수 있습니다 </p>
<pre><code class="language-tsx">// 기본 캐릭터 속성 정의
type Character = {
  strength: number;
  agility: number;
  intelligence: number;
};

// 티니핑 타입 정의 (Character에 추가 속성 포함)
type 티니핑 = Character &amp; {
  characterBrand: string;
};

// 티니핑 객체 예시
const character1: 티니핑 = {
  strength: 29,
  agility: 48,
  intelligence: 13,
  characterBrand: '캐치! 티니핑'
};

// 일반 캐릭터와 티니핑 캐릭터 구분
const character2: Character = {
  strength: 25,
  agility: 40,
  intelligence: 15
};

console.log(character1.characterBrand); // &quot;캐치! 티니핑&quot;</code></pre>
<p><strong>그렇다면 타입이 아닌 character2에만 가능한 characterBrand 프로퍼티 추가를 한다면 ?</strong> </p>
<pre><code class="language-jsx">const character2 = Character({
  strength: 12;
  agility: 32;
  intelligence: 434;
  characterBrand: '캐치! 티니핑'
})                                  /** 타임검사 결과 : 오류 있음 */

//다음처럼 함수에 들어온 인자가 fresh하면 해당 함수에서만 사용되고 다른 곳에서 사용되지 않기에 오류로 지정 </code></pre>
<p>TypeScript Type Checker는 구조적 서브타이핑을 기반으로 타입 호환을 판단하되, Freshness에 따라 예외를 둔다고 합니다.</p>
<h3 id="2-branded-type--프로퍼티-추가-">2. Branded type ( 프로퍼티 추가 )</h3>
<p>의도적으로 <code>__brand</code> 와 같은 프로퍼티를 추가시켜, 개발자가 함수의 매개변수로 정의한 타입 외에는 호환이 될 수 없도록 강제하는 기법</p>
<p>2-1 . 타입에 <strong>고유한 식별자</strong>를 부여하는 방식 </p>
<p>: 브랜트 타입에 브랜드 속성을 '캐치! 티니핑’으로 부여하여 티니핑 타입을 정의할 수 있습니다</p>
<pre><code class="language-jsx">type Brand&lt;K, T&gt; = K &amp; { __brand: T };

// 티니핑 타입 정의
type 티니핑 = Brand&lt;{
  strength: number;
  agility: number;
  intelligence: number;
}, '캐치! 티니핑'&gt;;

// 티니핑의 능력치를 계산하는 함수
function calculatePower(character: 티니핑): number {
  return character.strength * 2 + character.agility * 1.5 + character.intelligence * 3;
}

// 정석적인 방법으로 브랜디드 타입 사용
const brandedCharacter2 = {
  strength: 29,
  agility: 48,
  intelligence: 13,
  __brand: '캐치! 티니핑' as const // __brand 속성 추가로 명확하게 타입 지정
};

console.log(calculatePower(brandedCharacter2)); // OK
</code></pre>
<p>2-2. 함수에서의 타입 제한</p>
<p>: 어떤 글자가 들어올 때 핑으로 끝나더라도 타입을 통해 티니핑 캐릭터인지 확인할 수 있습니다</p>
<pre><code class="language-jsx">// Character type 정의
type Character&lt;K, T&gt; = K &amp; { _Character: T };

// 티니핑 타입 정의
type 티니핑 = Character&lt;string, '티니핑'&gt;;

// 마지막 글자가 '핑'으로 끝나는지 체크하는 함수
function is티니핑(name: string): name is 티니핑 {
  return name.endsWith('핑');
}

// 티니핑 캐릭터만 허용하는 함수
//매개변수 character타입 외 제한
function introduceCharacter(character: 티니핑) {
  console.log(`안녕하세요, 저는 ${character}입니다!`);
}

// 일반 문자열과 티니핑 문자열 예시
const Character1 = &quot;구조적타이핑&quot;;
const Character2 = &quot;하츄핑&quot; as const;

// 타입 검사
if (is티니핑(tinipingCharacter)) {
  introduceCharacter(tinipingCharacter); // OK
} else {
  console.log(`${tinipingCharacter}는 티니핑이 아닙니다.`);
}

if (is티니핑(normalCharacter)) {
  introduceCharacter(normalCharacter); // 타입 오류 발생, normalCharacter는 티니핑이 아님
} else {
  console.log(`${normalCharacter}는 티니핑이 아닙니다.`);
}

//result 
안녕하세요, 저는 하츄핑입니다!
구조적타이핑는 티니핑이 아닙니다.</code></pre>
<h3 id="3-index-signature를-이용한-타입-호환성">3. Index Signature를 이용한 타입 호환성</h3>
<p>객체가 고정된 속성 외에도 임의의 속성을 가질 수 있음을 타입스크립트에게 알려주는 방법</p>
<p><strong>🤔 티니핑의 종류에 대해서 알아보자면..</strong></p>
<p>티니핑에서 가장 우수한 티니핑으로 로열 티니핑 6종이 있다고 합니다 </p>
<p>뿐만 아니라 <strong>레전드 티니핑</strong> , <strong>일반 티니핑</strong>,  <strong>빌런 티니핑</strong> 등 여러 종류가 있습니다 ! </p>
<p><a href="https://namu.wiki/w/%ED%8B%B0%EB%8B%88%ED%95%91#s-3.4">https://namu.wiki/w/티니핑#s-3.4</a></p>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/768be6b7-f65d-4e48-b272-79b3ff7e250b/image.png" /></p>
<p>해당 속성을 옵션 속성을 통해 추가로 알려줄 수 있습니다 </p>
<p>그럼 다음과 같이 기본 타입을 확장할 수 있습니다</p>
<pre><code class="language-jsx">// 기본 티니핑 타입 정의
interface 티니핑 {
  name: string;
  strength: number;
  agility: number;
  intelligence: number;
  [key: string]: number | string | boolean | undefined; // 추가적인 속성 허용
  type: string;  // 티니핑 종류 (일반, 레전드, 빌런 등)
}

// 레전드 티니핑 타입 정의 (티니핑을 확장)
interface 레전드티니핑 extends 티니핑 {
  legendaryPower: string;  // 레전드 티니핑만의 특별한 능력
}

// 빌런 티니핑 타입 정의 (티니핑을 확장)
interface 빌런티니핑 extends 티니핑 {
  evilPower: string;   // 빌런 티니핑만의 특별한 능력
  invertedTrianglePattern: boolean; 
  // 빌런 티니핑은 뿌뿌핑, 트러핑을 제외하면 눈밑에 역삼각형 무늬가 있음
}

// 일반 티니핑 타입 정의 (특별한 속성 없이 기본 티니핑 속성만 가짐)
type 일반티니핑 = 티니핑;

// 일반 티니핑 객체
const 일반TiniPing: 일반티니핑 = {
  name: &quot;키키핑&quot;,
  strength: 30,
  agility: 35,
  intelligence: 40,
  type: &quot;일반&quot;,
};

// 레전드 티니핑 객체
const 레전드TiniPing: 레전드티니핑 = {
  name: &quot;행운핑&quot;,
  strength: 80,
  agility: 70,
  intelligence: 90,
  type: &quot;레전드&quot;,
  legendaryPower: &quot;행운전달🍀&quot;, // 레전드 티니핑만의 특별한 능력
};

// 빌런 티니핑 객체
const 빌런TiniPing: 빌런티니핑 = {
  name: &quot;악동핑&quot;,
  strength: 60,
  agility: 55,
  intelligence: 65,
  type: &quot;빌런&quot;,
  evilPower: &quot;번개발사⚡️&quot;, // 빌런 티니핑만의 특별한 능력
  invertedTrianglePattern: true, // 역삼각형 무늬 여부
};

// 티니핑의 능력치를 계산하는 함수
function calculatePower(tiniPing: 티니핑): number {
  return tiniPing.strength * 2 + tiniPing.agility * 1.5 + tiniPing.intelligence * 3;
}

// 티니핑을 소개하는 함수
function introduceTiniPing(tiniPing: 티니핑) {
  console.log(`안녕하세요, 저는 ${tiniPing.name}입니다. 저는 ${tiniPing.type} 티니핑이에요!`);

  if ('legendaryPower' in tiniPing) {
    console.log(`저의 전설적인 능력은 ${tiniPing.legendaryPower}입니다!`);
  }

  if ('evilPower' in tiniPing) {
    console.log(`저의 악당 능력은 ${tiniPing.evilPower}입니다!`);
    if ((tiniPing as 빌런티니핑).invertedTrianglePattern) {
      console.log(&quot;저는 역삼각형 무늬를 가진 빌런 티니핑입니다!&quot;);
    }
  }

  console.log(`능력치 합계: ${calculatePower(tiniPing)}`);
}

// 티니핑 객체 출력
introduceTiniPing(일반TiniPing);   // 일반 티니핑 소개
introduceTiniPing(레전드TiniPing); // 레전드 티니핑 소개
introduceTiniPing(빌런TiniPing);   // 빌런 티니핑 소개

//strength: number; agility: number; intelligence: number;는 임의로 지정한 것 입니다 ! 
</code></pre>
<h2 id="정리-🧹"><strong>정리 🧹</strong></h2>
<table>
<thead>
<tr>
<th>방식</th>
<th>구분 방법</th>
<th>사용 목적</th>
</tr>
</thead>
<tbody><tr>
<td><strong>변수 타입 지정</strong></td>
<td>타입 이름</td>
<td>같은 구조의 데이터를 <strong>명확한 상황</strong>에 맞게 구분할 때 사용</td>
</tr>
<tr>
<td><strong>Branded Type</strong></td>
<td>고유한 식별자(__brand)</td>
<td>같은 구조라도 <strong>엄격한 타입 구분</strong>이 필요할 때 사용</td>
</tr>
<tr>
<td><strong>Index Signature</strong></td>
<td>속성의 타입만 제한</td>
<td>객체의 속성 이름을 <strong>동적으로 확장</strong>할 때 사용</td>
</tr>
</tbody></table>
<p><strong><em>📚 핑으로만 끝난다고 혹은 모든 티니핑이 일반 티니핑은 아니듯이 타입 구분의 필요성을 느꼈습니다!!</em></strong> </p>
<hr />
<h3 id="퀴즈-1-구조적-서브타이핑"><strong>퀴즈 1: 구조적 서브타이핑</strong></h3>
<p>다음 코드에서 <code>calculateLength</code> 함수에 <code>NamedVector</code> 타입의 객체를 전달할 수 있는 이유는 무엇일까요?</p>
<pre><code class="language-jsx">interface Vector2D {
  x: number;
  y: number;
}

function calculateLength(v: Vector2D) {
  return Math.sqrt(v.x * v.x + v.y * v.y);
}

interface NamedVector {
  name: string;
  x: number;
  y: number;
}

const v: NamedVector = { name: &quot;zee&quot;, x: 3, y: 4 };
console.log(calculateLength(v)); // 결과: 5
</code></pre>
<ul>
<li><p>정답</p>
<p>  <code>Vector2D</code>와 <code>NamedVector</code>는 명시적으로 상속 관계에 있고</p>
<p>  <code>NamedVector</code>는 <code>Vector2D</code>에서 요구하는 모든 속성(<code>x</code>, <code>y</code>)을 가지고 있기 때문</p>
<p>  <code>TypeScript</code>는 항상 모든 객체를 호환 가능하게 처리하기 때문</p>
<p>  <code>NamedVector</code>의 구조가 더 복잡하므로 자동으로 호환됨</p>
</li>
</ul>
<h3 id="퀴즈-2-명목적-서브타이핑"><strong>퀴즈 2: 명목적 서브타이핑</strong></h3>
<p>다음 언어 중 <strong>명목적 서브타이핑</strong>(Nominal Subtyping)을 사용하는 언어는 무엇일까요?</p>
<ol>
<li>TypeScript</li>
<li>JavaScript</li>
<li>Java</li>
<li>Python</li>
</ol>
<ul>
<li>정답<ol>
<li>Java는 명목적 서브타이핑을 사용하는 언어로, 상속 관계가 명확히 명시된 경우에만 타입 호환을 허용합니다</li>
</ol>
</li>
</ul>
<h3 id="퀴즈-3-브랜딩된-타입"><strong>퀴즈 3: 브랜딩된 타입</strong></h3>
<p>다음 코드를 실행할 때 타입 오류가 발생하는 이유는 무엇일까요?</p>
<pre><code class="language-jsx">type Brand&lt;K, T&gt; = K &amp; { __brand: T };

type Food = Brand&lt;{
  protein: number;
  carbohydrates: number;
  fat: number;
}, 'Food'&gt;;

function calcCalory(food: Food) {
  return food.protein * 4 + food.carbohydrates * 4 + food.fat * 9;
}

const burger = {
  protein: 100,
  carbohydrates: 100,
  fat: 100,
  burgerBrand: '버거킹'
};

calcCalory(burger); // 오류 발생
</code></pre>
<ul>
<li><p>정답</p>
<p>  <code>Food</code> 타입과 <code>burger</code> 타입이 구조적으로 일치하지 않기 때문에</p>
<ul>
<li><code>burger</code>에 <code>__brand: 'Food'</code>가 없기 때문에</li>
<li><code>calcCalory</code> 함수는 매개변수로 <code>burgerBrand</code>를 허용하지 않기 때문에</li>
</ul>
</li>
</ul>
<h3 id="퀴즈-4-index-signature">퀴즈 4: Index Signature</h3>
<p><strong>FlexibleFood 인터페이스가 Food와 다른 점은 무엇일까요?</strong></p>
<pre><code class="language-jsx">interface Food {
  protein: number;
  carbohydrates: number;
  fat: number;
}

interface FlexibleFood {
  protein: number;
  carbohydrates: number;
  fat: number;
  [key: string]: number | string; // 추가 속성 허용
}</code></pre>
<ul>
<li><p>정답</p>
<p>  <code>Food</code>는 고정된 속성만 가질 수 있다</p>
<p>  <code>FlexibleFood</code>는 <code>Food</code>와 달리 추가적인 속성을 허용한다</p>
<p>  <code>FlexibleFood</code>는 타입 호환성 문제를 발생시킨다</p>
<p>  <code>FlexibleFood</code>는 <code>Food</code>를 상속한 타입이다</p>
</li>
</ul>