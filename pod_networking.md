# Pod Networking


## Pod 란

Pod 란 하나 이상의 컨테이너로 이루어진 k8s가 생성하고 관리할 수 있는 배포 가능한 가장 작은 컴퓨팅 단위이다.

## Pod 에서의 컨테이너
Pod는 일종의 논리적으로 하나의 컴퓨팅 환경이다. 컨테이너들은 스토리지와 네트워크를 공유하고 파드의 콘텐츠는 항상 함께 배치, 스케쥴 되며 공유 콘텍스트에서 실행된다. pod 내의 컨테이너는 하나의 논리 호스트에서 실행되는 것과 같다.

일반적인 케이스로, 단일 컨테이너를 싱행하는 파드의 경우에는 파드는 단순히 컨테이너를 둘러싼 wrapper이다.

여러 컨테이너를 실행하는 pod의 경우에는 리소스를 같이 공유하는 여러개의 컨테이너가 동시에 수행된다. 이런 컨테이너 그룹은 하나의 서비스 단위를 형성한다.

단일 컨테이너를 실행하는 파드. "파드 당 하나의 컨테이너" 모델은 가장 일반적인 쿠버네티스 유스케이스이다. 이 경우, 파드를 단일 컨테이너를 둘러싼 래퍼(wrapper)로 생각할 수 있다. 쿠버네티스는 컨테이너를 직접 관리하는 대신 파드를 관리한다.

OOP의 관점에서 보았을 때 이러한 한 pod에 연관된 서비스들이 모여있는 것을 Encapsulation을 구현한 형태라고 볼수 있다.

<img width="589" alt="425316801bc6981900d8bc290d29c369227b5c48" src="https://github.com/namsick96/GCP-k8s-study/assets/61309514/9870feb7-3564-4fa5-9df3-4e242228d36e">


pod의 컨테이너 그룹은 하나의 네트워크 네임스페이스를 공유한다. 즉 하나의 IP, MAC 주소를 공유함. 즉 OSI 7계층에서 데이터 링크, 네트워크 계층을 k8s가 구현함.

pod는 주소 대역에서 고유한 ip 주소를 배정 받고 port를 공유

노드 내의 모든 pod와 노드 상의 에이전드 (kublet, daemon)은 서로 통신할 수 있다.

pod내 컨테이너 끼리 통신은 localhost를 통해 통신이 가능하다. 단 port로 컨테이너 구분을 함. 하나의 가상 네트워크 인터페이스를 공유하기 때문임.

이러한 pod 내 컨테이너 통신은 루프백 방식으로 구현됨.(루프 백 방식이란 패킷을 외부에 전송하지 않고 자신에게 주는 것 처럼 하여 상위 계층으로 올려보내는 방식. 127.0.0.1)

<img width="502" alt="스크린샷 2023-06-28 오후 7 39 12" src="https://github.com/namsick96/GCP-k8s-study/assets/61309514/057b0268-e6e6-4d2b-9cd3-2952bd667319">


다른 pod의 컨테이너와 통신하려면 ip 주소를 가지고 통신해야함.

또한 컨테이너 끼리는 SystemV 세마포어, POSIX Shared Memory를 통해 서로 통신할 수 있다.(마치 프로세스 처럼)

쿠버네티스 네트워크 모델은 각 노드의 컨테이너 런타임에 의해 결정됨. 보통 CNI 플러그인 사용. CNI란 일종의 네트워크 표준임.

k8s 네트워크 초기 Proposal 주소
https://github.com/kubernetes/design-proposals-archive/blob/main/network/networking.md

서비스, 인그레스를 통해 pod 내의 어플리케이션을 외부에 노출 시킬 수 있음.

참고 문헌
- https://kubernetes.io/ko/docs/concepts/workloads/pods/
- https://www.alibabacloud.com/blog/understanding-kubernetes-from-the-perspective-of-application-development_597457
- https://kubernetes.io/ko/docs/concepts/cluster-administration/networking/
- https://medium.com/finda-tech/kubernetes-네트워크-정리-fccd4fd0ae6