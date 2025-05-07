<p>React에서 상태 관리와 데이터 변경을 다룰 때, <strong>깊은 복사 (Deep Copy)</strong>와 <strong>얕은 복사 (Shallow Copy)</strong>의 개념은 매우 중요합니다. 이러한 복사 방식은 주로 <strong>불변성 (Immutability)</strong>을 지키는 데 사용되며, React의 성능과 렌더링 효율성을 높이는 데 큰 역할을 합니다.</p>
<h3 id="1-원시-타입과-참조형-타입의-차이점">1. <strong>원시 타입과 참조형 타입의 차이점</strong></h3>
<ul>
<li><p><strong>원시 타입</strong> (Primitive Types): JavaScript의 숫자, 문자열, 불리언, <code>null</code>, <code>undefined</code>와 같은 타입을 포함합니다. 원시 타입은 <strong>값 자체</strong>를 저장하고, 변수에 값을 할당해도 다른 변수에는 영향을 주지 않습니다.</p>
<ul>
<li><p>예시:</p>
<pre><code class="language-jsx">  let a = 10;
  let b = a;
  b = 20;
  console.log(a); // 10
  console.log(b); // 20</code></pre>
<ul>
<li><code>a</code>와 <code>b</code>는 독립적인 값으로, 하나를 변경해도 다른 변수에는 영향을 미치지 않습니다.</li>
</ul>
</li>
</ul>
</li>
<li><p><strong>참조형 타입</strong> (Reference Types): 객체, 배열, 함수 등으로, 변수에는 <strong>참조(메모리 주소)</strong>가 저장됩니다. 참조형 타입은 <strong>주소</strong>를 저장하고 있기 때문에, 하나의 객체를 여러 변수에 할당하면, 하나를 변경할 경우 나머지 변수들도 영향을 받습니다.</p>
<ul>
<li><p>예시:</p>
<pre><code class="language-jsx">  let obj1 = { name: 'Alice' };
  let obj2 = obj1;
  obj2.name = 'Bob';
  console.log(obj1.name); // 'Bob'
  console.log(obj2.name); // 'Bo</code></pre>
<ul>
<li><code>obj1</code>과 <code>obj2</code>는 같은 객체를 참조하고 있기 때문에, 하나를 수정하면 다른 것도 변경됩니다.</li>
</ul>
</li>
</ul>
</li>
</ul>
<h3 id="2-얕은-복사-shallow-copy">2. <strong>얕은 복사 (Shallow Copy)</strong></h3>
<p>얕은 복사는 객체의 <strong>1단계</strong>만 복사하고, <strong>중첩된 객체</strong>는 여전히 원본 객체와 동일한 <strong>참조</strong>를 공유합니다. 이 말은 중첩된 객체나 배열에 변경이 일어나면 원본 객체에 영향을 미친다는 뜻입니다.</p>
<ul>
<li><p><strong>예시</strong>:</p>
<pre><code class="language-jsx">  const clubInfo = {
    name: 'COW Club',
    members: [
      { name: 'John', fee: 100, part: 'Server', isAttending: true },
      { name: 'Sara', fee: 150, part: 'Web', isAttending: false }
    ]
  };

  const newClubInfo = { ...clubInfo }; // 얕은 복사

  // 중첩된 객체는 여전히 참조를 공유
  newClubInfo.members[0].fee = 200;

  console.log(clubInfo.members[0].fee); // 200
  console.log(newClubInfo.members[0].fee); // 200</code></pre>
<ul>
<li>위 코드에서 <code>newClubInfo</code>는 <code>clubInfo</code>의 얕은 복사본을 만들었지만, <code>members</code> 배열 안의 객체는 여전히 같은 메모리 주소를 참조하고 있습니다. 따라서 <code>newClubInfo.members[0]</code>을 수정하면 <code>clubInfo.members[0]</code>에도 영향을 미칩니다.</li>
</ul>
</li>
</ul>
<h3 id="3-깊은-복사-deep-copy">3. <strong>깊은 복사 (Deep Copy)</strong></h3>
<p>깊은 복사는 객체의 <strong>모든 중첩된 속성</strong>까지 독립적으로 복사하는 방식입니다. 깊은 복사를 사용하면 원본 객체와 복사본 객체가 서로 영향을 미치지 않습니다.</p>
<ul>
<li><p><strong>예시</strong>:</p>
<pre><code class="language-jsx">  const clubInfo = {
    name: 'COW Club',
    members: [
      { name: 'John', fee: 100, part: 'Server', isAttending: true },
      { name: 'Sara', fee: 150, part: 'Web', isAttending: false }
    ]
  };

  const deepCopyClubInfo = JSON.parse(JSON.stringify(clubInfo)); // 깊은 복사

  // 깊은 복사본에서 변경
  deepCopyClubInfo.members[0].fee = 200;

  console.log(clubInfo.members[0].fee); // 100
  console.log(deepCopyClubInfo.members[0].fee); // 200</code></pre>
<ul>
<li><code>JSON.parse(JSON.stringify())</code> 방법은 객체의 깊은 복사를 수행합니다. 이 방식은 객체 내부의 <strong>모든 속성</strong>을 복사하고, 원본 객체와 복사본이 완전히 독립적인 객체로 존재합니다.</li>
</ul>
</li>
</ul>
<h3 id="4-얕은-복사-vs-깊은-복사-어떤-경우에-사용해야-할까">4. <strong>얕은 복사 vs 깊은 복사: 어떤 경우에 사용해야 할까?</strong></h3>
<ul>
<li><strong>얕은 복사</strong>는 <strong>배열이나 객체의 1단계 속성</strong>만 복사할 때 유용합니다. 중첩된 데이터가 없거나, 중첩된 데이터를 수정할 필요가 없는 경우 적합합니다.</li>
<li><strong>깊은 복사</strong>는 객체나 배열이 <strong>중첩된 구조</strong>를 가지고 있고, 중첩된 데이터까지 독립적으로 다뤄야 할 때 사용합니다.</li>
</ul>
<h3 id="5-react에서-불변성immutability과-상태-관리">5. <strong>React에서 불변성(Immutability)과 상태 관리</strong></h3>
<p>React에서는 상태를 직접 수정하지 않고, 새로운 객체를 생성하여 상태를 업데이트해야 합니다. 이 과정에서 <strong>불변성(Immutability)</strong>을 지키는 것이 매우 중요합니다. 불변성을 지키면 React가 상태 변화(예: <code>setState</code>)를 쉽게 추적하고, <strong>효율적인 리렌더링</strong>을 할 수 있습니다.</p>
<ul>
<li><p><strong>불변성을 유지하는 이유</strong>:</p>
<ul>
<li>React는 <strong>참조 비교</strong>를 통해 상태 변경을 감지합니다. 상태를 변경할 때마다 새로운 객체를 생성하면, React는 이전 상태와 새로운 상태를 비교하여 리렌더링을 최적화할 수 있습니다.</li>
<li>불변성을 유지하면 코드가 더 예측 가능하고, 상태 관리가 쉬워집니다.</li>
</ul>
</li>
<li><p><strong>불변성 예시</strong>:</p>
<pre><code class="language-jsx">  const [members, setMembers] = useState([
    { name: 'John', fee: 100, part: 'Server', isAttending: true },
    { name: 'Sara', fee: 150, part: 'Web', isAttending: false }
  ]);

  // 불변성을 유지하면서 상태 업데이트
  const updateFee = (index, newFee) =&gt; {
    const updatedMembers = [...members]; // 얕은 복사
    updatedMembers[index] = { ...updatedMembers[index], fee: newFee }; // 중첩 객체 복사
    setMembers(updatedMembers); // 새로운 배열로 상태 업데이트
  };</code></pre>
</li>
</ul>
<h3 id="퀴즈"><strong>퀴즈</strong></h3>
<ol>
<li><strong>얕은 복사와 깊은 복사에 대한 설명이 올바른 것은?</strong><ol>
<li>얕은 복사는 중첩된 객체까지 독립적으로 복사하고, 깊은 복사는 객체의 1단계만 복사한다.</li>
<li>얕은 복사는 객체의 1단계만 복사하고, 깊은 복사는 중첩된 객체까지 독립적으로 복사한다.</li>
<li>얕은 복사는 객체와 배열을 모두 복사하고, 깊은 복사는 객체만 복사한다.</li>
<li>얕은 복사는 값 타입만 복사하고, 깊은 복사는 참조 타입만 복사한다.</li>
</ol>
</li>
<li><strong>다음 중 깊은 복사를 수행하는 방법으로 올바른 것은?</strong><ol>
<li><code>Object.assign()</code></li>
<li><code>JSON.parse(JSON.stringify())</code></li>
<li><code>...</code> (스프레드 연산자)</li>
<li><code>slice()</code></li>
</ol>
</li>
<li><strong>불변성을 지키지 않았을 때 발생할 수 있는 문제는 무엇인가요?</strong><ol>
<li>React의 리렌더링 최적화가 이루어지지 않거나 예기치 않은 동작이 발생할 수 있다.</li>
<li>상태 업데이트가 불가능해진다.</li>
<li>코드가 더 간결해진다.</li>
<li>상태를 직접 변경할 수 없게 된다.</li>
</ol>
</li>
<li><strong>React에서 상태를 업데이트할 때 얕은 복사를 사용하는 이유는 무엇인가요?</strong><ol>
<li>값 타입만 복사하고, 객체 타입을 무시하기 위해서</li>
<li>중첩된 객체가 변경되지 않기 때문에</li>
<li>상태를 변경할 때 새로운 객체를 생성하여 불변성을 지키기 위해서</li>
<li>React에서 상태를 직접 변경할 수 없기 때문에</li>
</ol>
</li>
<li><strong>스프레드 연산자를 사용한 얕은 복사의 예시로 올바른 코드는?</strong><ol>
<li><code>const newObj = { ...obj };</code></li>
<li><code>const newObj = obj;</code></li>
<li><code>const newObj = Object.assign({}, obj);</code></li>
<li><code>const newObj = JSON.parse(JSON.stringify(obj));</code></li>
</ol>
</li>
</ol>