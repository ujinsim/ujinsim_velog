<h2 id="목차">목차</h2>
<ul>
<li>Operating System Services</li>
<li>User and Operating System Interface</li>
<li>System Calls</li>
<li>System Services</li>
<li>Linkers and Loaders</li>
<li>Why Applications Are Operating System Specific</li>
<li>Operating System Structure</li>
</ul>
<h2 id="1운영체제-서비스">1.운영체제 서비스</h2>
<p>OS는 프로그램들이 돌아갈 환경을 제공한다. 아래의 그림은 OS sevices들과 어떻게 서로 연관되어 있는지 보여준다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/44048264-f0b2-4512-b809-8872000923d1/image.png" /></p>
<p><strong>User interface</strong> </p>
<ul>
<li>GUI</li>
<li>touch screen</li>
<li>command line : 터미널 등</li>
</ul>
<p>communication || resource allocation 기준으로 </p>
<ul>
<li>왼쪽 : 프로그램을 쉽게 할 수 있도록</li>
<li>오른쪽 : 컴퓨터 시스템의 효율을 높임</li>
</ul>
<p><strong>왼쪽</strong> </p>
<ul>
<li><strong>Program execution</strong> : 시스템은 프로그램을 메모리에 로드시킨 후, 작동시켜야 한다. 프로그램은 또한 정상,비정상적으로 종료될 수 있어야 한다.</li>
<li><strong>I/O operations</strong>: 프로그램은 file이나 입출력 장치로부터 입출력을 요구할 수 있다. <strong>효율성과 시스템 보호를 위해, 사용자는 직접적으로 입출력 장치에 접근해서는 안된다. OS가 대신 입출력 관리를 해준다.</strong></li>
<li><strong>File-system manipulation</strong>: 프로그래머들은 디렉토리와 파일을 읽고 써야한다. 또한 삭제나 생성 역시 해야 하는데, 많은 OS들은 파일 시스템들의 다양성을 제공하여, custom하게 사용할 수 있게 해준다</li>
<li><strong>Error detection</strong>: OS는 발생 가능한 errors들을 파악하고 수정할 수 있어야 한다. 에러들은 CPU나 memory hardware, 입출력장치, user program에서 발생 가능하다. 각각의 에러들에 대해 OS는 적절한 조치를 취해서, 에러를 수정하고 일관된 연산을 가능하게 하도록 해야한다. 때때로 OS는 에러를 발생시키는 프로세스를 종료시키거나, 프로세스에게 에러 코드를 반환하여 수정하게 한다.</li>
</ul>
<p><strong>오른쪽</strong></p>
<ul>
<li><strong>Communications</strong> : 프로세스간의 정보교환이 필요할 때가 있다. <strong>Communications은 shared memory를 이용하여 실행되며</strong>, 여러개의 process들이 shared memory에 읽고 쓰거나 <strong>message passing</strong> 방식을 통해서 정보를 주고 받는다.</li>
<li><strong>Resource allocation</strong> : <strong>multi process들이 동시에 돌아갈 때, resources은 각각 할당되어야 한다</strong>. OS는 여러 종류의 resources을 관리한다. 몇 개의 resources(CPU, main memory, File system)에 관해서는 allocation code에 의해 할당된다.</li>
<li><strong>Logging(account아니고)</strong> : <strong>어떤 프로그램이 얼만큼의 resources들을 사용하는지 추적해야 한다</strong>. 이러한 사용량 통계들은 시스템 관리자에게 유용한 도구가 되어, computing service를 향상하는데 도움이 될 것이다. → 자원할당을 위해 추적</li>
<li><strong>Protection and security</strong> : 여러개의 프로세스들이 실행되고 있을때, 한개의 프로세스가 다른 프로세스나 OS에 간섭하는 경우는 없어야 한다. Protection(보호)는 시스템 자원들에 대한 접근이 통제되어 있다는 것을 보장한다. Security(보안)은 외부로부터의 접근을 막고, 외부 입출력 장치도 보호한다.</li>
</ul>
<hr />
<h2 id="2-사용자와-운영체제-인터페이스">2. 사용자와 운영체제 인터페이스</h2>
<h3 id="21-command-interpreters">2.1. Command Interpreters</h3>
<ul>
<li><p>what? 🤔</p>
<p>  사용자가 명령을 입력하면 해석해서 실행해줌 </p>
<p>  <strong>있는 곳</strong></p>
<ul>
<li>커맨드</li>
<li>시스템 프로그램 → shell이라고 부르기도 함 (여러개 제공)</li>
</ul>
</li>
</ul>
<h3 id="22-graphical-user-interface"><strong>2.2. Graphical User Interface</strong></h3>
<ul>
<li><p>what? 🤔</p>
<p>  우리에게 가장 친숙한 인터페이스로, mouse-based의 window, system menu를 통해 명령어들을 실행한다.</p>
<p>  <strong>사용자 친화적 인터페이스</strong></p>
<ul>
<li><p>마우스, 키보드, 모니터</p>
</li>
<li><p>아이콘, 프로그램 등</p>
</li>
<li><p>다양한 버튼 등</p>
</li>
<li><p><em>터치스크린 인터페이스*</em> </p>
</li>
<li><p>음성 지시</p>
</li>
<li><p>터치 스크린</p>
</li>
<li><p><em>선택지*</em></p>
<p>System administrators and power users use CLI
➢ more efficient, faster access, make repetitive tasks easier(shell scripts)
◆ Most Windows users are happy to use GUI
◆ Design of UI is not a direct function of OS</p>
<p>종류 </p>
</li>
<li><p>touch-screen Interface</p>
</li>
<li><p>사용자 인터페이스 - 운영체제의 기능은 아님</p>
</li>
</ul>
</li>
</ul>
<h3 id="23-system-calls"><strong>2.3. System calls</strong></h3>
<ul>
<li><p>what? 🤔</p>
<p>  <strong>: 운영 체제의 커널이 제공하는 서비스에 대해, 응용 프로그램의 요청에 따라 커널에 접근하기 위한 인터페이스이다. ( C나 C++ 로 구현 )</strong></p>
<p>  운영체제에게 시스템 콜을 통해 요청하여 요청을 받음 사용자 인터페이스를 연결을 해줘요~ </p>
<p>  <strong>파일을 복사하기 위한 System Call</strong></p>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/b01f44e1-c77b-4e42-bbb6-96848c559d47/image.png" /></p>
<pre><code>![](https://velog.velcdn.com/images/ujinsimss_/post/c5f5b411-5e23-436a-9e31-37fc5a9345f9/image.png)</code></pre><h3 id="function-전달">function 전달</h3>
<p><strong>Application programming Interface (API)</strong></p>
<p>개발자들은 주로 API에 따라 프로그램을 설계한다. 실제로 <strong>API를 이루는 함수들은 System calls을 호출</strong>하여 application programmer에게 도움을 준다. 실제 System calls을 호출하는 것보다, API 위에서 설계하는 이유는 여러가지 이유가 있다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/22e5e3da-61eb-4911-b26e-174dcec4a175/image.png" /></p>
<p><strong>run-time-environment : system-call-interface</strong>는 API의 function-call을 가로채어 필수적인 system calls을 OS에게 호출한다. 이후 system call 상태를 반환한다.</p>
<ul>
<li>시스템 번호와 연결링크를 저장을 함</li>
<li>시스템 인터페이스는 요청에 해당하는 콜 번호를 호출을 함</li>
<li>수행 후 리턴 값을 받아와서 시스템 콜 인터페이스를 통해 사용자 프로그램에 전달</li>
</ul>
<h3 id="파라미터-전달">파라미터 전달</h3>
<ol>
<li>CPU 내에 파라미터 저장 </li>
<li>레지스터보다 파라미터가 많으면 (개수 제한 xx)</li>
</ol>
<ul>
<li>2-1. 메모리 블럭에 저장</li>
<li>2-2. 스택 공간에 저장</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/9b91ca3d-b70d-447c-9710-2f421661cd7e/image.png" /></p>
<hr />
<h2 id="3--시스템-서비스">3 . 시스템 서비스</h2>
<p>프로그램 개발과 실행을 위한 편리한 환경을 제공한다. System calls을 모아둔 간단한 것도 있으며, 복잡한 것도 존재한다. </p>
<ul>
<li>File management</li>
<li>Status information</li>
<li>File modification</li>
<li>Programming language support</li>
<li>Program loading and execution : 통신</li>
<li>Communications : 시스템이 부팅될 때 부터 ~ ..</li>
<li>Background services</li>
</ul>
<hr />
<h2 id="4-linkers-and-loaders">4. Linkers and Loaders</h2>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/1d202f8f-5b90-4635-82ab-522a429197c4/image.png" /></p>
<p>고급언어로 작성된 파일을 컴파일러로 컴파일 → 오브젝트 파일 ( 기계어 ) : 메인메모리 어디에 올라갈지 모르는 상태 ( relocatable object files : 프로그램에서 상대적인 번지수로 지정 ) → 실제 메모리 주소 할당과 진짜 메모리 지정을 해줘요 !</p>
<p><strong>LInker</strong> : 여러개의  relocatable object files → 하나의 실행가능한 파일로 묶어줌 </p>
<p><strong>Loader</strong> : LInker를 거친 파일을 실행하도록 메인 메모리에 올리기 </p>
<p>→ 완료 후 최종 주소를 통해 <strong>Relocation</strong> 완료 </p>
<p>: 너무 커질경우 실행하면 (<strong>dynamically link libraries)</strong> : 프로그램 중에 사용할 수 있는 라이브러리 ( 연결 정보만 저장 : 동적 저장 )</p>
<hr />
<h2 id="5-why-applications-are-os-specific">5. Why Applications are OS Specific</h2>
<p>: 인터프리터가 환경에 맞춰서 컴파일과 실행을 해줌 </p>
<ul>
<li>프로그램을 실행하면 System Call을 해야됨 → 그걸 위한 API가 OS마다 다르기 때문에 해당 코드를 바꿔야 한다.</li>
<li>CPU 마다 명령문이 다름</li>
<li>System Call 종류도 다름</li>
</ul>
<hr />
<h2 id="6-operationg-system-structure">6. Operationg System Structure</h2>
<p>크고 복잡한 현대의 시스템을 다루기 위해서는 운영체제는 조심스럽게 engineered 되어야 하며, 수정하기도 쉬워야 한다.</p>
<p>가장 흔히 사용하는 방법으로는 <strong>task를 여러 components(modules)로 쪼개는 방법</strong>이다.</p>
<h3 id="61-monolithic-structure"><strong>6.1 Monolithic Structure</strong></h3>
<p><strong>OS를 구성하는 가장 간단한 방법은 구조가 없는 것, 즉 1개의 kernel에 모든 기능을 넣는 것이다.</strong> </p>
<p>단점</p>
<ul>
<li>monolithic structure는 <strong>확장하기 어렵다</strong></li>
<li>구현이 어렵다</li>
</ul>
<p>장점 </p>
<ul>
<li><p>system-call interfaces에서 적은 system-call interfaces에서 적은 overhead (계층사이의 이동)</p>
</li>
<li><p><strong>정보교환이 빨라 speed와 efficiency가 매우 좋다 ( 현대에 많이 사용하는 이유 )</strong></p>
</li>
<li><p>커널을 수정할 수 있는 모듈식 구조</p>
</li>
</ul>
<h3 id="62-layered-approach"><strong>6.2 Layered Approach</strong></h3>
<p>운영체제를 여러개의 층으로 </p>
<ul>
<li>구성 아래층 기능만 사용 가능</li>
</ul>
<p>layer 0 :</p>
<p>layer 2 : 1에서 제공하는 기능만 사용 </p>
<p>장점</p>
<ul>
<li>한 부분의 변화가 그 부분만 영향을 끼친다는 점이다.</li>
<li>시스템 내부에 변화와 생성을 자유롭게 할 수 있다.</li>
</ul>
<p>단점</p>
<ul>
<li>성능이 떨어짐 ( call을 할 때 overhead 발생 )</li>
</ul>
<h3 id="63-microkernels"><strong>6.3 Microkernels</strong></h3>
<p><strong>OS의 불필요한 요소들을 커널에서 제거하고, user-level 단계의 프로그램들로 별도의 공간을 두어서 분리</strong>시키는 것이다. (Small Kernel)</p>
<ul>
<li>CPU scheduling</li>
<li>memory managment</li>
<li>interprocess Communication : user level과 소통하게 함</li>
</ul>
<p>user level </p>
<ul>
<li>file System</li>
<li>Device Driver</li>
</ul>
<p>장점</p>
<ul>
<li>확장이 쉬움</li>
<li>하드웨어 수정시에도 변경이 쉬움</li>
<li>커널모드가 아니기에 유저모드기에 안정성</li>
</ul>
<p>단점 </p>
<ul>
<li>마이크로 오버헤드가 계속 등장하고 process 변경이 잦아 overhead 발생</li>
</ul>
<h3 id="64-modules"><strong>6.4 Modules</strong></h3>
<p><strong>커널에 핵심 요소들이 있고, 부가 서비스들을 modules을 통해 추가(loadsble Kernel modules) 할 수 있다</strong></p>
<p>장점</p>
<ul>
<li><strong>services을 추가하는 것이 쉬우며</strong>(직접 커널에 연결)</li>
<li>각각의 모듈이 모듈을 호출가능해 계층적인 단점을 보완</li>
<li><strong>message passing</strong> 같은 것이 필요하지 않아 빠른 속도가 있다.</li>
</ul>
<h3 id="65-hybrid-systems"><strong>6.5 Hybrid Systems</strong></h3>
<p>많은 운영체제들은 위에서 언급되었던 설계 기법들을 혼합하여 사용한다. 리눅스는 하나의 공간에 OS가 있기 때문에 monolithic하다고 할수도 있지만, 새로운 기능을 dynamically하게 추가할 수 있다는 점에서 modular하다고 할수도 있다. 윈도우 역시 큰 관점에서는 monolithic하지만, microkernel 같이 subsystems로 구성되기도 한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/60ae00a7-772d-4341-8852-abc0e32d27ee/image.png" /></p>