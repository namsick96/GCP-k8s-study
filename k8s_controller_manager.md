# k8s controller manager

## k8s Controller Manager란
k8s control plane에 존재하는 컨트롤러 프로세스를 실행하는 컴포넌트이다.

## K8s Controller란?

컨트롤러란 쿠버네티스 클러스터 상태를 관찰 한 후 필요한 경우에 생성 또는 변경을 요청한다. 각 컨트롤러는 현재 클러스터 상태를 의도한 상태에 가깝게 유지시킨다. 즉, 다운된 노드가 없는지, 파드가 의도한 복제 숫자를 유지하고 있는지, 서비스와 파드는 적절하게 연결되어있는지, 네임스페이스에 대한 기본 계정과 토큰이 생성되어잇는지를 확인하고 적절하지 않다면 적잘한 수준을 유지하도록 조치하는 역할을 가지고 있습니다.


k8s에서 컨트롤러는 문서상에서는 4가지임.
- node controller
- job controller
- endpointslice controller
- service account controller

노드 컨트롤러는 노드가 다운되었을 때 통지와 대응에 대한 책임을 가지고 있다.
잡 컨트롤러는 일회성 작업을 나타내는 잡 오프젝트를 감시하다가 해당 작업을 완료할 때까지 동작하는 파드를 생성한다.
엔드포인트슬라이스 컨트롤러는 서비스와 파드 사이의 연결고리를 제공하기 위해 엔드포인트슬라이스 오브젝트를 Populate한다. 
엔드포인트슬라이스문서 (https://kubernetes.io/ko/docs/concepts/services-networking/endpoint-slices/) 
서비스어카운트 컨트롤러는 새로운 네임스페이스에 대한 기본 서비스어카운트(ServiceAccount)를 생성한다.

사실 컨트롤러는 더 많습니다... <img width="751" alt="스크린샷 2023-06-22 오후 8 11 12" src="https://github.com/namsick96/GCP-k8s-study/assets/61309514/9b6b8182-c8a7-4a63-8589-50255ebe4abf">

이러한 컨트롤러는 개별적으로 go Routine으로 동작하며 모두 하나의 프로세스에서 동작한다.


## k8s Controller Manager의 코드레벨 동작 원리
[시작점](https://github.com/kubernetes/kubernetes/blob/release-1.24/cmd/kube-controller-manager/app/controllermanager.go#L104)
여기서 이제 Controller Manager가 시작됨.

[전체 Controller의 시작](https://github.com/kubernetes/kubernetes/blob/3e2840400880d7e698fdb390a9cb8eef08464ce9/cmd/kube-controller-manager/app/controllermanager.go#LL428C14-L428C14)

[각 컨트롤러의 시작점](https://github.com/kubernetes/kubernetes/blob/release-1.24/cmd/kube-controller-manager/app/apps.go#L58)
처음 start를 여기서 시작함.
main 함수에서 여기 있는 start~~ Controller들을 다 실행시킴. go 키워드 보이는데 이게 go 루틴임. 병렬 처리에 강점을 가짐. 경량 스레드로서 동시성 처리할 수 있고 함수를 동시에 실행하거나 백그라운드에서 실행가능. 

각 package에서 Go 메서드를 실행함. 
이때 Go 메서드에서 필요로 하는 인자들은 ControllerContext struct에서 가지고 있음. [여기](https://github.com/kubernetes/kubernetes/blob/release-1.24/cmd/kube-controller-manager/app/controllermanager.go#L335)에서 확인 가능
해당 ControllerContext에서 https://pkg.go.dev/k8s.io/controller-manager/pkg/informerfactory를 통해 resource Group에 대한 정보를 가져옴.

그리고 ClientBuilder에서 해당 controller가 필요한 client를 만들어줌. client는 api와 통신할 수 있게 해주고 config를 가지고 올 수 있게 해줌.
https://github.com/kubernetes/kubernetes/blob/release-1.24/staging/src/k8s.io/controller-manager/pkg/clientbuilder/client_builder.go

그리고 실질적인 상태에 대한 처리는 각 Controller 코드에서 진행함.
https://github.com/kubernetes/kubernetes/blob/release-1.24/pkg/controller/statefulset/stateful_set.go

참고자료
- k8s controller manager 코드 https://github.com/kubernetes/kubernetes/tree/release-1.24/cmd/kube-controller-manager