바이너리를 분석하는 분야인 리버스 엔지니어링은 그 거대한 세계의 동작 자체를 이해하는 작업을 수행한다. 
시스템 해커는 컴퓨터의 언어로 작성된 소프트웨어에서 취약점을 발견해야 하기 때문에 컴퓨터 언어를 잘 이해할 수록 보안 취약점을 찾고, 공격하여 시스템을 장악하는 것이 수월해진다. 
어셈블리어는 컴퓨터 아키텍처별로 다른 문법을 가지고 있다. 

### x86-64 어셈블리어
프로그래밍 언어도 문법 구조를 가지고 있다. 이들의 문장은 동사에 해당하는 명령어와 목적어에 해당하는 피연산자로 구성된다. 피연산자는 해당 연산의 입력 값(레지스터, 상수, 메모리 주소 등)이 된다. 
이때 피연산자에 올 수 있는 경우의 수는 매우 다양한데, 그 이유는 운영체제가 발전함에 따라 다룰 수 있는 메모리의 크기가 점차 확장됐기 때문에 예전 명령어를 호환하기 위해 1바이트, 2바이트, 4바이트, 8바이트를 구분하여 피연산자를 정의한다. 

- add 덧셈
- sub 뺄셈
- mul, imul 곱셈
- div, idiv 나눗셈

### 논리 연산
논리 연산(비트연산)은 컴퓨터가 비트 단위로 데이터를 처리할 때 사용하는 연산이다. 
and / or / xor / not

- inc - 1 증가
- dec - 1 감소
- lea 명령어는 메모리에 실제 접근을 하지 않고, 유효 주소를 저장하기 위해 사용한다. 
	- 유효 주소한 연산의 대상이 되는 데이터가 저장된 위치를 말한다. 여기서 위치는 메모리 주소를 의미하며, c언어에서 포인터라고 생각하면 이해하기 쉽다. 
- cmp 명령어는 내부적으로 두 피연산자를 빼는 방식으로 플래그를 갱신한다. 뺀 값이 0이면 두 레지스터에 담긴 값이 서로 같다는 사실을 판단할 수 있다. 
- test 명령어는 내부적으로 destination과 source를 and 연산한 뒤, 결과는 버리고 플래그 레지스터만 갱신한다. 
- 어셈블리어에서 레이블은 특정 주소에 이름을 붙여서 사용할 수 있게 하는 기호이다. 특정 루틴의 시작 지점, 함수의 시작 지점을 가리킬 때 사용한다. 레이블을 사용하면 어셈블리어 코드의 규모가 거대 해질 때 특정 레이블로 코드를 분리시킬 수 있어 분기점을 사용하거나 함수를 선언할 때 많이 사용된다. 
- jmp 명령어는 특정 주소로 rip를 바꾼다. 
- je / jz 는 직전에 비교한 두 피연산자가 같으면 점프한다. 

### 함수
함수는 어셈블리어에서 프로그램이 처리해야 할 명령어들을 한 덩어리로 모아 놓은 코드 블록이다. 

### 스택 관련 명령어
- 스택은 자료 구조의 한 종류로, LIFO 방식으로 동작하는 구조이다. 즉, 가장 나중에 삽입된 데이터가 가장 먼저 꺼내지는 구조를 가지고 있다. 리눅스나 윈도우 프로세스의 메모리에는 스택 영역이 존재한다. 스택 자료구조에 따라 데이터를 임시로 저장하고 관리하는 데 사용하는 메모리 영역이며, 특히 함수 호출 및 복귀, 인자 전달, 함수의 지역 변수 저장 등에 활용된다. x86 아키텍처의 스택은 높은 메모리 주소에서 낮은 메모리 주소로 자라는 특징이 있다. 즉, 데이터를 추가할 때마다 메모리 주소가 감소하며, 데이터를 제거하면 다시 증가한다. 
- 스택에서 가장 중요한 요소는 스택 포인터 레지스터인 rsp 이다. rsp는 항상 스택의 가장 위를 가리키며, push 명령어를 실행하면 rsp가 감소하면서 스택에 값이 저장된다. 

### 함수 호출 및 반환 관련 명령어
함수를 부르는 행위를 호출이라고 부르며, 함수에서 돌아오는 것을 반환이라고 부른다. 함수를 호출할 때는 함수를 실행하고 나서 원래의 실행 흐름으로 돌아와야 하므로, call 다음의 명령어 주소를 스택에 저장하고 함수로 rip를 이동시킨다. 
- call : 함수를 호출할 때 사용
- leave : 프로시저가 반환되기 전, 스택프레임을 정리
- ret : 함수에서 반환하여 원래 실행하던 코드로 돌아오는 명령어

- 어셈블리어에서의 함수 선언
	1. 함수 시작 위치를 나타낼 라벨(Label)을 정의
	2. 스택 프레임이 필요한 경우, 함수 프롤로그를 통해 스택 프레임을 구성
	3. 함수 내부에서 실제 동작을 구현한다. 
	4. 함수 마지막에 함수 에필로그를 통해 스택 프레임을 해제하고, ret 명령어로 종료한다. 

### opcode: 시스템 콜
현대 운영체제는 크게 사용자 모드와 커널 모드로 나뉘어 동작한다. 
- 커널 모드는 운영체제가 전체 시스템을 제어하기 위해 시스템 소프트웨어에 부여하는 권한이다. 커널 모드에서는 모든 메모리 영역에 접근할 수 있고, 하드웨어에 직접적으로 접근할 수 있다. 
- 사용자 모드는 운영체제가 사용자에게 부여하는 권한으로, 접근할 수 있는 메모리 영역과 권한이 매우 한정되어 있다. 
- 시스템 콜은 유저 모드에서 커널 모드의 시스템 소프트웨어에게 어떤 동작을 요청하기 위해 사용된다. 
	- x86에서는 int 0x86 명령어를 사용해 시스템 콜을 호출할 수 있다. 이 때 eax 레지스터에 호출하고자 하는 시스템 콜의 번호를 넣는다. 
	- x64에선 syscall 명령어를 사용해 시스템 콜을 호출할 수 있다. 
