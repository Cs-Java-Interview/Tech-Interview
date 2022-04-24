## 가상 메모리

메모리 관리 기법의 하나로서 기계에 실제로 이용 가능한 기억자원을 이상적으로 추상화하여 사용자들에게 매우 큰 메모리로 보이게 만드는 것을 말한다. 각 프로그램에 실제 메모리 주소가 아니라 `가상의 메모리 주소`를 부여하는 방식이다.

이러한 방식은 `멀티태스킹 운영체제`에서 흔히 사용되며, 실제 주기억장치보다 큰 메모리 영역을 제공하는 방법으로 사용된다.

가상적으로 주어진 주소를 `가상 주소` 또는 `논리 주소`라고 하며, 실제 메모리 상에서 유효한 주소를 `물리 주소` 또는 `실 주소` 라고한다. 가상 주소의 범위를 `가상주소공간`, 물리 주소의 범위를 `물리주소공간`이라고 한다.

`가상주소공간`은 `메모리 관리 장치(MMU)`에 의해서 물리주소로 변환된다. 이 덕분에 프로그래머는 가상주소공간 상에서 프로그램을 짜기 때문에 프로그램이나 데이터가 주 메모리 상에서 어떻게 존재하는지를 의식할 필요가 없다. 

가상 메모리는 크게 나누어 `세그먼트(segement)`방식과 `페이징(paging)`방식의 2종류가 존재한다.