<h1 id="google-cloud-shell에서-명령어-학습하기"><strong>Google Cloud Shell에서 명령어 학습하기</strong></h1>
<p>Google Cloud Shell은 Google Cloud Platform(GCP)에서 제공하는 웹 기반의 터미널 환경으로, Linux 명령어를 사용하여 파일 및 디렉토리를 관리할 수 있습니다. 아래에서 주요 명령어들을 설명합니다.</p>
<hr />
<h2 id="1-현재-작업-디렉토리-확인-pwd-명령어"><strong>1. 현재 작업 디렉토리 확인 (<code>pwd</code> 명령어)</strong></h2>
<pre><code class="language-bash">pwd</code></pre>
<ul>
<li>현재 사용자가 위치한 디렉토리(경로)를 출력합니다.</li>
<li><code>pwd</code>는 &quot;print working directory&quot;의 약자입니다.</li>
</ul>
<hr />
<h2 id="2-디렉토리-및-파일-목록-확인-ls-명령어"><strong>2. 디렉토리 및 파일 목록 확인 (<code>ls</code> 명령어)</strong></h2>
<pre><code class="language-bash">ls</code></pre>
<ul>
<li>현재 디렉토리에 있는 파일과 폴더를 나열합니다.</li>
<li>옵션을 추가하면 더 많은 정보를 볼 수 있습니다.<pre><code class="language-bash">ls -l  # 자세한 정보 표시 (파일 권한, 소유자, 크기, 수정 날짜)
ls -a  # 숨김 파일까지 표시
ls -lh # 사람이 읽기 쉬운 크기 단위로 표시</code></pre>
</li>
</ul>
<hr />
<h2 id="3-디렉토리-이동-cd-명령어"><strong>3. 디렉토리 이동 (<code>cd</code> 명령어)</strong></h2>
<pre><code class="language-bash">cd [디렉토리명]</code></pre>
<ul>
<li>특정 디렉토리로 이동합니다.</li>
<li>예시:<pre><code class="language-bash">cd /home/user  # 절대 경로 이동
cd Documents   # 상대 경로 이동
cd ..          # 상위 디렉토리로 이동
cd ~           # 홈 디렉토리로 이동
cd -           # 이전 디렉토리로 이동</code></pre>
</li>
</ul>
<hr />
<h2 id="4-절대-경로absolute-paths와-상대-경로relative-paths"><strong>4. 절대 경로(Absolute Paths)와 상대 경로(Relative Paths)</strong></h2>
<ul>
<li><strong>절대 경로</strong>: 루트(<code>/</code>)에서부터 시작하는 전체 경로.<pre><code class="language-bash">cd /home/user/Documents</code></pre>
</li>
<li><strong>상대 경로</strong>: 현재 위치를 기준으로 이동하는 경로.<pre><code class="language-bash">cd Documents  # 현재 디렉토리에 있는 &quot;Documents&quot;로 이동
cd ../        # 상위 폴더로 이동</code></pre>
</li>
</ul>
<hr />
<h2 id="5-새-디렉토리-생성-mkdir-명령어"><strong>5. 새 디렉토리 생성 (<code>mkdir</code> 명령어)</strong></h2>
<pre><code class="language-bash">mkdir [디렉토리명]</code></pre>
<ul>
<li>새 폴더를 생성합니다.</li>
<li>여러 개의 폴더를 한 번에 생성할 수도 있습니다.<pre><code class="language-bash">mkdir new_folder
mkdir folder1 folder2 folder3  # 여러 개 생성
mkdir -p parent/child          # 중첩된 폴더 생성</code></pre>
</li>
</ul>
<hr />
<h2 id="6-옵션options"><strong>6. 옵션(Options)</strong></h2>
<ul>
<li>명령어에 옵션을 추가하면 기능이 확장됩니다.</li>
<li>예시:<pre><code class="language-bash">ls -l    # 상세 정보 출력
rm -r    # 디렉토리와 내부 파일 삭제
cp -i    # 덮어쓰기 전에 확인</code></pre>
</li>
</ul>
<hr />
<h2 id="7-특수-문자escaping"><strong>7. 특수 문자(escaping)</strong></h2>
<ul>
<li>공백이나 특수 문자가 포함된 파일명을 사용할 때 <code>\</code>를 이용해 이스케이프 처리합니다.<pre><code class="language-bash">touch my\ file.txt  # &quot;my file.txt&quot; 파일 생성</code></pre>
</li>
</ul>
<hr />
<h2 id="8-리디렉션-및-파일-출력--cat-echo-명령어"><strong>8. 리디렉션 및 파일 출력 (<code>&gt;</code>, <code>cat</code>, <code>echo</code> 명령어)</strong></h2>
<ul>
<li><code>&gt;</code> : 출력 결과를 파일에 저장.<pre><code class="language-bash">echo &quot;Hello, World!&quot; &gt; hello.txt  # hello.txt에 저장</code></pre>
</li>
<li><code>&gt;&gt;</code> : 기존 파일에 내용을 추가.<pre><code class="language-bash">echo &quot;추가된 내용&quot; &gt;&gt; hello.txt</code></pre>
</li>
<li><code>cat</code> : 파일 내용 출력.<pre><code class="language-bash">cat hello.txt</code></pre>
</li>
</ul>
<hr />
<h2 id="9-와일드카드---사용하기"><strong>9. 와일드카드 (<code>*</code>, <code>?</code>) 사용하기</strong></h2>
<ul>
<li><code>*</code> : 모든 문자에 해당하는 파일을 선택.<pre><code class="language-bash">ls *.txt  # .txt 확장자를 가진 모든 파일 출력</code></pre>
</li>
<li><code>?</code> : 한 글자에 해당하는 파일을 선택.<pre><code class="language-bash">ls file?.txt  # file1.txt, file2.txt 포함</code></pre>
</li>
</ul>
<hr />
<h2 id="10-파일-및-디렉토리-이동-및-복사-mv-cp-명령어"><strong>10. 파일 및 디렉토리 이동 및 복사 (<code>mv</code>, <code>cp</code> 명령어)</strong></h2>
<ul>
<li>파일 이동(<code>mv</code>)<pre><code class="language-bash">mv file.txt /home/user/Documents/  # 파일을 다른 폴더로 이동</code></pre>
</li>
<li>파일 복사(<code>cp</code>)<pre><code class="language-bash">cp file.txt copy.txt  # 파일 복사
cp -r dir1 dir2       # 디렉토리 복사</code></pre>
</li>
</ul>
<hr />
<h2 id="11-파일-이름-변경-리네임-mv-명령어"><strong>11. 파일 이름 변경 (리네임, <code>mv</code> 명령어)</strong></h2>
<pre><code class="language-bash">mv oldname.txt newname.txt</code></pre>
<ul>
<li>파일 또는 디렉토리의 이름을 변경합니다.</li>
</ul>
<hr />
<h2 id="12-파일-및-디렉토리-삭제-rm-rmdir-명령어"><strong>12. 파일 및 디렉토리 삭제 (<code>rm</code>, <code>rmdir</code> 명령어)</strong></h2>
<ul>
<li>파일 삭제 (<code>rm</code>)<pre><code class="language-bash">rm file.txt  # 파일 삭제
rm -r folder # 폴더 삭제</code></pre>
</li>
<li>빈 디렉토리 삭제 (<code>rmdir</code>)<pre><code class="language-bash">rmdir empty_folder  # 비어 있는 폴더 삭제</code></pre>
</li>
</ul>
<hr />
<h2 id="13-파일-내용-개수-확인-wc-명령어"><strong>13. 파일 내용 개수 확인 (<code>wc</code> 명령어)</strong></h2>
<pre><code class="language-bash">wc file.txt</code></pre>
<ul>
<li>파일의 줄 수, 단어 수, 바이트 수를 출력합니다.<pre><code class="language-bash">wc -l file.txt  # 줄 수만 출력
wc -w file.txt  # 단어 수만 출력
wc -c file.txt  # 바이트 크기 출력</code></pre>
</li>
</ul>
<hr />
<h2 id="14-ls-명령어와-wc-명령어-조합"><strong>14. <code>ls</code> 명령어와 <code>wc</code> 명령어 조합</strong></h2>
<pre><code class="language-bash">ls | wc -l</code></pre>
<ul>
<li>현재 디렉토리에 있는 파일 및 폴더 개수를 계산합니다.</li>
</ul>
<hr />