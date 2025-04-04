<p>Multipass를 이용한 Ubuntu 가상 머신 생성, SSH 설정, 텍스트 파일 작성 및 SSH 접속 확인 과정까지의 명령어 정리</p>
<hr />
<h2 id="1-multipass-vm-생성-및-접속">1. Multipass VM 생성 및 접속</h2>
<pre><code class="language-bash">multipass launch --name selected-neanderthal
multipass shell selected-neanderthal</code></pre>
<hr />
<h2 id="2-vm-안에서-ssh-서버-설치-및-설정">2. VM 안에서 SSH 서버 설치 및 설정</h2>
<pre><code class="language-bash">sudo apt update
sudo apt install openssh-server

sudo systemctl enable ssh
sudo systemctl start ssh</code></pre>
<hr />
<h2 id="3-비밀번호-인증-허용">3. 비밀번호 인증 허용</h2>
<pre><code class="language-bash">sudo passwd ubuntu</code></pre>
<p>비밀번호 입력 후:</p>
<pre><code class="language-bash">sudo nano /etc/ssh/sshd_config.d/60-cloudimg-settings.conf</code></pre>
<p>→ <code>PasswordAuthentication yes</code> 추가 또는 수정</p>
<pre><code class="language-bash">sudo systemctl restart ssh</code></pre>
<hr />
<h2 id="4-맥북에서-ssh-접속">4. 맥북에서 SSH 접속</h2>
<pre><code class="language-bash">ssh ubuntu@192.168.64.9

cat /etc/ssh/sshd_config | grep Port
// 포트번호 확인용 명령어 </code></pre>
<h2 id="5-텍스트-파일-작성-vm-안에서">5. 텍스트 파일 작성 (VM 안에서)</h2>
<pre><code class="language-bash">echo &quot;안녕하세요, 이건 테스트입니다.&quot; &gt; hello.txt
cat hello.txt</code></pre>
<hr />
<h2 id="6-ssh-세션-종료">6. SSH 세션 종료</h2>
<pre><code class="language-bash">exit</code></pre>
<hr />
<ul>
<li><input checked="" disabled="" type="checkbox" /> Multipass VM 생성</li>
<li><input checked="" disabled="" type="checkbox" /> SSH 서버 실행 및 비밀번호 설정</li>
<li><input checked="" disabled="" type="checkbox" /> 비밀번호 인증 허용 (<code>PasswordAuthentication yes</code>)</li>
<li><input checked="" disabled="" type="checkbox" /> 맥북에서 SSH 접속 성공</li>
<li><input checked="" disabled="" type="checkbox" /> 텍스트 파일 작성 및 확인</li>
</ul>