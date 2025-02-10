<p>뒤로가기 소제목 컴포넌트 개발을 하려는 중이다 </p>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/4cfc5109-7b0b-4779-a398-c332bc79ef67/image.png" />
next 앱라우팅 환경이고 useRouter()를 이용한 동적라우팅 방식을 사용하였다.</p>
<p>개발 후 스토리북에 업로드 하려하자 이런 오류가 난다. 🤮</p>
<h2 id="error-invariant-expected-app-router-to-be-mounted">Error: invariant expected app router to be mounted</h2>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/92805437-bde3-47c4-8313-4530c822e576/image.png" /></p>
<h3 id="공식문서에서-제시한-해결책을-알아보자">공식문서에서 제시한 해결책을 알아보자</h3>
<p><a href="https://storybook.js.org/docs/get-started/frameworks/nextjs">https://storybook.js.org/docs/get-started/frameworks/nextjs</a></p>
<blockquote>
<p>Next.js 프로젝트가 app모든 페이지에 디렉토리를 사용하는 경우(즉, 디렉토리가 없는 경우 ), 파일에서 매개 pages변수를 설정하여 모든 스토리에 적용할 수 있습니다. nextjs.appDirectorytrue.storybook/preview.js|ts
<img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/ae671731-6991-475a-b2e7-477d0cb6b437/image.png" /></p>
</blockquote>
<h4 id="sol-⭐️-appdirectorytrue-를-추가하니-해결됐다"><strong>Sol ⭐️ appDirectory:true 를 추가하니 해결됐다.</strong></h4>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/130e1794-32ee-4c05-be97-af68e3574094/image.png" /></p>
<h3 id="storybook에-tailwind가-적용되지-않은-모습이다-">*<em>Storybook에 tailwind가 적용되지 않은 모습이다. *</em></h3>
<h4 id="sol-⭐️-storybookpreviewts에-밑에-해당-코드를-작성-하면-끝이다-">Sol ⭐️ storybook/preview.ts에 밑에 해당 코드를 작성 하면 끝이다 !!!</h4>
<pre><code>import '../src/app/style/globals.css';</code></pre>