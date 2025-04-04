<h3 id="select-컴포넌트를-리팩토링하기-전-고민">Select 컴포넌트를 리팩토링하기 전 고민</h3>
<pre><code class="language-tsx">'use client';
import React, { useState } from 'react';
import { Icon } from '../Icon';

type Props = {
  contents: string[];
  size?: 'md' | 'lg';
  placeholder?: string;
};

export function Select({ contents, size, placeholder }: Props) {
  const [selectedContent, setSelectedContent] = useState(placeholder ? placeholder : contents[0]);
  const [openSelect, setOpenSelect] = useState(false);

  return (
    &lt;div
      className={`${openSelect ? 'rounded-lg' : 'rounded-lg border border-gray-100'} text-b'${
        size === 'md' ? 'md:min-w-26 w-fit min-w-24' : 'md:min-w-66 w-60'
      } cursor-pointer border border-gray-200 bg-white text-start text-base font-semibold text-gray-400 md:text-lg`}
    &gt;
      &lt;div
        className={`${
          openSelect
            ? 'border-b-2 border-gray-100 hover:rounded-lg hover:rounded-b-none'
            : 'border-0'
        } flex justify-between ${
          size === 'md' ? 'items-center py-1 pl-3 pr-2 text-sm' : 'px-5 py-2 md:py-3'
        } hover:rounded-md hover:bg-gray-50`}
        onClick={() =&gt; setOpenSelect(!openSelect)}
      &gt;
        {selectedContent}

        &lt;Icon
          name=&quot;arrowDown&quot;
          className={`transform transition-transform duration-300 ${
            openSelect ? 'rotate-180' : ''
          } ${size === 'md' ? 'w-5' : ''} }`}
        /&gt;
      &lt;/div&gt;

      {openSelect &amp;&amp; (
        &lt;div className=&quot;flex flex-col&quot;&gt;
          {contents.map((item, key) =&gt; (
            &lt;div
              onClick={() =&gt; {
                setSelectedContent(contents[key]);
                setOpenSelect(!openSelect);
              }}
              className={`cursor-pointer border-gray-100 ${
                size === 'md' ? 'px-3 py-1 text-sm' : 'px-5 py-2 md:py-3'
              } last:border-none hover:bg-gray-50 hover:last:rounded-b-md`}
              key={key}
            &gt;
              {item}
            &lt;/div&gt;
          ))}
        &lt;/div&gt;
      )}
    &lt;/div&gt;
  );
}</code></pre>
<h3 id="무언가-한덩이로-크게-span-stylecolor-red묶여져span있는-select-코드-를-바라보며-컴포넌트에-기준에-대해-고민했습니다">무언가 한덩이로 크게 <span style="color: red;">묶여져</span>있는 Select 코드.. 를 바라보며 컴포넌트에 기준에 대해 고민했습니다</h3>
<p><code>Select</code> 뿐만 아니라 <code>GroupingComponent</code>도 있는데 <code>groupName</code>만 추가된 형태임에도 새로운 컴포넌트를 만들어야됐습니다 <img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/67c06790-141c-4651-8288-a802d9a70b52/image.png" /></p>
<hr />
<h2 id="리팩토링-기준을-알게-된-계기">리팩토링 기준을 알게 된 계기</h2>
<p>Effective Component 강의 <strong>「지속 가능한 성장과 컴포넌트」</strong>를 보고 다음과 같은 인사이트를 얻었습니다.</p>
<h3 id="컴포넌트-리팩토링-기준">컴포넌트 리팩토링 기준</h3>
<h3 id="1-headless-기반으로-추상화하기">1. Headless 기반으로 추상화하기</h3>
<p>변하는 것과 변하지 않는 것을 나누자.<br />데이터는 같지만 UI는 달라질 수 있다면 <strong>UI와 데이터를 분리</strong>하자.</p>
<ul>
<li><strong>예: 달력</strong><ul>
<li>데이터는 훅으로 관리 (예: <code>useCalendar</code>)</li>
<li>UI는 별도 분리 → Headless 모듈화</li>
</ul>
</li>
</ul>
<blockquote>
<p>💡 변화하는 부분은 <code>훅</code>, 보여지는 부분은 <code>컴포넌트</code>로 분리</p>
</blockquote>
<hr />
<h3 id="2-한-가지-역할만-하기-→-composition-pattern">2. 한 가지 역할만 하기 → Composition Pattern</h3>
<p>하나의 역할을 가진 컴포넌트를 여러 개 조합해서 구성하자.</p>
<ul>
<li><strong>예: Select</strong><ul>
<li>선택된 값을 보여주는 <code>Trigger</code></li>
<li>드롭다운 메뉴인 <code>Menu</code></li>
<li>실제 아이템 목록인 <code>Item</code></li>
</ul>
</li>
</ul>
<p>→ 이벤트 흐름: <code>onClick</code> → <code>onChange</code>
<img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/8dd51d0d-9432-46b1-8b43-b3d535538197/image.png" /></p>
<blockquote>
<p> 이렇게 하면 각 컴포넌트는 서로를 몰라도 되고, 독립적으로 재사용 가능</p>
</blockquote>
<h3 id="3-도메인-분리하기">3. 도메인 분리하기</h3>
<p>데이터 주입받는 방식도 컴포넌트처럼 분리해야 한다.</p>
<ul>
<li>도메인을 알지 못하는 컴포넌트  </li>
<li>도메인을 포함하는 컴포넌트</li>
</ul>
<p>이 둘을 분리하면 <strong>외부 의존성 없이도 관리하기 쉬움</strong>
<img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/24764cb9-4323-4ed4-9ed6-cc10d25b9eb3/image.png" /></p>
<p>🔍 예: 인터페이스 정의부터 고민하기</p>
<hr />
<h2 id="해당-영상을-보고-select-컴포넌트에-어떤-문제가-존재했는지-알-수있었습니다-">해당 영상을 보고 Select 컴포넌트에 어떤 문제가 존재했는지 알 수있었습니다 ...</h2>
<h3 id="리팩토링-전-느낀-점">리팩토링 전 느낀 점</h3>
<blockquote>
<p>만약 Select에 MultiSelect나 검색 기능이 추가된다면,<br />어디서부터 수정해야 할지 감이 잡히지 않았습니다.</p>
</blockquote>
<p>구조가 <strong>확장에 취약</strong>하다는 점을 강하게 느꼈습니다.</p>
<p>📌 <strong>변경에 유연한 구조</strong>가 진짜 ‘좋은 컴포넌트’다!</p>
<hr />
<h3 id="리팩토링-구조-예시">리팩토링 구조 예시</h3>
<p>영상에서 dropdown컴포넌트를 다음과같이 구조화한 예시를 보여주었습니다 </p>
<ul>
<li><code>DropdownTrigger</code> : 열고 닫는 역할  </li>
<li><code>Menu</code> : 리스트의 wrapper  </li>
<li><code>Item</code> : 실제 선택할 항목  </li>
<li>이벤트 흐름은 <code>onClick</code> → <code>onChange</code></li>
</ul>
<p>변경된 컴포넌트는 <strong>서로의 존재를 몰라도 동작</strong>합니다.
그리고 이러한 구조는 <strong>확장성</strong>과 <strong>재사용성</strong> <strong>유지보수성</strong>을 높입니다 </p>
<hr />
<h2 id="영상에서는-컴포넌를-짜기-위한-2가지의-흐름을-알려줍니다">영상에서는 컴포넌를 짜기 위한 2가지의 흐름을 알려줍니다</h2>
<h3 id="1-인터페이스-먼저-고민하기">1. 인터페이스 먼저 고민하기</h3>
<blockquote>
<p>“이 컴포넌트의 의도는 무엇인가?”<br />“기능보다 표현 방식이 더 중요할 수도 있다.”</p>
</blockquote>
<hr />
<h3 id="2-컴포넌트를-분리하는-이유를-항상-생각하기">2. 컴포넌트를 분리하는 이유를 항상 생각하기</h3>
<ul>
<li>정말 복잡도를 낮추는가?  </li>
<li>정말 재사용 가능한가?</li>
</ul>
<blockquote>
<p>💬 “리팩토링은 기능을 더하는 것이 아니라, 이해를 더하는 과정이다.”</p>
</blockquote>
<hr />
<h4 id="이-영상을-통해서-얻은-점">이 영상을 통해서 얻은 점</h4>
<ol>
<li>컴포넌트를 짜는 방식 </li>
<li>내코드가 개구리다 🐸<h2 id="새롭게-select-컴포넌트-리팩토링-방향을-정해보았습니다">새롭게 Select 컴포넌트 리팩토링 방향을 정해보았습니다</h2>
</li>
</ol>
<h3 id="폴더구조-">*<em>폴더구조 *</em></h3>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/0634a504-f586-4e2b-82b1-3a6b5822918b/image.png" /></p>
<h2 id="상태-관리-흐름">상태 관리 흐름</h2>
<p><code>useSelectMain</code>: 선택 상태 및 열림 여부 등 4가지 상태를 지역 상태로 관리합니다</p>
<blockquote>
<p><code>selected</code>: 현재 선택된 항목
<code>isOpen</code>: 드롭다운 열림 여부
<code>setIsOpen</code>: 드롭다운 열고 닫기 제어
<code>handleSelect</code>: 옵션 선택 핸들러</p>
</blockquote>
<p><code>SelectMain</code>: UI 뼈대를 구성하고, context로 하위 컴포넌트에 상태 전달</p>
<p><code>SelectContext.Provider</code>: context를 통해 하위 컴포넌트로 상태 전파</p>
<p><code>useSelectContext</code>: 하위 컴포넌트들이 context 값을 받아 사용</p>
<h3 id="그림으로-표현하면-이렇습니다-⬇️"><strong>그림으로 표현하면 이렇습니다 ⬇️</strong></h3>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/f59a20d1-467e-42c8-b587-063cdf265876/image.png" /></p>
<p>*<em>구조로 나타내면 이렇게 표현할 수 있습니다 *</em></p>
<pre><code>[useSelectMain]         ← 지역 상태 생성
      │
      ▼
[SelectMain]            ← 상태를 context로 감싸서 하위에 전달
      │
      ▼
[SelectContext.Provider]    ← context 전파
      │
      ▼
[useSelectContext()]    ← 하위 컴포넌트가 context 값 사용
      ├── Option        → onSelect, size
      └── OptionList    → size
      └── TriggerButton    → size,onSelect, selected 
</code></pre><p>그리고 마지막으로는 만들어진 컴포넌트를 조립해서 
<img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/357091db-4af3-4784-b160-c1d5f0481c9c/image.png" />
각각의 컴포넌트처럼 사용할 수 있도록 만들었습니다 </p>
<h1 id="느낀점">느낀점</h1>
<p>이번 리팩토링을 통해,
그동안 <strong>컴포넌트를 나눈다 = 파일/폴더 단위로 나눈다</strong> 라고만 생각했던 저 자신을 돌아보게 되었습니다.</p>
<p>컴포넌트는 단순히 폴더에 따라 분리하는 것이 아니라,
그 안에서도 역할(기능 vs UI) 기반으로 더 깊이 있게 나눌 수 있고,
필요한 기능을 조립해서 사용하는 <strong>‘블록형 사고방식</strong>’이 가능하다는 것을 새롭게 체감했습니다.</p>
<p>하나의 Select 컴포넌트 안에서도</p>
<ul>
<li>상태 관리 (useSelectMain)</li>
<li>전역 상태 공유 (SelectContext)</li>
<li>UI 단위 분리 (Trigger, Option, OptionList, Group) 로 나눌 수 있다는 점에서 역할 중심의 설계 사고방식이 생겼습니다.</li>
</ul>
<p>단순히 쓰기 좋게 만드는 게 아니라,
나중에 내가 다시 볼 때도 유용한 구조로 만드는 것이 진짜 설계라는 걸 느꼈습니다.</p>
<p>*<em>잘 만든 컴포넌트는 나에게 다시 되돌아온다 *</em> 라는 생각이 가장 들었고 
이 구조가 앞으로 제 프로젝트에서도 재사용될 수 있다는 생각에 설계의 중요성을 더 실감하게 됐습니다.</p>
<p>이번 경험을 바탕으로 </p>
<ul>
<li>기능과 UI를 나누는 시선</li>
<li>context와 상태를 언제, 어떻게 공유할지에 대한 판단</li>
<li>조립 가능한 설계 패턴을 염두에 둔 컴포넌트 작성</li>
</ul>
<p>에 더 깊이 고민하며 성장해 나가고 싶습니다.</p>
<h3 id="참고">참고</h3>
<p><a href="https://www.youtube.com/watch?v=fR8tsJ2r7Eg&amp;t=46s">https://www.youtube.com/watch?v=fR8tsJ2r7Eg&amp;t=46s</a> 
<a href="https://velog.io/@aeong98/%EC%BB%B4%ED%8C%8C%EC%9A%B4%EB%93%9C-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%ED%8C%A8%ED%84%B4%EC%9C%BC%EB%A1%9C-Select-%EB%A7%8C%EB%93%A4%EA%B8%B0">https://velog.io/@aeong98/%EC%BB%B4%ED%8C%8C%EC%9A%B4%EB%93%9C-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%ED%8C%A8%ED%84%B4%EC%9C%BC%EB%A1%9C-Select-%EB%A7%8C%EB%93%A4%EA%B8%B0</a>
<a href="https://ui.shadcn.com/docs/components/dropdown-menu">https://ui.shadcn.com/docs/components/dropdown-menu</a></p>