<ol>
<li>API 명세읽기</li>
<li>string으로 표현하기, 타입 이름은 해당 분야로 짓기 </li>
<li>string 지양하기 → 구체적인 타입으로 좁히기</li>
<li>공식 명칭에는 상표 붙이기</li>
</ol>
<h1 id="예제-1-가상의-사용자-정보-api">예제 1: 가상의 사용자 정보 API</h1>
<p><strong>API 엔드포인트</strong>: <code>https://api.example.com/users</code></p>
<p><strong>응답 예시</strong>:</p>
<pre><code class="language-json">{
  &quot;id&quot;: 1,
  &quot;name&quot;: &quot;John Doe&quot;,
  &quot;email&quot;: &quot;john.doe@example.com&quot;,
  &quot;address&quot;: {
    &quot;street&quot;: &quot;123 Main St&quot;,
    &quot;city&quot;: &quot;Anytown&quot;,
    &quot;zipcode&quot;: &quot;12345&quot;
  },
  &quot;phone&quot;: &quot;123-456-7890&quot;,
  &quot;website&quot;: &quot;https://example.com&quot;
}</code></pre>
<h3 id="1-string으로-일단-만들기">1. string으로 일단 만들기</h3>
<pre><code class="language-tsx">// Address 타입 정의
interface Address {
  street: string;
  city: string;
  zipcode: string;
}

// User 타입 정의
interface User {
  id: number;
  name: string;
  email: string;
  address: Address;
  phone: string;
  website: string;
}</code></pre>
<p><strong>API 호출 함수</strong></p>
<p><code>fetch</code>를 사용하여 데이터를 호출하고 타입을 적용합니다.</p>
<pre><code class="language-tsx">async function fetchUsers(): Promise&lt;User[]&gt; {
  const response = await fetch(&quot;https://api.example.com/users&quot;);
  if (!response.ok) {
    throw new Error(&quot;Failed to fetch users&quot;);
  }
  return await response.json();
}</code></pre>
<h3 id="2-string을-지양하기">2. string을 지양하기</h3>
<p>: 이메일과 웹사이트URL 전화번호 타입 구체화 하기</p>
<pre><code class="language-tsx">// 이메일 타입 정의
type EmailAddress = `${string}@${string}.${string}`;

// 웹사이트 URL 타입 정의
type WebsiteURL = `http${&quot;s&quot; | &quot;&quot;}://${string}.${string}`;

// 전화번호 타입 정의
type PhoneNumber = `${number}-${number}-${number}`;

// Address 타입 정의
interface Address {
  street: string;
  city: string;
  zipcode: `${number}`; // 우편번호를 숫자로 제한
}

// User 타입 정의
interface User {
  id: number;
  name: string;
  email: EmailAddress; // 엄격한 이메일 형식
  address: Address;
  phone: PhoneNumber; // 간단한 전화번호 형식
  website: WebsiteURL; // 웹사이트 URL 형식
}
</code></pre>
<p><strong>올바른 값</strong></p>
<pre><code class="language-tsx">const user: User = {
  id: 1,
  name: &quot;John Doe&quot;,
  email: &quot;john.doe@example.com&quot;,
  address: {
    street: &quot;123 Main St&quot;,
    city: &quot;Anytown&quot;,
    zipcode: &quot;12345&quot;
  },
  phone: &quot;123-456-7890&quot;,
  website: &quot;https://example.com&quot;
};</code></pre>
<p><strong>잘못된 값 (컴파일 오류 발생)</strong></p>
<pre><code class="language-tsx">const invalidUser: User = {
  id: 2,
  name: &quot;Jane Doe&quot;,
  email: &quot;jane.doe[at]example.com&quot;, // 오류: 이메일 형식이 아님
  address: {
    street: &quot;456 Elm St&quot;,
    city: &quot;Othertown&quot;,
    zipcode: &quot;abcde&quot; // 오류: 숫자 형식이 아님
  },
  phone: &quot;1234567890&quot;, // 오류: 전화번호 형식이 아님
  website: &quot;example.com&quot; // 오류: URL 형식이 아님
};</code></pre>
<h3 id="4-브랜딩하기">4. 브랜딩하기</h3>
<p>: 있으면 하자 !</p>
<h2 id="그렇다면-any와-unknown은-명세서와-타입-설계시-언제-사용하나요-">그렇다면 any와 unknown은 명세서와 타입 설계시 언제 사용하나요 ?</h2>
<p><strong>any, unknown 특징</strong> </p>
<table>
<thead>
<tr>
<th><strong>특징</strong></th>
<th><strong><code>unknown</code></strong></th>
<th><strong><code>any</code></strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>안전성</strong></td>
<td>타입을 사용하기 전에 <strong>검사 필수</strong></td>
<td>모든 작업 허용 (타입 검사 없음)</td>
</tr>
<tr>
<td><strong>타입 검사</strong></td>
<td>엄격 (타입을 좁혀야만 사용 가능)</td>
<td>느슨함 (타입 체크가 무시됨)</td>
</tr>
<tr>
<td><strong>IntelliSense</strong></td>
<td>타입 좁히기 후 사용 가능</td>
<td>IntelliSense 거의 없음</td>
</tr>
<tr>
<td><strong>사용 시기</strong></td>
<td>외부 데이터, 알 수 없는 타입</td>
<td>빠른 프로토타이핑 또는 유연성이 필요할 때</td>
</tr>
</tbody></table>
<h3 id="언제-unknown을-사용할까"><strong>언제 <code>unknown</code>을 사용할까?</strong></h3>
<p>: <code>unknown</code>은 타입 안전성을 유지해야 하지만 데이터의 구체적인 타입을 사전에 알 수 없는 경우에 적합합니다.</p>
<ol>
<li><strong>외부에서 오는 데이터 (</strong>API 응답 구조가 문서화되지 않았거나 동적으로 변할 가능성이 있을 때.)</li>
</ol>
<pre><code class="language-tsx">async function fetchData(url: string): Promise&lt;unknown&gt; {
  const response = await fetch(url);
  return response.json();
}

async function processData() {
  const data: **unknown**= await fetchData(&quot;https://api.example.com/data&quot;);

  // 타입 좁히기 필요
  if (Array.isArray(data)) {
    console.log(`Array of length: ${data.length}`);
  } else if (typeof data === &quot;object&quot; &amp;&amp; data !== null) {
    console.log(`Object with keys: ${Object.keys(data)}`);
  } else {
    console.log(&quot;Unknown data type&quot;);
  }
}</code></pre>
<ul>
<li><strong>장점</strong>: 타입을 검사하기 전까지는 데이터를 조작할 수 없으므로, 실수를 방지할 수 있습니다.</li>
<li><strong>단점</strong>: 추가적인 타입 검사 코드가 필요합니다.</li>
</ul>
<ol start="2">
<li><strong>안전하게 다뤄야 할 데이터가 있을 때</strong></li>
</ol>
<p><code>unknown</code>은 강제로 타입을 좁혀야 하기 때문에 더 안전하게 데이터를 사용할 수 있습니다.</p>
<pre><code class="language-tsx">function processValue(value: unknown) {
  if (typeof value === &quot;string&quot;) {
    console.log(value.toUpperCase()); // 안전
  } else if (typeof value === &quot;number&quot;) {
    console.log(value.toFixed(2)); // 안전
  } else {
    console.log(&quot;Unsupported type&quot;);
  }
}</code></pre>
<hr />
<h3 id="언제-any를-사용할까"><strong>언제 <code>any</code>를 사용할까?</strong></h3>
<p>: <code>any</code>는 타입 안전성을 무시하고 빠르게 코드를 작성해야 할 때 사용합니다. 하지만 남용을 피해야 하며, 가능한 빨리 정확한 타입으로 대체하는 것이 좋습니다.</p>
<ol>
<li><strong>빠른 프로토타이핑</strong></li>
</ol>
<p>개발 초기 단계에서 타입 정의가 아직 명확하지 않거나, 많은 데이터를 테스트해야 할 때 유용합니다.</p>
<pre><code class="language-tsx">function logData(data: any) {
  console.log(data);
}

logData({ id: 1, name: &quot;John Doe&quot; });
logData(&quot;This is a string&quot;);
logData(42);</code></pre>
<ul>
<li><strong>장점</strong>: 유연성.</li>
<li><strong>단점</strong>: 모든 작업이 허용되므로, 타입 오류를 놓칠 가능성이 큽니다.</li>
</ul>
<ol start="2">
<li><strong>타입 검사가 필요 없는 경우</strong></li>
</ol>
<p>특정 로직이 타입에 구애받지 않고 모든 데이터 타입을 다뤄야 할 때 사용합니다.</p>
<pre><code class="language-tsx">function mergeObjects(obj1: any, obj2: any): any {
  return { ...obj1, ...obj2 };
}</code></pre>
<ul>
<li>예: 데이터 직렬화/역직렬화, <code>JSON.parse()</code>, 로깅.</li>
</ul>
<hr />
<h3 id="unknown-vs-any-사용-사례-비교"><code>unknown</code> vs <code>any</code> 사용 사례 비교</h3>
<pre><code class="language-tsx">const jsonData: **any** = JSON.parse('{&quot;id&quot;: 1, &quot;name&quot;: &quot;John&quot;}');
console.log(jsonData.toUpperCase()); // 런타임 오류 발생 가능 (TypeScript가 경고하지 않음)

const unknownData: **unknown** = JSON.parse('{&quot;id&quot;: 1, &quot;name&quot;: &quot;John&quot;}');
if (typeof unknownData === &quot;string&quot;) {
  console.log(unknownData.toUpperCase()); // 비교적 안전
} else {
  console.log(&quot;Data is not a string&quot;);
}</code></pre>
<ul>
<li><code>any</code>: 런타임 오류 가능성이 있음.</li>
<li><code>unknown</code>: 타입 검사 후 사용 가능, 타입 안전성 증가.</li>
</ul>
<h1 id="예제-2-상표가-있는-api">예제 2: 상표가 있는 API</h1>
<pre><code class="language-tsx">{
  &quot;id&quot;: &quot;12345&quot;,
  &quot;name&quot;: &quot;Apple MacBook Pro&quot;,
  &quot;category&quot;: &quot;laptop&quot;,
  &quot;price&quot;: 2499.99,
  &quot;currency&quot;: &quot;USD&quot;,
  &quot;availability&quot;: {
    &quot;inStock&quot;: true,
    &quot;estimatedDeliveryDays&quot;: 5
  },
  &quot;rating&quot;: {
    &quot;average&quot;: 4.8,
    &quot;reviews&quot;: 1245
  }
}</code></pre>
<h3 id="1-api-명세-읽고-타입-설계-string-중심"><strong>1. API 명세 읽고 타입 설계 (string 중심)</strong></h3>
<p>우선, API 데이터 구조를 단순히 <code>string</code> 및 기타 기본 타입으로 표현합니다.</p>
<pre><code class="language-tsx">interface Product {
  id: string; // 제품 ID
  name: string; // 제품 이름
  category: string; // 제품 카테고리
  price: number; // 제품 가격
  currency: string; // 통화 단위
  availability: {
    inStock: boolean; // 재고 여부
    estimatedDeliveryDays: number; // 예상 배송일
  };
  rating: {
    average: number; // 평균 평점
    reviews: number; // 리뷰 수
  };
}</code></pre>
<h3 id="2-string으로-표현된-타입-이름을-해당-분야로-지정"><strong>2. <code>string</code>으로 표현된 타입 이름을 해당 분야로 지정</strong></h3>
<p>타입 이름을 구체화하여 해당 데이터와 연관된 도메인 용어를 반영합니다.</p>
<pre><code class="language-tsx">type ProductID = string;
type ProductName = string;
type ProductCategory = string;
type Currency = string;

interface Product {
  id: ProductID;
  name: ProductName;
  category: ProductCategory;
  price: number;
  currency: Currency;
  availability: {
    inStock: boolean;
    estimatedDeliveryDays: number;
  };
  rating: {
    average: number;
    reviews: number;
  };
}</code></pre>
<p>이 단계에서는 단순히 <strong>의미를 부여한 별칭</strong>을 도입했습니다. 타입 자체는 여전히 <code>string</code> 기반입니다.</p>
<h3 id="3-string-지양하고-구체적인-타입으로-좁히기"><strong>3. <code>string</code> 지양하고 구체적인 타입으로 좁히기</strong></h3>
<p>이제 더 구체적인 타입으로 좁힙니다. 예를 들어:</p>
<ul>
<li><em><code>Currency</code></em>는 ISO 4217 표준 코드로 한정합니다.</li>
<li><em><code>ProductCategory</code></em>는 정해진 카테고리 값으로 제한합니다.</li>
</ul>
<pre><code class="language-tsx">type ProductID = `${number}`; // ID는 숫자 기반 문자열
type ProductName = string;

type ProductCategory = &quot;laptop&quot; | &quot;smartphone&quot; | &quot;tablet&quot; | &quot;accessory&quot;; // 카테고리 제한

type Currency = &quot;USD&quot; | &quot;EUR&quot; | &quot;JPY&quot;; // ISO 4217 통화 코드

interface Product {
  id: ProductID;
  name: ProductName;
  category: ProductCategory;
  price: number;
  currency: Currency;
  availability: {
    inStock: boolean;
    estimatedDeliveryDays: number;
  };
  rating: {
    average: number;
    reviews: number;
  };
}</code></pre>
<p>이 단계에서는 <strong><code>string</code></strong> 대신 <strong>구체적인 값</strong>으로 제한하여 데이터 구조의 타입 안전성을 향상시켰습니다.</p>
<h3 id="4-공식-명칭에-상표branding-붙이기"><strong>4. 공식 명칭에 상표(branding) 붙이기</strong></h3>
<p>마지막으로, <code>상표</code>를 추가하여 타입의 고유성을 강화합니다. 상표를 추가하면 다른 동일 구조의 타입과 구별할 수 있습니다.</p>
<pre><code class="language-tsx">type ProductID = `${number}` &amp; { _brand: &quot;ProductID&quot; };
type ProductName = string &amp; { _brand: &quot;ProductName&quot; };

type ProductCategory = &quot;laptop&quot; | &quot;smartphone&quot; | &quot;tablet&quot; | &quot;accessory&quot; &amp; { _brand: &quot;ProductCategory&quot; };

type Currency = &quot;USD&quot; | &quot;EUR&quot; | &quot;JPY&quot; &amp; { _brand: &quot;Currency&quot; };

interface Product {
  id: ProductID;
  name: ProductName;
  category: ProductCategory;
  price: number;
  currency: Currency;
  availability: {
    inStock: boolean;
    estimatedDeliveryDays: number;
  };
  rating: {
    average: number;
    reviews: number;
  };
}</code></pre>
<ul>
<li>이제 <code>ProductID</code>나 <code>Currency</code>는 동일한 구조의 다른 <code>string</code>과 구별됩니다.</li>
<li>타입 간 혼동을 방지하고, 잘못된 데이터 사용 가능성을 줄입니다.</li>
</ul>
<pre><code class="language-tsx">const macbook: Product = {
  id: &quot;12345&quot; as ProductID,
  name: &quot;Apple MacBook Pro&quot; as ProductName,
  category: &quot;laptop&quot; as ProductCategory,
  price: 2499.99,
  currency: &quot;USD&quot; as Currency,
  availability: {
    inStock: true,
    estimatedDeliveryDays: 5,
  },
  rating: {
    average: 4.8,
    reviews: 1245,
  },
};</code></pre>
<h3 id="퀴즈-️">퀴즈 ‼️</h3>
<p>아래의 <code>Product</code> 타입은 특정 API 명세를 기반으로 설계되었습니다. 다음 중 <strong>올바르지 않은</strong> 부분은 무엇일까요 </p>
<pre><code class="language-tsx">type ProductID = `${number}` &amp; { _brand: &quot;ProductID&quot; };
type ProductCategory = &quot;laptop&quot; | &quot;smartphone&quot; | &quot;tablet&quot; | &quot;accessory&quot; &amp; { _brand: &quot;ProductCategory&quot; };
type Currency = &quot;USD&quot; | &quot;EUR&quot; | &quot;JPY&quot; &amp; { _brand: &quot;Currency&quot; };

interface Product {
  id: ProductID;
  name: string;
  category: ProductCategory;
  price: number;
  currency: Currency;
  availability: {
    inStock: boolean;
    estimatedDeliveryDays: number;
  };
}</code></pre>
<pre><code class="language-tsx">const productB: Product = {
  id: &quot;102&quot; as ProductID,
  name: &quot;Apple iPad&quot;,
  category: &quot;tablet&quot; as ProductCategory,
  price: 599.99,
  currency: &quot;GBP&quot; as Currency,
  availability: {
    inStock: false,
    estimatedDeliveryDays: 7,
  },
};</code></pre>
<ul>
<li><p>답</p>
<p>  <code>currency</code>가 GBP로 설정되어 있습니다. <code>Currency</code> 타입은 <code>&quot;USD&quot;</code>, <code>&quot;EUR&quot;</code>, <code>&quot;JPY&quot;</code>만 허용합니다.</p>
</li>
</ul>