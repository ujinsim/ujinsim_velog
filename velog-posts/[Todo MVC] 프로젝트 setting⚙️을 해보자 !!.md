<h2 id="프로젝트-목적-및-목표">프로젝트 목적 및 목표</h2>
<p>동아리에서 <strong>&quot;우아한 리액트 타입스크립트&quot;</strong> 책을 읽으며  TodoMVC 프로젝트를 진행하기로하였습니다 ! </p>
<p>프로젝트를 시작하기 전, <strong>이 프로젝트를 통해 어떤 걸 얻고 싶은지</strong> 먼저 정리해봤습니다.<br />크게 네 가지 목표 정했는데</p>
<ol>
<li><strong>리액트 훅을 효율적으로 활용하기</strong><ul>
<li>이전에는 훅을 단순히 &quot;이럴 때 쓰는 거구나&quot; 정도로만 이해하고 무조건 사용했던 경향이 있었습니다.</li>
<li>이번에는 <strong>&quot;왜 이 훅을 써야 하는지&quot;</strong>, <strong>&quot;지금 이 구조에서 최선인지&quot;</strong> 등을 고민하며 제대로 활용해보고 싶습니다.</li>
</ul>
</li>
<li><strong>작고 명확한 컴포넌트 설계</strong><ul>
<li>예전에는 컴포넌트를 크고 뭉뚱그려 짰고, “어차피 나 혼자 볼 거니까” 라는 생각도 있었습니다.</li>
<li>하지만 이제는 <strong>하나의 책임만 가지는 컴포넌트 단위</strong>로 분리해, 재사용성과 유지보수성을 고려한 구조를 만들고자 합니다.</li>
</ul>
</li>
<li><strong>명확한 함수/변수 네이밍과 가독성 있는 코드 작성</strong><ul>
<li>함수명이 모호하거나 중복된 역할을 하는 경우가 많았습니다. 예를 들어, handlePrintSomething처럼 명확하지 않은 경우요.</li>
<li>이번엔 <strong>기능을 정확히 설명하는 네이밍</strong>, 그리고 꼭 필요한 주석만을 추가해 직관적인 코드 를 지향합니다.</li>
</ul>
</li>
<li><strong>상태 관리 최적화: useReducer + Context 적용</strong><ul>
<li>props drilling으로 인한 렌더링 이슈를 줄이고, 상태를 더 깔끔하게 관리하기 위해 useReducer와 Context API를 적극 활용해보려 합니다.</li>
</ul>
</li>
</ol>
<blockquote>
<p>추가적으로 프로젝트가 어느 정도 안정화되면<br />Jest와 React Testing Library를 도입해 테스트 코드를 작성하고,<br />변경에 강한 구조를 만들어보고 싶습니다.</p>
</blockquote>
<h2 id="1-프로젝트-구성-선택-왜-vite--tailwind인가">1. 프로젝트 구성 선택: 왜 Vite + Tailwind인가?</h2>
<p>요즘 많이 쓰이는 <strong>Next.js / Vite / Vanilla</strong> 중 고민이 있었지만, 최종적으로는 <strong>Vite + Tailwind</strong> 조합으로 결정했습니다.</p>
<ul>
<li><strong>왜 Vite?</strong><br />SSR보다는 컴포넌트 구조와 훅 사용에 초점을 두고 있어, 빠르고 가벼운 환경의 Vite를 선택했습니다.</li>
<li><strong>왜 Tailwind?</strong><br />스타일링보다는 <strong>기능과 구조 설계에 집중</strong>하고 싶었기 때문에, 빠르게 UI를 구성할 수 있는 Tailwind를 도입했습니다</li>
</ul>
<h3 id="초기-설정-작업">초기 설정 작업</h3>
<ul>
<li>프로젝트 생성: npm create vite@latest</li>
<li>Tailwind 설치 및 설정: npm install -D tailwindcss postcss autoprefixer</li>
</ul>
<hr />
<h2 id="2-코드-스타일-및-품질-도구-설정">2. 코드 스타일 및 품질 도구 설정</h2>
<h3 id="eslint--prettier-설정">Eslint + Prettier 설정</h3>
<p>혼자 하는 프로젝트라고 해도 <strong>코드 가독성</strong>은 결국 미래의 나를 위한 투자라고 생각합니다.<br />나중에 다시 보았을 때 <strong>&quot;이게 뭐였지?&quot;</strong> 싶은 코드를 줄이기 위해,<br />초반부터 <strong>코딩 스타일을 통일하고 일관된 룰을 적용</strong>하기 위해 ESLint와 Prettier를 설정했습니다.</p>
<ul>
<li>코드가 자동으로 정돈되어 시간도 절약되고</li>
<li>협업 시 스타일 충돌 없이 코드 리뷰에 집중할 수 있는 기반도 다질 수 있다고 생각합니다.</li>
</ul>
<h3 id="husky-설정">Husky 설정</h3>
<p>혼자 하는 프로젝트라도 무심코 규칙을 어기면 <strong>프로젝트가 내부적으로 무너질 수 있다</strong>고 생각합니다.<br />그래서 <strong>Git hook을 통해 규칙을 강제</strong>하기 위해 husky를 설정했습니다.</p>
<hr />
<h2 id="3-컴포넌트-설계-및-구조화">3. 컴포넌트 설계 및 구조화</h2>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/2ac76ea2-2653-45df-9d9a-c4abb36b7c14/image.png" /></p>
<p>예전에는 ‘일단 기능부터 만들자’는 생각으로 코드를 짜왔지만,<br />이번 프로젝트에서는 개발에 들어가기 전에 <strong>컴포넌트의 역할을 먼저 설계</strong>하고 구조를 정리해보았습니다.</p>
<h3 id="고민했던-포인트는-다음과-같아요">고민했던 포인트는 다음과 같아요:</h3>
<ul>
<li>어떻게 하면 <strong>각 컴포넌트가 하나의 책임만 가지도록</strong> 나눌 수 있을까?</li>
<li>추후에 재사용하거나 유지보수할 때 <strong>코드의 의도가 쉽게 드러날까?</strong></li>
<li><strong>props 전달</strong>이 너무 깊어지지 않게 설계할 수 있을까?</li>
</ul>
<h3 id="예상되는-컴포넌트-목록">예상되는 컴포넌트 목록:</h3>
<ol>
<li>Button.tsx: 기본 버튼 UI 컴포넌트</li>
<li>Icon.tsx: 아이콘 처리 컴포넌트</li>
<li>TodoInput.tsx: 할 일 추가 입력창</li>
<li>TodoList.tsx: 투두 목록 전체를 보여주는 컴포넌트</li>
<li>TodoItem.tsx: 각각의 투두 항목</li>
<li>TodoFilter.tsx: 전체/완료/미완료 필터링 기능</li>
</ol>
<h3 id="필요한-훅-구성">필요한 훅 구성</h3>
<ol>
<li>useTodoController:<ul>
<li>투두 추가/삭제/수정 같은 <strong>핵심 로직</strong>을 담는 훅</li>
<li>상태 조작은 이 훅에서만 일어나게 분리하는 것이 목표입니다.</li>
</ul>
</li>
<li>useTodoFilter:<ul>
<li>필터에 따라 보여줄 투두를 처리하는 훅</li>
<li>렌더링을 최소화하고, 조건별 필터링을 깔끔하게 분리하고 싶어요.</li>
</ul>
</li>
<li>useEnterClick:<ul>
<li>키보드에서 Enter를 눌렀을 때 동작을 감지하는 <strong>재사용 가능한 유틸성 훅</strong></li>
</ul>
</li>
</ol>
<p>이렇게 미리 컴포넌트와 훅을 나눠 생각해두면,<br />구현을 시작할 때 불필요한 혼란을 줄일 수 있고 <strong>코드의 방향성</strong>도 명확해진다고 느꼈습니다.</p>
<hr />
<h2 id="4-폴더-구조-설계">4. 폴더 구조 설계</h2>
<p>처음에는 FSD 패턴 도입을 고민했지만, 현재 다른 프로젝트에서 적용 중이므로 이번 프로젝트에서는 간단한 기능 중심 구조를 선택했습니다.</p>
<pre><code>├── components
│   └── ... (UI 컴포넌트)
├── hooks
│   └── useTodoController.ts
├── contents
│   └── ... (상수, config 등)
├── assets
│   └── ... (아이콘, 이미지)
├── App.tsx
├── main.tsx</code></pre><hr />
<h2 id="마무리">마무리</h2>
<p>Todo라는 작은 프로젝트이지만, 유지보수성과 설계 원칙을 잘 적용한다면 큰 프로젝트에서도 많은 도움이 될 것이라 믿고, 약간은 과할 수도 있는 목표를 가지고 출발.........</p>
<p>최대한 얻어갈 수 있도록 블로그에 하나하나 남겨보겠습니다 !!</p>