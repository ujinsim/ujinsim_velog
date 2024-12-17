<h3 id="시작-전-발표">시작 전 발표</h3>
<ol>
<li><strong>운영체제</strong>를 잘 이해하는 것이 <strong>다른 전공 학습에 도움이 될 수 있다</strong>고 생각하십니까? 그렇다면 <strong>어떤 점에서 그렇게 생각</strong>하십니까?</li>
<li><strong>운영체제 학습</strong>은 <strong>취업에 도움이 될 수 있다</strong>고 생각하십니까? 그렇다면 <strong>어떤 점에서 그렇게 생각</strong>하십니까?</li>
</ol>
<hr />
<h2 id="목차">목차</h2>
<ol>
<li><strong>os가 무엇을 하는가</strong></li>
<li><strong>컴퓨터 체제 조직</strong> : 하드웨어가 어떻게 조직화되어 움직이는가 → 이것 안에서 운영체제가 동작하기 때문에 ( intterupt 개념 ) </li>
<li><strong>컴퓨터 체제 구조</strong> : cpu가 여러개 사용되는 멀티 시스템 종류</li>
<li><strong>운영체제가 어떻게 동작하는지</strong> : 프로그램이 여러개 돌아가는게 어떻게 가능한지 ? ( 듀얼모드 , 유저모드, 커널모드 )</li>
<li><strong>컴퓨터 자원 관리</strong> : 하드웨어 장치와 관련하여 어떤 일을 하는지 설명</li>
</ol>
<h2 id="핵심-개념">핵심 개념</h2>
<ul>
<li>interrupt가 어떻게 기본 구성을 움직이는지</li>
<li>커널모드</li>
</ul>
<hr />
<h1 id="정리-✏️-ch1-introduction">정리 ✏️ ch1. Introduction</h1>
<h2 id="1-운영체제">1. 운영체제</h2>
<p>:  컴퓨터의 하드웨어를 제어하고 하드웨어의 자원을 분배 및 관리하는 소프트웨어 </p>
<ul>
<li>커널 : 운영체제의 핵심 , 커널 모듈로 구성 , 하드웨어와 소통하며 가장 가까운 운영체제</li>
<li>하드웨어를 제어하는 소프트웨어</li>
<li>응용 프로그램과 하드웨어 사이의 중계자</li>
</ul>
<h3 id="11-사용자의-관점">1.1 사용자의 관점</h3>
<ul>
<li><strong>편의성(ease of use)</strong> : 한 사용자가 독점 사용하도록 Resource utilization 을 신경 안쓰게 해줌</li>
<li><strong>모바일 디바이스(mobile device)</strong> : 여러 사용자가 여러 모바일 기기와 상호 작용하는 경우 연결을 도와줌</li>
<li><strong>임베디드 컴퓨터(Embedded computers)</strong> : 가전제품이나 자동차에 내장된 컴퓨터가 사용자 인터페이스(UI)가 거의 없거나 전혀 없는 경우에도 사용</li>
</ul>
<h3 id="12-시스템-관점">1.2. 시스템 관점</h3>
<ul>
<li><strong>자원 할당(resource allocator)</strong> : 리소스를 효율적이고 공평하게 관리</li>
<li><strong>프로그램 제어(control program)</strong> : 프로그램 제어를 통한 에러와 부적절한 사용 방지</li>
</ul>
<h3 id="13-운영체제의-정의--🤔">1.3 운영체제의 정의 ? 🤔</h3>
<p>정의할 수 없다 → <strong>너무 다양해서 !</strong> </p>
<h3 id="14-그러나-운영체제가-포함하는-것-">1.4 그러나 운영체제가 포함하는 것 ?</h3>
<ul>
<li><strong>커널</strong>(<strong>kernel</strong>) : 컴퓨터에서 항상 실행되는 핵심 프로그램</li>
<li><strong>미들웨어</strong>(<strong>middleware</strong>) : 개발자를 위한 소프트웨어 프레임워크</li>
<li><strong>시스템 프로그램</strong>(<strong>system programs</strong>) : 운영체제와 연관돼어있으나 커널의 일부분은 아닌것</li>
</ul>
<hr />
<h2 id="2-컴퓨터-시스템-조직">2. 컴퓨터 시스템 조직</h2>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/b5b6c84d-44ad-4aeb-9948-8acdbaea7ed1/image.png" /></p>
<p><strong>구성</strong> </p>
<p><strong>cpu :</strong> 데이터를 이동시킨다  main memory ↔ local buffer of device controller</p>
<p><strong>device controller</strong>  </p>
<ul>
<li>device controller(장치 측면)에 대해서 device driver(OS 측면)라는 장치 제어 프로그램을 가진다</li>
<li><code>local buffer storage</code>와<code>register</code>의 집합</li>
</ul>
<p><strong>common bus</strong> : cpu와 device controller를 이어 메모리 공유 ( 둘이 경쟁 )</p>
<blockquote>
<p>1개 이상의 CPU로 구성되었고 버스(Bus)를 통해서 연결된 기기 제어기(Device Controller)로 연결되었습니다. CPU는 메모리에 명령어들을 저장하고 기기 제어기를 통해서 하드웨어적인 작업을 수행합니다.</p>
</blockquote>
<p><strong>동작 과정</strong> </p>
<ul>
<li>device → device controller(local buffer)로 데이터 이동 → cpu가 device controller를 통해서 device에게 명령</li>
<li>명령이 끝나면 Device controller가 <strong>interrupt</strong>를 통해 CPU에게 실행이 끝남을 알림</li>
</ul>
<p><strong>예시</strong> </p>
<ol>
<li>읽기</li>
<li>1 내용을 바탕으로 명령하기 </li>
<li>이것의 로컬버퍼를 이용하여 데이터로 변환함</li>
<li>디바이스 드라이버에게 작업이 끝났다고 알림</li>
<li>작업이 읽혔다면 다른 부분의 운영체제에게 제어<strong>(interrupts</strong>)를 가하고 데이터를 반환</li>
<li>읽기를 잘 끝냈다와 같은 정보 상태를 반환한다</li>
</ol>
<h3 id="21-interrupts">2.1. Interrupts</h3>
<p>하드웨어가 작동중에 CPU에게 알려주는 신호(signal)입니다. 
시스템 버스(system bus)를 통해서 CPU에게 신호를 전송함으로써 어느 시간이던 하드웨어는 인터럽트를 발생시킬 수 있습니다.</p>
<p><strong>예를 들어 키보드 &quot;A&quot;키를 누른다면 메모리에 A 누름 명령어가 적재되고 CPU에게 알려줍니다. (Interrupt)</strong></p>
<p><strong>Interrupt Handling 과정</strong></p>
<ul>
<li><strong>Interrupt 발생</strong>: 하드웨어 신호가 발생하여 CPU에 전달됨.</li>
<li><strong>작업 멈추고 백업</strong>: 현재 수행 중인 작업을 멈추고 CPU의 상태(레지스터 값 등)를 저장.</li>
<li><strong>Interrupt 처리 장소로 이동</strong>: 인터럽트 벡터를 통해 지정된 위치로 이동하여 인터럽트 루틴을 수행.</li>
<li><strong>수행 완료 후 복구</strong>: 인터럽트 처리 루틴이 완료되면 이전 상태로 복구하여 멈췄던 작업을 다시 시작.</li>
</ul>
<p><strong>Interrupt Components</strong></p>
<ul>
<li><strong>Interrupt Vector</strong>: 특정 인터럽트 처리 루틴의 주소를 저장하는 곳으로, 인터럽트 발생 시 해당 루틴을 즉시 수행하도록 돕습니다.</li>
<li><strong>Interrupt Request Line (IRQ Line)</strong>: 인터럽트 신호를 감지하는 라인입니다.</li>
<li><strong>Interrupt Handling</strong>: 인터럽트가 발생했을 때 어떤 작업을 수행할지 결정하고 처리하는 과정.</li>
<li><strong>Interrupt Mechanism</strong>: 인터럽트가 발생했을 때 현재 작업을 멈추고 인터럽트 루틴을 실행하도록 하는 시스템의 동작 원리.</li>
<li><strong>Interrupt Chaining</strong>: 인터럽트 벡터보다 많은 장치가 연결된 경우, 각 장치의 요청을 처리하기 위해 체인 구조로 인터럽트를 연결하여 관리합니다.</li>
</ul>
<p><strong>Interrupt Timeline</strong></p>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/9c48c98d-5439-4abc-a66c-f22b14735232/image.png" /></p>
<ul>
<li>인터럽트가 발생하여 처리되고 원래 작업으로 돌아가는 전체 과정을 시각적으로 나타낸 것입니다. 일반적으로 작업 멈춤 → 인터럽트 처리 → 상태 복구 순서로 진행됩니다.</li>
</ul>
<p><strong>Interrupt-Driven I/O Cycle</strong></p>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/58e381fa-123f-47a5-9268-b6057737ae3d/image.png" /></p>
<ul>
<li>I/O 작업이 CPU의 주도하에 이루어지는 방식으로, CPU가 직접 I/O 장치를 제어하지 않고 인터럽트를 통해 데이터를 송수신합니다.</li>
</ul>
<p><strong>Modern Interrupt Handling</strong></p>
<ul>
<li><strong>Nonmaskable Interrupt</strong>: reserved for events such as <strong>unrecoverable</strong> momory errors (무시되서는 안됨)</li>
<li><strong>Maskable Interrupt</strong>: can be turned off by CPU before execution of critical instructions that must <strong>not be interrupted</strong>. used by device
controllers (무시 o)</li>
<li><strong>Interrupt priority levels : 중요도 낮으면 마스킹 , 중요도 높으면 먼저 시행</strong></li>
</ul>
<p><strong>Intel processor event-vector table</strong></p>
<ul>
<li>특정 이벤트 또는 인터럽트와 관련된 설명을 저장하여 어떤 작업을 수행할지 결정하는 테이블</li>
<li>vector number : description으로 저장</li>
</ul>
<hr />
<h3 id="22-storage-structure">2.2 Storage Structure</h3>
<p><strong>Primary Storage (주 저장 장치)</strong></p>
<ul>
<li><strong>Registers (레지스터)</strong>: CPU에 가장 가까운 메모리로, 속도가 매우 빠르지만 용량이 작습니다.</li>
<li><strong>Cache (캐시)</strong>: CPU와 메인 메모리 사이에 위치해 데이터를 임시 저장하여 빠르게 접근할 수 있도록 합니다.</li>
<li><strong>Main Memory (메인 메모리)</strong> - <strong>RAM (Random Access Memory, 휘발성 메모리)</strong>:<ul>
<li>CPU가 직접 접근할 수 있는 유일한 대용량 저장 매체입니다.</li>
<li><strong>장점</strong>: 매우 빠른 접근 시간, 프로그램 실행에 필수적<ul>
<li><strong>예시</strong>: DRAM (Dynamic Random-Access Memory)</li>
</ul>
</li>
<li><strong>단점</strong>: 휘발성(전원 공급이 중단되면 데이터 손실)</li>
</ul>
</li>
</ul>
<p><strong>Secondary Storage (보조 저장 장치)</strong></p>
<ul>
<li>메인 메모리의 확장으로, 큰 비휘발성 저장 용량을 제공합니다.</li>
<li><strong>Nonvolatile Memory (NVM, 비휘발성 메모리)</strong>: 플래시 메모리 등, HDD보다 빠름.</li>
<li><strong>Hard-Disk Drives (HDDs, 하드 디스크 드라이브)</strong>:<ul>
<li><strong>장점</strong>: 큰 저장 용량, 비휘발성</li>
<li><strong>단점</strong>: 비교적 느린 접근 시간</li>
</ul>
</li>
</ul>
<p><strong>Tertiary Storage (3차 저장 장치)</strong></p>
<ul>
<li>주로 백업 및 데이터 보관용으로 사용됩니다.</li>
<li><strong>Optical Disk (광학 디스크)</strong>: CD-ROM, DVD 등</li>
<li><strong>Magnetic Tapes (마그네틱 테이프)</strong>:<ul>
<li><strong>장점</strong>: 매우 큰 용량, 데이터 장기 저장 가능</li>
<li><strong>단점</strong>: 가장 느린 접근 시간, 순차적 접근 필요</li>
</ul>
</li>
</ul>
<p><strong>Bootstrap Program (부트스트랩 프로그램)</strong></p>
<p><strong>부트스트랩 프로그램</strong>은 컴퓨터가 전원을 켤 때 최초로 실행되는 소프트웨어입니다. 이 프로그램의 주요 역할은 컴퓨터를 초기화하고 운영 체제를 메모리에 로드하여 실행하는 것입니다. </p>
<p><strong>저장 위치와 특성</strong></p>
<ul>
<li><strong>저장 위치</strong>: 부트스트랩 프로그램은 주로 EPROM (Electrically Erasable Programmable Read-Only Memory, 전기적으로 삭제 가능한 프로그래머블 읽기 전용 메모리)**에 저장됩니다.</li>
<li><strong>펌웨어(Firmware)</strong>: 부트스트랩 프로그램이 저장된 EPROM은 일반적으로 펌웨어로 알려져 있으며, 이는 시스템에 필수적이고 자주 변경되지 않는 소프트웨어를 의미합니다.</li>
<li><strong>특성</strong>: 비휘발성 메모리이기 때문에 전원이 꺼져도 데이터가 유지됩니다. EPROM은 필요할 때 데이터를 수정할 수 있지만, 자주 쓰거나 지워지지 않는 특성을 가지고 있습니다.</li>
</ul>
<p><strong>장점</strong></p>
<ul>
<li><strong>비휘발성</strong>: 전원이 꺼져도 유지되므로, 컴퓨터가 다시 켜질 때마다 사용됩니다.</li>
<li><strong>안정성</strong>: 자주 변경되지 않아 안정적이며, 시스템의 신뢰성을 높입니다c</li>
</ul>
<p><strong>단점</strong></p>
<ul>
<li><strong>수정 어려움</strong>: 수정이 필요할 때마다 EPROM: 하드디스크 을 삭제하고 다시 기록해야 하므로 번거로울 수 있습니다.</li>
<li><strong>속도 제한</strong>: RAM처럼 빠르게 동작하지 않으므로 주로 프로그램을 로드하고 실행하는 역할에 사용됩니다.</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/9b0e8f6a-bc64-4409-bd39-238a84d8f1c6/image.png" /></p>
<p><strong>저장 특성</strong></p>
<ul>
<li><strong>휘발성 (Volatile)</strong>: 전원이 꺼지면 데이터가 사라지는 메모리 (예: 레지스터, 캐시, 메인 메모리)</li>
<li><strong>비휘발성 (Nonvolatile)</strong>: 전원이 꺼져도 데이터가 유지되는 저장 장치 (예: 하드디스크, 옵티컬 디스크, 마그네틱 테이프)</li>
</ul>
<p><strong>Storage Capacity and Access Time (저장 용량과 접근 시간)</strong></p>
<ul>
<li>용량이 클수록 접근 시간이 느리고, 작은 용량의 메모리일수록 접근 시간이 빠릅니다.</li>
<li><strong>단위</strong>: Byte → Kilobyte (KB) → Megabyte (MB) → Gigabyte (GB) → Terabyte (TB) → Petabyte (PB)</li>
</ul>
<h3 id="23-io-structure">2.3 I/O Structure</h3>
<p>OS의 대부분은 I/O를 관리하는데 할당된다. 시스템의 신뢰도와 성능에 대한 중요성과 devices간의 다른 특성 때문이라고 한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/bbdeeb48-e3bc-47e2-9c32-996f8008b533/image.png" /></p>
<p><strong>Direct Memory Access</strong></p>
<p>다양한 장치들로부터 얻은 데이터들은 common bus를 통해 이동한다. 만약 무거운 데이터를 CPU를 통해 옮기려 할때, <strong>Direct Memory Access</strong>를 이용하여 <strong>CPU의 개입없이 memory와 장치와 소통하게 한다</strong>.</p>
<hr />
<h2 id="3-computer-system-architecture">3. Computer-System Architecture</h2>
<blockquote>
<p><strong>Definitions of Computer System Components</strong></p>
<ul>
<li>CPU : instuctions을 실행하는 하드웨어</li>
<li>Processor : 한 개 이상의 CPUs를 가지는 chip</li>
<li>Core : CPU의 연산 단위</li>
<li>Multicore : 같은 CPU에서 여러개의 연산 코어를 포함하는 것</li>
<li>Multiprocessor : 여러개의 프로세서를 가지는 것</li>
</ul>
</blockquote>
<h3 id="31-symmetric-multiprocessing-smp"><strong>3.1 Symmetric Multiprocessing (SMP)</strong></h3>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/d1e6108c-61eb-4a09-aa20-b3d6a3cd5c1f/image.png" /></p>
<p>여러 CPU가 하나의 메모리를 공유하며 대등한 권한으로 작업을 수행하는 구조입니다.</p>
<p>전통적인 컴퓨터 시스템들은 <strong>단일 코어</strong>를 사용했다. <strong>코어는 instructions과 registers를 실행하는 요소이다</strong>. 하나의 CPU에서 한 개의 process만 처리가 가능한 시스템이다.</p>
<ul>
<li><strong>Flexibility</strong>: 모든 CPU가 대등한 작업을 처리할 수 있습니다.</li>
<li><strong>Overhead</strong>: 작업 스케줄링 및 동기화로 인한 오버헤드가 발생할 수 있습니다.</li>
</ul>
<h3 id="32-multicore-system">3.2 Multicore System</h3>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/d70168f3-b0b9-4cc9-a4f2-a8dda4edfe9a/image.png" /></p>
<p>현대 컴퓨터들은 <strong>multiprocessor systems</strong> 기반이다. 이러한 시스템들은 보통 하나의 CPU에 2개 이상의 processor를 탑재하였다. </p>
<p> 장점</p>
<ul>
<li><strong>적은 시간에 많을 일</strong>을 할수 있게 된 것이다.</li>
</ul>
<p>단점</p>
<ul>
<li>processor를 많이 한다고 해서 기대만큼 성능이 개선되지는 않는다. (오버헤드 때문)</li>
</ul>
<p>Symmetric multiprocessing(SMP)가 주로 멀티 프로세싱 시스템에 많이 사용된다. Peer CPU processor들이 모든 작업(OS함수들이나, 사용자 process)를 수행한다. 위의 그림은 SMP의 예시이다. CPU마다 고유 레지스터를 가지고 있지만, system bus에 의해 공유한다. <strong>이러한 방법의 장점으로는 N개의 CPU가 있으면 N개의 process를 동시에 실행할 수 있다는 것이다.</strong></p>
<p> <strong>하지만 CPU가 분리되어 어떤 것은 overload되지만 다른 것은 놀고 있는 문제가 발생할 수 있다.</strong> 이를 막기 위해 data structures를 Processor들이 공유하여, processor간 processes와 resources를 동적으로 분배하여 workload variance를 낮출 수 있다.</p>
<h3 id="33-non-uniform-memory-access-numa">3.3 Non-Uniform Memory Access (NUMA)</h3>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/01e9e438-575a-415d-b144-2d417e9dbc5d/image.png" /></p>
<p>각 CPU가 자체적인 로컬 메모리를 가지며, 다른 CPU의 메모리에 접근할 때는 공유 인터커넥트를 사용하는 구조입니다.</p>
<p>로컬 메모리에 대한 접근은 빠르지만, 다른 CPU의 메모리에 접근할 때는 지연이 발생합니다.</p>
<ul>
<li><strong>장점</strong>:<ul>
<li><strong>Scalability</strong>: 많은 프로세서를 추가해도 성능이 더 잘 확장됩니다.</li>
<li><strong>Efficient Memory Access</strong>: 로컬 메모리에 접근할 때 빠르며, 충돌이 적습니다.</li>
</ul>
</li>
<li><strong>단점</strong>:<ul>
<li><strong>Latency</strong>: 원격 메모리에 접근할 때 지연 시간이 발생합니다.</li>
<li><strong>Complex Configuration</strong>: 설정과 최적화가 복잡할 수 있습니다.</li>
</ul>
</li>
</ul>
<h3 id="34-clustered-systems">3.4 Clustered Systems</h3>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/9aff91b5-d624-4be1-a729-a0931ac77a66/image.png" /></p>
<p>여러 독립적인 시스템(노드)이 상호 연결되어 저장소를 공유하며 작업을 분산 처리하는 구조입니다.</p>
<p>노드들이 함께 작업하며, 주로 스토리지 영역 네트워크(SAN)를 통해 데이터를 공유합니다.</p>
<ul>
<li><strong>장점</strong>:<ul>
<li><strong>High Availability</strong>: 한 시스템이 고장 나도 다른 시스템이 작동하여 서비스를 유지합니다.</li>
<li><strong>Scalability</strong>: 노드를 추가하여 시스템을 쉽게 확장할 수 있습니다.</li>
</ul>
</li>
<li><strong>단점</strong>:<ul>
<li><strong>Complexity</strong>: 클러스터 관리가 복잡하며, 시스템 간 동기화와 잠금 관리가 필요합니다.</li>
<li><strong>Cost</strong>: 구성 및 유지 비용이 높을 수 있습니다.</li>
</ul>
</li>
</ul>
<hr />
<h2 id="4-operating-system-operations">4. Operating System Operations</h2>
<p>컴퓨터가 켜질 때 <strong>부트스트랩 프로그램</strong>이 하드웨어를 초기화하고 운영체제를 메모리에 로드.
운영체제 <strong>커널</strong>이 실행을 시작하며 서비스와 데몬을 실행.
시스템에 할 일이 없으면 운영체제는 이벤트(인터럽트)를 기다림.
하드웨어/소프트웨어 문제나 요청이 발생하면 <strong>운영체제가 이를 처리</strong>.</p>
<h3 id="41-multiprogramming">4.1 Multiprogramming</h3>
<ul>
<li><strong>정의</strong>: 여러 <strong>프로그램을 동시에 메모리에 유지</strong>하여 CPU가 항상 실행할 작업을 가지도록 하는 방식.</li>
<li><strong>특징</strong>:<ul>
<li>CPU와 I/O 장치의 사용률을 높이기 위해 여러 프로세스를 메모리에 동시에 유지.</li>
<li>한 프로세스가 대기 상태가 되면 OS가 다른 프로세스를 실행.</li>
</ul>
</li>
</ul>
<h3 id="42-multitasking">4.2 Multitasking</h3>
<ul>
<li><strong>정의</strong>: 멀티프로그래밍의 논리적 확장으로, CPU가 빠른 속도로 여러 프로세스를 번갈아가며 실행해 사용자가 빠른 응답을 경험하도록 합니다.</li>
<li><strong>특징</strong>:<ul>
<li>여러 프로세스가 동시에 실행 대기 중일 때 CPU 스케줄링을 통해 관리.</li>
<li>가상 메모리로 물리 메모리보다 큰 프로그램 실행 가능.</li>
</ul>
</li>
</ul>
<h3 id="43-dual-mode-and-multimode-operation">4.3 Dual-Mode and Multimode Operation</h3>
<ul>
<li><strong>정의</strong>: 운영 체제가 자신과 다른 시스템 구성 요소를 보호하기 위해 사용자 모드와 커널 모드를 제공합니다.</li>
<li><strong>특징</strong>:<ul>
<li><strong>모드 비트</strong>: 사용자 모드(1)와 커널 모드(0)를 구분.</li>
<li>시스템 호출 시 커널 모드로 전환, 호출 종료 시 사용자 모드로 복귀.</li>
<li>일부 명령어는 커널 모드에서만 실행 가능.</li>
<li>최신 CPU는 가상 머신 관리자(VMM) 모드 등 다중 모드 지원.</li>
</ul>
</li>
</ul>
<p><strong>Transition from User to Kernel Mode</strong></p>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/3a3c5beb-1329-40ce-adac-00e3758286a1/image.png" /></p>
<p>사용자 모드에서 실행되는 프로그램이 시스템 호출 등을 통해 커널 모드로 전환되는 과정.</p>
<p>: 시스템 호출시 방해로부터 유저 프로그램을 막기위해 사용</p>
<ul>
<li><strong>작동</strong>:<ul>
<li>시스템 호출 발생 시 커널 모드로 전환하여 안전하게 운영 체제 작업 수행.</li>
<li>완료 후 사용자 모드로 복귀하여 프로세스가 계속 실행.</li>
</ul>
</li>
</ul>
<h3 id="45-timer">4.5 Timer</h3>
<p>프로세스가 무한 루프에 빠지거나 자원을 독점하지 못하도록 방지하는 장치.</p>
<ul>
<li>일정 시간 후 인터럽트를 발생시키도록 타이머를 설정합니다.</li>
<li>타이머가 0이 되면 인터럽트를 발생시켜 OS로 제어를 넘깁니다.</li>
</ul>
<hr />
<h2 id="5-resource-management">5. Resource Management</h2>
<h3 id="51-process-management-프로세스-관리"><strong>5.1 Process Management (프로세스 관리)</strong></h3>
<ul>
<li><strong>프로세스</strong>: 실행 중인 프로그램으로, 시스템 내 작업의 단위입니다. 프로그램은 수동적이지만, 프로세스는 능동적입니다.</li>
<li><strong>자원 필요</strong>: CPU 시간, 메모리, I/O 장치, 파일 등 프로세스가 작업을 수행하는 데 필요한 자원.</li>
<li><strong>프로세스 종료</strong>: 사용된 자원을 해제하고 시스템에 반환.</li>
<li><strong>단일 스레드 프로세스</strong>: 하나의 프로그램 카운터를 통해 순차적으로 명령을 실행.</li>
<li><strong>다중 스레드 프로세스</strong>: 각 스레드마다 프로그램 카운터를 가짐.</li>
<li><strong>동시성 관리</strong>: 여러 프로세스와 스레드가 CPU를 공유하여 동시에 실행.</li>
</ul>
<h3 id="52-memory-management-메모리-관리"><strong>5.2 Memory Management (메모리 관리)</strong></h3>
<ul>
<li><strong>기능</strong>: 프로세스를 생성 및 삭제, 프로세스 스케줄링, 프로세스 동기화 및 통신 제공.</li>
<li><strong>메모리 관리 활동</strong>:<ul>
<li>현재 사용 중인 메모리 부분과 각 프로세스가 사용하는 메모리 추적.</li>
<li>필요할 때 메모리 할당 및 해제.</li>
<li>프로세스와 데이터를 메모리에 적절하게 배치하여 CPU 사용률 최적화.</li>
</ul>
</li>
</ul>
<h3 id="53-file-system-management-파일-시스템-관리"><strong>5.3 File-System Management (파일 시스템 관리)</strong></h3>
<ul>
<li><strong>기능</strong>: 물리적 저장소를 논리적 파일 단위로 추상화하여 사용자에게 제공.</li>
<li><strong>활동</strong>:<ul>
<li>파일과 디렉토리 생성 및 삭제.</li>
<li>파일과 디렉토리를 조작하는 기본 기능 제공.</li>
<li>파일을 대용량 저장소에 매핑.</li>
<li>파일 백업 및 접근 제어.</li>
</ul>
</li>
</ul>
<h3 id="54-mass-storage-management-대용량-저장소-관리"><strong>5.4 Mass-Storage Management (대용량 저장소 관리)</strong></h3>
<ul>
<li><strong>주요 저장 매체</strong>: HDD와 비휘발성 메모리(NVM).</li>
<li><strong>관리 활동</strong>:<ul>
<li>장치의 마운트 및 언마운트.</li>
<li>여유 공간 관리 및 저장 공간 할당.</li>
<li>디스크 스케줄링, 파티셔닝, 보호 등.</li>
<li>느린 저장 장치(CD, DVD 등)도 관리 필요.</li>
</ul>
</li>
</ul>
<h3 id=""></h3>
<h3 id="55-cache-management-캐시-관리"><strong>5.5 Cache Management (캐시 관리)</strong></h3>
<ul>
<li><strong>캐싱</strong>: 느린 저장소의 데이터를 빠른 저장소(캐시)로 복사하여 성능 향상.</li>
<li><strong>작동 원리</strong>:<ul>
<li>데이터를 캐시에서 우선 확인 후 사용.</li>
<li>캐시 크기와 교체 정책 등 관리 문제.</li>
</ul>
</li>
</ul>
<h3 id="56-io-systems-management-입출력-시스템-관리">5.6 <strong>I/O Systems Management (입출력 시스템 관리)</strong></h3>
<ul>
<li><strong>목적</strong>: 하드웨어 장치의 복잡성을 사용자에게 숨김.</li>
<li><strong>활동</strong>:<ul>
<li>I/O 메모리 관리(버퍼링, 캐싱, 스풀링).</li>
<li>일반적인 장치 드라이버 인터페이스 제공.</li>
<li>특정 하드웨어 장치에 대한 드라이버 관리.</li>
</ul>
</li>
</ul>
<br />
피드백 환영입니다 ⭐️