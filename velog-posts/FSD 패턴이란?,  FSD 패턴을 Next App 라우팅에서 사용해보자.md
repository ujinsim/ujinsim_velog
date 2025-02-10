<p>프로젝트 마이그래이션을 위해 FSD패턴을 구조로 잡고가자고 제안을 주셨습니다 </p>
<h2 id="fsd-패턴fsfp-feature-sliced-design-pattern-이란">FSD 패턴(FSFP, Feature-Sliced Design Pattern) 이란?</h2>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/a97453d3-dd61-4659-bd8f-2412400c1d42/image.png" /></p>
<pre><code>FSD 패턴은 프론트엔드 프로젝트를 계층적으로 구성하여 유지보수성을 높이는 아키텍처 패턴입니다.
기존 폴더 구조보다 역할별 분리를 강조하며, 프로젝트가 커질수록 폴더 구조가 명확해지고, 코드 간 의존성이 줄어드는 장점이 있습니다.</code></pre><h3 id="fsd-패턴의-주요-폴더-구조">FSD 패턴의 주요 폴더 구조</h3>
<blockquote>
<p><strong>app</strong>
전역 설정 담당 (_app , _document 등 로직이 초기화 되는 곳)
Provider, Router, client 같은 컴포넌트들이 slice 된다.
<strong>processes (선택)</strong>
회원가입 등과 같이 여러 단계로 이루어진 프로세스를 처리한다.
특정한 상태를 기반으로 여러 화면 또는 로직이 포함된 작업
<strong>processes/</strong>
├── <strong>registration/</strong>
│   ├──** model/ ** // 회원가입 상태 및 로직 관리
│   ├── <strong>ui/</strong>  // 회원가입 관련 화면 및 컴포넌트
│   └── <strong>index.ts</strong>
└── <strong>checkout/</strong>
<strong>pages</strong> → 메인페이지 URL 라우트에 매핑되는 페이지가 포함된다.
<strong>widgets</strong> (선택) → 헤더, 내용별 컨텐츠
여러 feature를 결합해 특정 화면 단위의 구성 요소를 정의, 페이지에 사용되는 독립적인 UI 컴포넌트
<strong>features</strong> (선택) → 폼, 액션버튼
행동, 비즈니스 가치를 전달하는 사용자 시나리오와 기능
<strong>entities</strong> →
주요 데이터 모델과 그와 관련된 상태/로직, 비즈니스 엔티티
<strong>shared</strong> →공유 컴포넌트</p>
</blockquote>
<p>이제 패턴에 대해 어느정도 이해를 했으니 Next.js에 적용하려고 했다 
근데 .. </p>
<h2 id="nextjs-app-router와-fsd-패턴-충돌-문제">Next.js App Router와 FSD 패턴 충돌 문제</h2>
<pre><code>Next.js 13.4부터 App Router 방식이 도입되면서 
기존 pages/ 라우팅 방식과 app/ 폴더 방식이 혼용될 수 있는 문제가 발생합니다.</code></pre><p>FSD 패턴에는 pages/ 폴더가 존재하는데, Next.js에도 pages/가 존재하기 때문에 폴더 충돌 가능성이 발생한다 
Next.js App Router 방식에서는 app/ 폴더를 루트로 두어야 하지만, FSD 패턴에서도 app/ 폴더가 존재할 수 있어 혼동 가능한 것이다 </p>
<h3 id="해결-방법">해결 방법</h3>
<p><a href="https://feature-sliced.design/docs/guides/tech/with-nextjs#app-router">next.js에서 FSD패턴 가이드</a>
공식문서를 통해서 해결 방법을 찾아보았다 </p>
<p> Next.js App Router를 루트로 두고 FSD 페이지 구조를 src/ 폴더 내부에 배치</p>
<p>app/ 폴더를 루트에 유지하고,
pages/ 폴더를 FSD 내부에서 관리하는 방식이 권장된다 </p>
<p>📦 프로젝트 루트
 ├── 📂 app/  (Next.js App Router 전용 폴더)
 │    ├── 📂 src/
 │    │    ├── 📂 pages/   (🚀 FSD 패턴에 맞춘 페이지 구조)
 │    │    ├── 📂 processes/
 │    │    ├── 📂 widgets/
 │    │    ├── 📂 features/
 │    │    ├── 📂 entities/
 │    │    ├── 📂 shared/
 │    │    ├── index.ts
 │    ├── layout.tsx  (Next.js의 App Router 전용)
 │    ├── page.tsx  (Next.js의 기본 페이지)
 │    └── global.css
 ├── 📂 public/
 ├── 📂 styles/
 ├── 📂 components/
 ├── next.config.js
 ├── tsconfig.json
 └── package.json</p>
<p> 해당 해결책으로 FSD패턴을 적용하기로 했습니다 </p>
<h3 id="그래서-우리는-기존-프로젝트의-마이그래이션으로-fsd패턴을-쓰려고-했는가">그래서 우리는 기존 프로젝트의 마이그래이션으로 FSD패턴을 쓰려고 했는가</h3>
<ol>
<li>프로젝트 유지보수가 쉬워짐
기존 Next.js의 라우팅 방식 (pages/ 중심)에서는 컴포넌트의 역할이 모호해질 수 있음.
FSD 패턴을 적용하면 각 기능과 데이터 모델이 명확하게 분리되어 유지보수가 편리함.</li>
</ol>
<p>-&gt; 기존 구조에서는 하나의 작업에서 여러 폴더 계층을 오가야되는 상황 해결 </p>
<ol start="2">
<li><p>페이지별 독립적인 관리 가능
pages/ 폴더가 혼합되지 않고, 각 기능을 독립적인 feature로 관리할 수 있음.
하나의 페이지에서 필요한 기능만 로드하여 모듈화가 쉬워짐.</p>
</li>
<li><p>의존성 관리가 쉬워짐
상위 폴더에서 하위 폴더만 import 가능하도록 규칙을 강제하면, 순환 의존성 문제를 예방할 수 있음.
ESLint 규칙을 적용하면 코드 규칙을 자동으로 검사할 수 있으니 사용해보면 좋을 듯 !! </p>
</li>
</ol>
<p>ESLint FSD 규칙 적용 예시</p>
<pre><code>&quot;overrides&quot;: [
  {
    &quot;files&quot;: [&quot;src/**/*.ts&quot;, &quot;src/**/*.tsx&quot;],
    &quot;rules&quot;: {
      &quot;fsd-layer-imports/path-checker&quot;: &quot;error&quot;,
      &quot;fsd-layer-imports/public-api-imports&quot;: &quot;error&quot;
    }
  }
]</code></pre><h3 id="우려되는-점--해결책">우려되는 점 &amp; 해결책</h3>
<ol>
<li>우선순위가 꼬일 가능성
문제: 프로젝트에서 어떤 폴더를 기준으로 관리해야 할지 헷갈릴 수 있음.</li>
</ol>
<p>-&gt; app/을 Next.js의 루트로 유지하되, FSD 패턴을 src/ 내부에서 유지하여 혼동을 방지.</p>
<ol start="2">
<li>라우팅이 꼬일 가능성
문제: Next.js의 App Router와 FSD의 pages/ 구조가 섞이면서 라우팅 우선순위 문제가 발생할 수 있음.</li>
</ol>
<p>-&gt; app/ 라우터 내부에서 FSD 패턴을 적용하고,
pages/ 폴더를 루트가 아닌 src/ 내부에 배치하여 충돌을 방지.</p>
<h4 id="위의-이유로-이번-마이그레이션에서는-fsd패턴을-적용하기로-하였다-">위의 이유로 이번 마이그레이션에서는 FSD패턴을 적용하기로 하였다 ~</h4>