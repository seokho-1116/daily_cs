## 운영체제
- 컴퓨터 하드웨어를 관리하는 소프트웨어(중재자 역할)
- 운영체제의 근본적인 책임은 여러 자원들을 프로그램에 할당하는 것
- 사용자 <-> 응용프로그램 <-> 운영체제 <-> 컴퓨터 하드웨어
- 커널, 응용 프로그램, 시스템 프로그램과 같은 프로그램 유형 존재

## 컴퓨터 시스템의 구성
- 공통 버스를 통해 연결된 여러 장치 컨트롤러로 구성
- 장치 컨트롤러는 특정 유형의 장치 담당
  - 제어하는 주변 장치와 로컬 버퍼 저장소 간에 데이터 이동 담당
- 장치 컨트롤러마다 장치 드라이버 존재
- CPU와 장치 컨트롤러는 병렬로 실행되어 메모리 사이클을 놓고 경쟁
  - CPU <-> 장치 드라이버 <-> 장치 컨트롤러
- 메모리 컨트롤러는 메모리에 대한 액세스를 동기화

### 인터럽트
- 하드웨어는 어느 순간이든 시스템 버스를 통해 CPU에 신호를 보내 인터럽트 발생 가능
- 운영체제와 하드웨어의 상호 작용 방식의 핵심 부분
- CPU는 하던 일을 중단하고 즉시 고정된 위치로 실행을 옮기고 인터럽트를 위한 서비스 루틴을 실행
- 인터럽트 서비스 루틴이 완료되면 CPU는 중단된 지점부터 연산 재개
- 인터럽트 루틴에 대한 포인터 테이블을 이용하여 중간 루틴 없이, 테이블블 통하여 간접적으로 인터럽트 루틴 호출
- 반드시 현재 상태 저장 및 복귀할 때 상태 복원 필요

### 구현
- 인터럽트 요청 라인이 존재
- 하나의 명령어의 실행을 완료할 때마다 CPU가 이 선을 감지
- 감지 시, 인터럽트 번호 읽고 이 번호를 인터럽트 벡터의 인덱스로 사용하여 인터럽트 핸들러 루틴 접근
- 장치 컨트롤러가 인터럽트 요청 라인에 신호 선언(인터럽트 발생) -> CPU가 인터럽트 포착 -> CPU가 인터럽트 핸들러로 디스패치 -> 핸들러는 장치를 서비스하고 인터럽트 제거
- 최신 운영체제에서는 더욱 정교한 인터럽트 처리 기능이 필요
  - 인터럽트 처리 연기
  - 적절한 인터럽트 핸들러로 효율적인 디스패치
  - 우선순위에 따른 처리
- 인터럽트 벡터의 주소 개수보다 많은 장치가 존재
  - 인터럽트 체인
  - 인터럽트 벡터의 각 원소 -> 인터럽트 핸들러 리스트의 헤드
  - 인터럽트 발생 시 인터럽트 벡터의 헤더를 순회하면서 처리할 수 있는 핸들러 발견
 
### 요약
인터럽트는 최신 운영체제에서 비동기 이벤트를 처리하기 위해 사용

### 저장장치 구조
- CPU는 메모리에서만 명령을 적재할 수 있으므로 프로그램을 먼저 메모리에 적재해야 함
- 가장 먼저 실행되는 프로그램은 부트스트랩 프로그램 -> 운영체제 적재
- 폰 노이만 구조에서 명령-실행 사이클은 메모리로부터 명령을 인출해, 그 명령을 명령 레지스터에 저장하고 명령을 해독 후 피연산자를 인출하여 내부 레지스터에 저장한 후 명령을 실행하고 다시 메모리에 저장됨
- 크기가 작고 휘발성이기에 메모리만으로 모든 데이터를 저장할 수 없음 -> 보조저장장치 필요
- 저장 용량 및 액세스 시간에 따라 계층 구조로 구성될 수 있음
  - 비용, 용량, 속도와 같은 모든 요소의 균형을 맞추는 것이 중요
  - 레지스터 -> 캐시 -> 메인 메모리 -> 비휘발성 메모리 -> 하드 디스크 드라이브 -> 광학 디스크 -> 자기 테이프

### 입출력 구조
- 운영체제 코드의 상당 부분은 시스템의 안정성과 성능에 대한 중요성과 장치의 다양한 특성으로 인해 I/O 관리에 할애됨
- 비휘발성 저장장치에 대한 대량 데이터 이동은 높은 오버헤드를 유발함 -> DMA(Direct Memory Access)로 해결
  - DMA는 CPU 개입 없이 메모리로부터 또는 메모리로 데이터 블록 전체를 전송함
  - 블록 전송이 완료되면 인터럽트 발생

## 컴퓨터 시스템 구조
### 단일 처리기 시스템
- 단일 처리 코어를 가진 하나의 CPU
- 현대 컴퓨터 시스템에서는 거의 없음

### 다중 처리기 시스템
- 단일 코어 CPU가 있는 두 개 이상의 프로세서가 존재
- N 프로세서의 속도 향상 비율은 N이 아님 -> 여러 프로세서가 협력할 때 올바르게 작동시키기 위한 것과 공유 자원에 대한 경합이 오버헤드 발생 시킴
- SMP(Symmetric MultiProcessing) -> 각 피어 CPU 프로세서가 운영체제 기능 및 사용자 프로세스를 포함한 모든 작업을 수행
  - 장점은 많은 프로세스를 동시 실행 가능
  - CPU가 독립적이기 때문에 하나는 유휴 상태이고 다른 하나는 과부하가 걸릴 수도 있음
- 다중 코어 시스템 -> 하나의 CPU에 두 개 이상의 코어
  - 각 코어는 자체 레지스터 세트와 L1 캐시 존재
  - L2 캐시는 코어에서 공유
- 다중 처리기 시스템에서 CPU를 추가하는 것만이 정답은 아님
  - 시스템 버스에 대한 경합이 병목 현상이 되어 성능이 저하되기 시작함
  - 각 CPU에 작고 빠른 로컬 버스를 통해 액세스 되는 자체 로컬 메모리를 제공하는 것이 해결책
  - 모든 CPU가 공유 시스템 연결로 연결되어 하나의 물리 주소 공간 공유 -> NUMA(Non-Uniform Memory Access)
  - CPU가 시스템 상호 연결을 통해 원격 메모리에 액세스해야 할 때 지연 시간이 증가하여 성능 저하 발생
- 클러스터형 시스템
  - 둘 이상의 독자적 시스템 또는 노드들을 연결하여 구성
  - 높은 가용성을 제공함

## 운영체제의 작동
- 부팅 시 -> 일반적으로 부트스트랩 프로그램은 컴퓨터 하드웨어 내에 펌웨어로 저장됨
  - 운영체제 커널을 찾아 메모리에 적재 필요
  - 커널이 실행되는 전체 시간 동안 실행되는 시스템 데몬이 되기 위해 부팅할 때 메모리에 적재되는 시스템 프로그램에 의해 커널 외부에서 제공됨
- 시스템의 모든 측면을 초기화함

### 다중 프로그래밍과 다중 태스킹
- 다중 프로그래밍은 CPU가 항상 한 개의 프로그램을 실행할 수 있도록 구성
  - 실행 중인 프로그램 -> 프로세스
  - 여러 프로세스를 동시에 메모리에 유지해야 함
  - 일부 작업이 완료되기를 기다려야 할 수 있음
- 다중 태스킹
  -  CPU가 여러 프로세스를 전환하며 프로세스 실행
  -  빠른 응답 시간 제공
- 여러 프로세스를 메모리에 유지하려면 메모리 관리 방식 필요
- 여러 프로세스를 실행하려면 CPU 스케줄링 필요
- 가상 메모리를 이용하여 프로세스의 일부만 적재해도 실행 가능하게 하도록 하는 기법
- 파일 시스템, 저장장치 관리, 자원 보호, 프로세스 동기화, 통신 기법 제공, 프로세스 간 교착상태 방지 등이 필요함

### 이중-모드와 다중모드 운용
- 운영체제는 사용자와 소프트웨어, 하드웨어를 공유하기에 부적절하거나 잘못된 사용으로부터의 보호해야 함
- 하드웨어 자원을 통해 실행 모드를 차별화 할 수 있도록 제공함 -> 사용자 모드와 커널 모드로 구분
  - 모드 비트가 컴퓨터의 하드웨어에 추가됨
- 시스템 콜과 같은 운영체제 서비스 수행에는 커널 모드가 필요
- 특권 명령은 커널 모드에서만 수행되도록 함
  - 초기 제어, I/O 제어, 타이머 관리 및 인터럽트 관리 등
- 가상화를 지원하는 CPU는 가상화에 대한 더 많은 권한을 위해 더 다양한 모드가 존재
- 시스템 콜은 사용자 프로그램이 운영체제를 대신하여 운영체제가 수행하도록 지정되어 있는 작업을 운영체제에 요청할 수 있게 해줌

### 타이머
- 운영체제가 CPU가에 대한 제어를 유지할 수 있도록 타이머를 이용함
- 지정된 시간 이후 인터럽트를 발생시킴

## 자원 관리
### 프로세스 관리
- 프로세스는 자기 일을 수행하기 위해서 여러 가지 자원을 필요로 함
- 프로그램은 수동적 개체, 프로세스는 다음 수행할 명령을 지정하는 프로그램 카운터를 가진 능동적 개체
- 한 프로세스의 수행은 반드시 순차적
- 한 프로세스는 한 시스템 내의 작업의 단위
- 운영체제가 하는 프로세스 관리
  - 사용자 프로세스와 시스템 프로세스의 생성과 제거
  - CPU에 프로세스와 스레드 스케줄하기
  - 프로세스의 일시 중지와 재수행
  - 프로세스 동기화를 위한 기법 제공
  - 프로세스 통긴을 위한 기법 제공
 
### 메모리 관리
- 프로그램 수행을 위해선 반드시 절대 주소로 매핑되고 메모리에 적재되어야 함
- 메모리 관리 기법 선택은 하드웨어 설계에 의존적임
- 운영체제가 하는 메모리 관리
  - 메모리의 어느 부분이 현재 사용되고 있으며 어느 프로세스에 의해 사용되고 있는지 추적
  - 필요에 따라 메모리 공간을 할당하고 회수
  - 어떤 프로세스들을 메모리에 적재하고 제거할 것인가를 결정

### 파일 시스템 관리
- 운영체제는 저장장치의 물리적 특성을 추상화하여 논리적인 저장 단위인 파일을 정의함
  - 파일을 물리적 매체로 매핑하여 저장장치를 통해 이들 파일에 접근
  - 파일은 파일 생성자에 의해 정의된 관련 정보의 집합체
  - 대량 저장 매체와 그것을 제어하는 장치를 관리함
- 운영체제가 하는 파일 시스템 관리
  - 파일의 생성 및 제거
  - 디렉터리 생성 및 제거
  - 파일과 디렉터리를 조작하기 위한 프리미티브의 제공
  - 파일을 보조저장장치로 매핑
  - 안정적인(비휘발성) 저장 매체에 파일을 백업

### 대용량 저장장치 관리
- 운영체제가 하는 대용량 저장장치 관리
  - 마운팅과 언마운팅
  - 사용 가능 공간의 관리
  - 저장장소 할당
  - 디스크 스케줄링
  - 저장장치 분할
  - 보호

### 캐시 관리
- 정보가 사용되면 더 빠른 장치인 캐시에 일시적으로 보관됨
- 캐시 조회 -> 없으면 메인 메모리 조회 -> 가져와서 캐시에 저장
- 캐시 크기는 제한되어 있으므로 캐시 관리는 중요한 설계 문제
- 캐시 크기와 교체 정책을 신중하게 선택하면 성능이 크게 향상될 수도 있음
- 캐시 -> CPU 또는 레지스터는 운영체제 간섭없이 하드웨어적으로 이루어짐
- 디스크 <-> 메모리는 운영체제에 의해 제어됨
- 다중 태스킹 환경에서는 여러 프로세스가 가장 최신의 특정 값일 얻을 것을 보장하기 위해선 극도의 주의가 필요함 -> 캐시 일관성 문제

### 입출력 시스템 관리
- 특정 하드웨어 장치의 특성을 숨김
- 장치 드라이버만이 자신에게 지정된 저장장치의 특성을 알고 있음
- 입출력 시스템 구성 요소
  - 버퍼링, 캐싱, 스풀링을 포함한 메모리 관리 구성요소
  - 일반적인 장치 드라이버 인터페이스
  - 특정 하드웨어 장치들을 위한 드라이버
