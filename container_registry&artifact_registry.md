# Container Registry

## Container Registry란?
Container Registry란 설치할 수 있는 컨테이너 이미지를 모아둔 곳으로 컨테이너 이미지를 손쉽게 저장, 관리, 공유하는 서비스이다. 즉, 컨테이너 이미지와 Open Container Initiative 표준 아티팩트를 Container Registry에서 관리할 수 있다. Helm 차트도 저장이 가능하다.

(OCI는 컨테이너와 컨테이너 이미지의 업계 표준을 의미함)


## Background & 장점
컨테이너 환경이 폭발적으로 증가하면서 다양한 이미지를 손쉽게 관리해야할 필요성이 증대됨. 컨테이너 레지스트리는 컨테이너 이미지를 한 곳에서 저장하여 중앙집중화된 관리를 할 수가 있음.

보안상의 이점이 존재함. 중앙에서 IAM 리소스 기반 정책을 사용해 리포지토리별, 이미지별 접근 권한을 설정하고 설정한 권한에 따라 이미지를 사용할 수 있음.

컨테이너 레지스트리를 사용하면 오브젝트 스토리지와 연계되어 이미지를 보관하고 불러올 수 있어 효과적인 이미지 관리가 가능함. (Hot-Warm-Cold로 분리)

이미지에 대한 형상관리, 수명주기 관리 버저닝(태그 기능) 제공을 통해 좀더 쉬운 배포가 가능해짐.

CI/CD 파이프라인을 통해 자동화된 배포를 구축할 수 있음.

컨테이너에 대한 이벤트 모니터링, 이벤트 로깅이 가능해 컨테이너 레지스트리에 저장된 아티팩트의 상태, 접속기록등을 확인할 수 있다.

캐싱을 통해 좀 더 빠른 컨테이너 이미지 pull push가 가능해짐

rest, rpc 타입의 api를 제공해서 프로그래머틱하게 서비스 사용 가능. gcloud에서도 사용 가능.

마지막으로 저장된 컨테이너의 이미지 분석을 통해 보안 취약점 파악가능.

# Artifact Registry

## Artifact Registry란?

Artifact Registry란 Container Registry의 확장된 개념임. 컨테이너 말고도 언어 패키지 (java, node.js, python), OS 패키지(Debian, RPM) 형식을 저장해주기도 함.

## Background & 장점

컨테이너가 점점 de facto가 되면서 기존 Container Registry에게 요구되는 기능보다 더 많은 기능이 요구됨.

그 과정에서 Container Registry의 superset인 Artifact Registry가 등장함.

gcp의 container Registry에서는 멀티리전에 단일 레지스트리 호스트만 만들 수 있어서 글로벌이 다 같은 도메인을 사용했음 (gcr.io). 추후에 아시아, us, eu로 늘어나긴 함. 그런데 artifact registry는 모든 리전에 존재함.

Container Registry보다 더 세밀한 권한 설정이 가능하다. Container Registry의 경우 멀티리전 레지스트리 호스트에 대한 권한을 부여하며 저장소 수준까지는 권한 부여 세밀화가 안됨. 하지만 Artifact Registry는 가능하다.

GKE에서의 이미지 스트리밍이 가능함. GKE 1.25버전 이상의 Autopilot에서 가능함. 이미지 스트리밍이란 컨테이너 이미지를 스트리밍 방식으로 가져오는것임. 컨테이너 이미지가 다 다운로드 되기까지 안기다려도 되어서 좀더 빠른 자동 확장, 대용량 이미지 다운로드 시간 감소, 기민한 pod 시작이라는 장점이 있음. 



# 참고문헌
- https://cloud.google.com/container-registry/docs/overview?hl=ko
- https://cloud.google.com/container-registry/docs/overview?hl=ko
- https://docs.aws.amazon.com/ko_kr/AmazonECR/latest/userguide/what-is-ecr.html
- https://docs.aws.amazon.com/ko_kr/AmazonECR/latest/userguide/LifecyclePolicies.html
- https://learn.microsoft.com/en-us/azure/devops/pipelines/ecosystems/containers/acr-template?view=azure-devops
- https://medium.com/google-cloud/artifact-registry-the-new-way-to-keep-your-app-artifacts-and-docker-images-on-gcp-d1a72da09ff9
- https://velog.io/@calmlake79/Container-Registry-를-대체할-Artifact-Registry