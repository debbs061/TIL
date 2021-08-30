# DMA(직접 메모리 접근)

DMA 는 CPU의 참여 없이 입출력장치와 메모리가 직접 데이터를 주고 받는 것을 말한다.

* DMA 제어기는 작업이 끝나면 CPU에게 인터럽트 신호를 보내 작업이 종료됐음을 알림
* DMA 방식을 이용하면 CPU는 입출력 작업에 참여하지 않고 다음 명령을 계속 처리하므로, 시스템의 전반적인 속도가 향상됨



> 원래는 입출력 장치가 메모리에 접근하고 싶을 때, 반드시 cpu 에게 허락을 받아야 합니다. 하지만 cpu 는 입출력 장치 뿐 아니라 많은 일들을 처리해야 하기 때문에 그 일을 덜어주기 위해 DMA 라는 개념이 등장했습니다. DMA 는 cpu 의 참여 없이 입출력장치와 메모리가 직접 데이터를 주고 받는 것을 말합니다.