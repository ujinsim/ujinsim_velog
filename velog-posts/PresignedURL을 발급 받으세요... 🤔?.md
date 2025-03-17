<blockquote>
<p>presignedurl 발급 받고
presignedurl로 s3에 올린 후
-&gt; 파일 개수만큼 위 1,2번 반복 후
presignedurl 발급때 만들어진 모든 id를
answer의 value값으로 보내주시면 될 것 같아요</p>
</blockquote>
<p>백앤드 분이 파일 업로드 API 관련해서 (위에)라고 해주셨는데 이게 머징 ㅠㅠㅠㅠㅠ 뭔 말인지 이해가 잘 안돼서
그래서 <strong>S3 Presigned URL</strong>의 개념에 대해서 우선적으로 정리하고 가려고 한다. </p>
<h3 id="s3의-개념">S3의 개념</h3>
<blockquote>
<p><strong>Amazon S3(Simple Storage Service)</strong>는 아마존 웹 서비스(AWS)가 제공하는 클라우드 스토리지 서비스입니다. S3는 파일, 데이터 및 다양한 유형의 미디어 등을 저장하고 관리하는 데 사용되는 웹 기반 스토리지 시스템입니다.</p>
</blockquote>
<h3 id="s3를-사용하는-이유">S3를 사용하는 이유</h3>
<p><strong>1. 스토리지 및 데이터 저장</strong>
S3는 파일, 이미지, 비디오, 문서 등의 데이터를 저장하기 위한 공간을 제공합니다. 이 데이터들은 &quot;버킷&quot;이라고 불리는 저장 공간에 저장됩니다. 각 버킷은 전 세계 어느 곳에서나 고유한 이름을 가지며, 이를 통해 데이터를 관리하고 접근할 수 있습니다.</p>
<p><strong>2. 확장성</strong>
S3는 확장 가능한 서비스로, 수천 개에서 수백만 개의 파일을 저장하고 관리할 수 있습니다. 필요에 따라 스토리지 용량을 조정하거나 파일을 추가하거나 삭제할 수 있습니다.</p>
<p><strong>3. 데이터 보안 및 접근 제어</strong>
S3는 데이터 보안을 위한 다양한 기능을 제공합니다. 데이터를 암호화하여 저장하고, 접근 권한을 세밀하게 제어하여 누가 데이터에 접근하고 수정할 수 있는지를 관리할 수 있습니다.</p>
<p><strong>4. 데이터 백업 및 복원</strong>
S3는 데이터의 백업과 복원 기능을 사용하여 중요한 데이터를 안전하게 저장하고, 필요한 경우에는 데이터를 원래 상태로 복원할 수 있습니다.</p>
<p><strong>5. 웹 호스팅 및 정적 웹 사이트 호스팅</strong>
S3를 사용하여 정적 웹 페이지나 웹 사이트를 호스팅 할 수 있습니다. 이를 통해 비용을 절감하고, 빠르고 안정적인 웹 호스팅 환경을 구축할 수 있습니다.</p>
<p><strong>6. 데이터 전송 및 배포</strong>
S3를 사용하면 전 세계 어디에서나 데이터를 손쉽게 업로드하고 다운로드할 수 있습니다. 또한, 데이터를 글로벌하게 배포하기 위해 S3의 내용을 Content Delivery Network(CDN)와 통합하여 사용할 수 있습니다.</p>
<p><strong>7. 비용 효율성</strong>
S3는 사용한 용량에 따라 과금되며, 필요한 만큼만 비용을 지불하면 됩니다. 이로써 비용을 효율적으로 관리하면서 필요한 데이터 스토리지를 유연하게 활용할 수 있습니다.</p>
<p>요약하자면, Amazon S3는 클라우드 기반의 스토리지 서비스로, 데이터 저장, 보안, 관리, 백업 등 다양한 기능을 제공합니다.  S3를 사용하면 사용자는 높은 확장성과 신뢰성을 갖춘 데이터 스토리지 솔루션을 구축하고 운영할 수 있습니다.</p>
<h3 id="s3의-파일-공유하는-방법">S3의 파일 공유하는 방법</h3>
<p><strong>1. 모든 파일을 퍼블릭으로 만들기</strong></p>
<ul>
<li>장점 : 별도의 관리하 필요 없음</li>
<li>단점 : 아무나 파일 다운로드 가능 </li>
</ul>
<p>*<em>2. IAM 자격증명 공유 (Access Key Pair) *</em></p>
<ul>
<li><p>장점 : 지정한 사람만 공유 가능</p>
</li>
<li><p>단점 </p>
<ul>
<li>자격증명 유출/변경 시 공유자 모두에게 다시 부여 필요</li>
<li>자격증명의 관리가 어려움</li>
</ul>
</li>
</ul>
<p><strong>3. IAM 사용자 부여하기</strong></p>
<ul>
<li><p>장점 : 지정한 사람만 공유가능</p>
</li>
<li><p>단점 </p>
<ul>
<li>IAM 사용자 숫자 제한</li>
<li>모든 유저에게 IAM 사용자를 부여하는 과정 필요</li>
<li>유지보수의 어려움 </li>
</ul>
</li>
</ul>
<p>*<em>4. Presigned URL *</em></p>
<ul>
<li>장점<ul>
<li>지정한 사람만 공유 가능</li>
<li>만료기간 설정 가능</li>
<li>권한 관리 가능</li>
<li>HTTP 기반으로 접근 가능 </li>
</ul>
</li>
</ul>
<h3 id="presigned-url">Presigned URL</h3>
<p>: S3의 파일을 안전하게 공유하고 싶을 때 사용</p>
<ul>
<li><p>생성자가 가진 권한으로 파일에 접근 가능한 임시 URL을 생성</p>
<ul>
<li>URL의 만료 기간 지정 가능</li>
<li>Method(Get, Post등) 설정 가능 </li>
</ul>
</li>
<li><p>URL의 권한은 생성자가 가진 권한중 일부 혹은 전체 사용</p>
<ul>
<li>예 : 생성자가 Get 권한이 없다면 URL로 GET 불가능</li>
</ul>
</li>
</ul>
<p>그럼 위에 말을 다시 이해해보자 </p>
<blockquote>
<p>presignedurl 발급 받고
presignedurl로 s3에 올린 후
-&gt; 파일 개수만큼 위 1,2번 반복 후
presignedurl 발급때 만들어진 모든 id를
answer의 value값으로 보내주시면 될 것 같아요</p>
</blockquote>
<ol>
<li><strong>Presigned URL</strong> 발급 (PUT 권한 부여)
생성자가 <strong>Presigned URL</strong>을 발급하고, 이를 사용해 클라이언트가 <strong>S3에 파일 업로드</strong> 가능.</li>
<li><strong>Presigned URL</strong>을 사용해 <strong>파일 업로드</strong></li>
<li>클라이언트는 발급받은 URL을 이용해 <strong>S3에 파일 업로드</strong>.</li>
<li>업로드가 완료되면 <strong>S3 객체의 Key(파일 ID)</strong> 가 생성됨.</li>
<li>업로드된 파일의 <strong>Key(파일 ID)</strong>를 서버로 전송</li>
<li>서버는 <strong>이 Key</strong>를 저장하여 이후 요청에서 활용 가능.</li>
<li>필요할 때 <strong>Presigned URL 발급(GET 권한 부여)</strong> → 다운로드 가능</li>
<li><strong>저장된 Key</strong>를 이용해 <strong>Presigned URL을 다시 생성</strong>하면 해당 파일을 다운로드할 수 있음.</li>
</ol>
<p><strong>즉, Presigned URL을 사용하면 생성자의 권한을 기반으로 특정 파일을 업로드하거나 다운로드할 수 있고, 이를 통해 다른 요청에서도 해당 파일을 관리할 수 있게 되는 것이다 ‼️</strong></p>
<p><em>참고자료</em> <a href="https://bigco-growth-diary.tistory.com/43">[ S3 ] S3 란 무엇인가? S3 개념</a>
<a href="https://www.youtube.com/watch?v=v2yJLMltX1Y">S3의 파일을 안전하게 공유하고 싶다면: S3 Presigned URL</a></p>