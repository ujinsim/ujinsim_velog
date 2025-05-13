<h2 id="git이란">Git이란?</h2>
<ul>
<li><strong>버전 관리 시스템 (Version Control System, VCS)</strong></li>
<li>파일이나 파일 집합의 변경사항을 시간에 따라 기록하는 시스템</li>
<li>특정 시점의 버전을 다시 불러올 수 있음</li>
</ul>
<h2 id="버전-관리-시스템vcs의-주요-기능">버전 관리 시스템(VCS)의 주요 기능</h2>
<ul>
<li>선택한 파일을 <strong>이전 상태</strong>로 되돌리기</li>
<li><strong>전체 프로젝트</strong>를 과거의 특정 시점으로 복원</li>
<li>시간에 따른 <strong>변경사항 비교</strong></li>
<li><strong>누가</strong>, <strong>언제</strong>, <strong>어떤 변경</strong>을 했는지 추적</li>
<li>문제를 일으킨 <strong>수정자</strong>와 <strong>시점</strong> 파악</li>
</ul>
<h2 id="vcs-사용의-장점">VCS 사용의 장점</h2>
<ul>
<li>실수로 파일을 망치거나 삭제해도 <strong>쉽게 복구 가능</strong></li>
<li><strong>적은 오버헤드</strong>(자원 소모 없이)로 강력한 관리 기능 제공</li>
</ul>
<h1 id="local-version-control-systems">Local Version Control Systems</h1>
<ul>
<li><strong>로컬 버전 관리 시스템</strong></li>
<li>파일의 변경 이력을 <strong>로컬 컴퓨터</strong>에만 저장</li>
<li>보통 파일을 <strong>복사해서 다른 폴더</strong>에 백업하는 방식으로 운영</li>
<li>단점:<ul>
<li>실수로 원본이나 백업을 덮어쓰거나 삭제하면 복구가 힘듦</li>
<li>협업에는 부적합</li>
</ul>
</li>
</ul>
<hr />
<h1 id="distributed-version-control-system-dvcs">Distributed Version Control System (DVCS)</h1>
<ul>
<li><strong>분산 버전 관리 시스템</strong></li>
<li>변경 이력을 <strong>로컬에도 저장</strong>하고, <strong>서버에도 저장</strong>함</li>
<li>각각의 사용자가 전체 프로젝트의 <strong>풀(Full) 히스토리</strong>를 가지고 있음</li>
<li>서버에 문제가 생겨도, <strong>로컬 저장소</strong>에서 복구 가능</li>
<li>협업에 매우 강력</li>
</ul>
<hr />
<h1 id="snapshot">Snapshot</h1>
<ul>
<li>Git은 <strong>&quot;스냅샷(snapshot)&quot;</strong> 방식으로 버전 관리함</li>
<li><strong>&quot;변경사항만 저장&quot;</strong> 하는 방식이 아니라<br />→ 파일 전체를 <strong>현재 시점의 상태(스냅샷)</strong> 로 저장</li>
<li>하지만 똑같은 파일은 다시 저장하지 않고, <strong>링크(link)</strong> 로 가볍게 관리</li>
</ul>
<h2 id="the-three-states"> The Three States</h2>
<p>[##<em>Image|kage@dSi6K0/btsNDDI5xKG/NRBdEgDyKZkQYTSYVgR9Q0/img.png|CDM|1.3|{&quot;originWidth&quot;:1112,&quot;originHeight&quot;:600,&quot;style&quot;:&quot;alignCenter&quot;,&quot;caption&quot;:&quot;Git Three States&quot;,&quot;filename&quot;:&quot;스크린샷 2025-04-28 오후 3.24.55.png&quot;}</em>##]</p>
<ul>
<li><strong>Checkout</strong>: 원하는 브랜치나 커밋으로 이동하는 것.</li>
<li><strong>Stage Fixed</strong>: 수정한 파일을 커밋 준비(Staging Area) 상태로 올리는 것.</li>
<li><strong>Commit</strong>: 준비된 변경사항을 저장소에 기록하는 것.</li>
<li><strong>Working Directory</strong>: 내가 직접 수정하고 작업하는 실제 폴더.</li>
<li><strong>Staging Area</strong>: 커밋할 파일을 잠깐 모아두는 대기 구역.</li>
<li><strong>.git directory</strong>: Git이 버전 관리에 필요한 모든 기록을 저장하는 숨겨진 폴더</li>
</ul>
<h4 id="파일의-3가지-상태">파일의 3가지 상태 </h4>
<ul>
<li><strong>Modified (수정됨)</strong><br />→ 파일을 수정했지만 아직 Git에 저장(commit)하지 않은 상태</li>
<li><strong>Staged (스테이징됨)</strong><br />→ 수정된 파일을 다음 커밋에 포함되도록 준비해둔 상태<br />→ git add 한 파일</li>
<li><strong>Committed (커밋됨)</strong><br />→ 수정사항이 <strong>로컬 Git 데이터베이스</strong>(.git 디렉토리)에 저장된 상태</li>
</ul>
<h3 id="프로젝트의-3개-구역">프로젝트의 3개 구역 </h3>
<ul>
<li><strong>Working Tree (작업 디렉토리)</strong><br />→ Git으로 관리 중인 프로젝트의 실제 파일들이 있는 곳<br />→ 수정하는 곳</li>
<li><strong>Staging Area (스테이징 영역)</strong><br />→ 다음 커밋에 포함할 파일들의 목록을 임시 저장하는 곳<br />→ Git에서는 &quot;Index&quot;라고도 부름</li>
<li><strong>Git Directory (.git 디렉토리)</strong><br />→ Git이 모든 버전 기록, 메타데이터를 저장하는 핵심 폴더<br />→ Git 프로젝트의 진짜 본체</li>
</ul>
<h3 id="기본-작업-흐름">기본 작업 흐름 </h3>
<ol>
<li><strong>Working Tree</strong>에서 파일을 수정한다.</li>
<li>수정한 파일을 골라서 <strong>Staging Area</strong>에 추가한다. (git add)</li>
<li>스테이징된 파일을 <strong>Commit</strong>해서 <strong>Git Directory</strong>에 저장한다. (git commit)</li>
</ol>
<blockquote>
<p><strong>요약 흐름</strong><br />수정 → 스테이징 → 커밋</p>
</blockquote>
<h2 id="git-초기설정하기-🎉">Git 초기설정하기 🎉</h2>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/16112a15-da2a-451a-942d-10970a70c8f6/image.png" /></p>
<h2 id="깃-레포지토리를-만드는-방법">깃 레포지토리를 만드는 방법</h2>
<p>1. 기존 폴더를 Git 저장소로 &quot;변환&quot; 하기 </p>
<p>2. 다른 곳에 있는 Git 저장소를 &quot;복제&quot; 하기 </p>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/6ff88c62-5d9d-49e3-90f3-8ac65f9cb1b5/image.png" /></p>
<p>1. Untracked </p>
<p>2. Unmodified</p>
<p>3. Modified</p>
<p>4. Staged </p>
<p>- git Add</p>
<p>- Remove the file</p>
<p>- Edit the file</p>
<p>- Stage the file</p>
<p>- Commit </p>
<h2 id="ignoring-files">Ignoring Files </h2>
<p>버전컨트롤에서 무시하고싶은 ☠️ 파일 </p>
<p>.gitgnore &lt;&lt; 파일을 만들고 진행한다 ㅋㅋ </p>
<ul>
<li>.gitignore 파일에 <strong>무시할 파일/패턴</strong>을 적으면 됨.</li>
<li>.gitignore은 보통 프로젝트 루트에 만들고,<br />→ 내용은 전체 프로젝트에 적용됨.</li>
<li>필요하면 하위 폴더에도 추가할 수 있다.</li>
</ul>
<h3 id="gitignore-작성-규칙">.gitignore 작성 규칙 </h3>
<table>
<thead>
<tr>
<th>빈 줄, #로 시작하는 줄</th>
<th>무시됨 (주석)</th>
</tr>
</thead>
<tbody><tr>
<td>* (*.log )</td>
<td>아무 문자 0개 이상</td>
</tr>
<tr>
<td>? (file?.log &gt; file+아무글자1개 + .log)</td>
<td>아무 문자 1개</td>
</tr>
<tr>
<td>[abc]</td>
<td>a, b, c 중 하나</td>
</tr>
<tr>
<td>[0-9]</td>
<td>0~9 중 하나</td>
</tr>
<tr>
<td>/로 시작</td>
<td>루트 기준 (재귀 방지)</td>
</tr>
<tr>
<td>/로 끝</td>
<td>디렉터리만 무시</td>
</tr>
<tr>
<td>!패턴</td>
<td>특정 파일/패턴은 무시 안 함 (예외 만들기)</td>
</tr>
<tr>
<td>**</td>
<td>중첩 폴더(디렉터리들)까지 매칭</td>
</tr>
</tbody></table>
<p>어떤게 Staged됐는지 확인하려면 </p>
<p>`git diff --staged`</p>
<p>git diff -&gt; staged가 안된것만 보여줌 </p>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/977c334a-c17c-48c2-83d4-99f210579517/image.png" /></p>
<h2 id="git-log">git log </h2>
<p>맨 위가 최신 </p>
<p>자주쓰는 option</p>
<p>- p : 어디 파일에서, 어떤 function에서 변경사항이 발생했는지 commit 단위로 보여줌</p>
<p>-- patch : </p>
<p>--stat : 어디파일에서 뭐가 삭제됐는지</p>
<p>--pretty : 어떤 형식으로 로그를 생성할지를 지정해줌 </p>
<ul>
<li>oneline, short, full, funller valus show the output information </li>
</ul>
<p>git log --pretty=format:&quot;%h - %an, %ar : %s&quot;</p>
<p>👇외우거라</p>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/8a67cd8d-83ae-43bf-8596-6f68ce4ce226/image.png" /></p>
<h3 id="authorand-and-committer">authorand and committer</h3>
<p>authorand : 수정하는 사람</p>
<p>committer : 커밋하는 사람 </p>
<p>git log --graph</p>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/a79b348a-9129-410e-87cf-06087817b960/image.png" /></p>