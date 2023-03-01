# pintos
pintos project for operating system lecture<br>
<br>
project 0: pintos 설치<br>
<br>
<br>
<h2><b>project 1:  기본적인 시스템콜 구현하기</b><h2>
<br>
1.	Argument Passing
호출할 시스템 콜이름 매개변수(파일명이나 옵션 등) 등을 파싱하여 스택에 쌓아 저장하여, 시스템콜을 실행할 수 있게 하는 기능.<br>
스택의 top에 해당하는 esp를 이용하여, 문자열을 파싱하여 나온 argument들을 스택에 쌓습니다.<br>
<br>
2.	User Memory Access<br>
유저가 os를 거치지 않고 메모리에 직접 접근하거나, 사용할 때에 프로세스들이나 커널과 충돌을 발생시킬 수 있습니다. 또는, null pointer나 아직 할당되지 않은 메모리에 접근하는 문제도 발생할 수 있습니다. 가상 메모리를 사용하여 이러한 문제를 완화할 수 있습니다.<br>
<br>
핀토스에서는 커널 메모리와 프로세스별 유저 메모리로 구분하여 사용합니다.<br>
이때, 메모리를 가상화 하지 않고 직접적으로 사용할 경우에는 널 포인터를 참조하거나 서로의 영역을 침범하거나 메모리가 오염되거나 심하면 os가 손상되는 위험성이 발생 할 수 있습니다.<br>
<br>
가상 메모리를 사용할 때의 또다른 장점은 각 프로세스가 전체 메모리를 사용하고 있는 것 처럼 동작시킬 수 있다는 점입니다.<br>
<br>
내부 함수인 is_user_vaddr, is_kernel_vaddr 등을 사용하면 해당 주소가 유저가 사용해도 되는 메모리인지 여부등을 확인 할 수 있습니다
<br>
3.	System Calls<br>
위의 가상 메모리 개념에 의해 유저와 커널의 영역이 구분되고 서로 사용할 수 없다면 유저 프로그램은 커널의 핵심 기능을 이용할
수 없게 될 것 입니다. ( 커널의 api, 메모리, 함수 등)<br>
그러한 부분을 해결하기위해서 커널에 접근가능한 커널 모드로 들어가게 해 주는 중간 단계가 system call입니다. <br>
이를 통하여 프로세스 스케쥴, 파일 시스템 접근, 커널 메모리 접근 등의 커널의 핵심 기능을 이용할 수 있습니다. <br>
<br>
project 1에서는 halt, exit, exec, wait, read, write를 구현하였습니다.<br>
<br>
시스템콜의 내부적인 로직:<br>
유저레벨에서 스택에 정보를 쌓은 후에 시스템 콜 api를 호출하게 되면 syscall1,2,3.. 이 호출됩니다.(각각 인수 1,2,3,4..개를 처리가능)<br>
인자등의 정보가 담긴 스택의 정보가  infr_frame* f 에 담겨 intr_handler(인터럽트 핸들러)로 가게 됩니다. <br>
거기서 유효한 메모리인지, 몇번째 시스템콜을 호출하는 것인지를 알아냅니다. <br>
스택의 쌓인 정보(시스템콜 번호, 인자의 갯수, 인자)들을 기반으로 kernel api 가 실행되고, 그것의 결과값이 위의 intr_frame의 멤버인 eax에 저장되어서 다시 유저에게로 결과값을 알려주게 됩니다.<br>

