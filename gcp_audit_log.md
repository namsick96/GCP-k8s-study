# GCP Logging

## GCP Loggging란

GCP Logging은 GCP의 로그 관리 시스템으로 GCP 리소스 로그를 자동으로 수집해서 다른 곳으로 라우팅 해주거나, GCP 내 버킷에 저장해 GCP 환경에서 일어난 일들을 파악할 수 있도록 도와준다.

GCP 로그 데이터의 데이터 사이클은 다음과 같다.
![routing-overview-17-09-21](https://github.com/namsick96/GCP-k8s-study/assets/61309514/4057b75a-5067-4d88-b095-8e594f736505)

## GCP Log 활용하기

### GCP Log 분석
[로그 탐색기](https://cloud.google.com/logging/docs/view/logs-explorer-interface?hl=ko)를 활용해 빅쿼리 표준 SQL로 데이터를 쿼리할 수 있음.
빅쿼리에 로그 데이터를 연결된 데이터세트로 만들면 빅쿼리를 활용해서 분석할 수 있다. BigQuery에서 Analytic Hub를 통해 데이터를 BigQuery에 연결할 수 있다. 여기서 GCP Log를 Linked Dataset으로 지정하여 BigQuery에서 쿼리할 수 있다.

### GCP Log 모니터링 
GCP 로그 기반으로 사용자 정의 알람, 오류 메세지 발송을 설정할 수 있음.

또한 로그 심각도 및 오류를 시각화하는 대시보드를 GCP는 제공한다. 또는 로그를 외부로 라우팅해 모니터링 SaaS를 통한 모니터링 시스템을 구축하거나 Prometheus같은 툴을 이용해 직접 모니터링 시스템을 구축할 수 있다.



### GCP Log 저장
기본적으로 GCP 로그는 Cloud Logging 로그 버킷에 저장을 한다. _Required, _Default 로그 버킷이 기본으로 주어지고 다른 사용자 정의 버킷을 만들 수 있다. _Required 버킷에는 관리자 활동 감사 로그, 시스템 이벤트 감사로그, 엑세스 투명성 로그가 저장된다.

_Default 버킷은 데이터 엑세스 감사 로그, 정책 거부 감사로그가 저장된다.

사용자가 정의한 Cloud Storage 버킷에 저장할 수도 있다.

GCP 로그를 다른 GCP 프로젝트로 라우팅 할 수 있다. 해당 프로젝트로 로그를 라우팅 하면 대상 프로젝트의 로그 라우터가 로그를 수신하고 처리한다.

Pub/Sub Topic을 통해 메세지 큐 안에 로그 저장이 가능하다 (일종의 Kafka)

BigQuery 데이터셋으로 저장할 수 있다. 이 경우 데이터 분석에 용이하다.

## GCP Audit Log
GCP Audit Log는 GCP 리소스 내의 활동 및 엑세스에 대한 정보를 제공함. 즉 누가 언제 어디서 무엇을 했는지 알 수 있음. 이러한 활동 및 엑세스에 대한 정보를 바탕으로 보안, 감사, 규정 항목에 대한 모니터링을 할 수 있음. -> 누가 헛짓거리 했는지 알 수 있음. 과금된것도 알 수 있음.

GCP 감사로그(Audit Log)는 다른 로그와 달리 protopayload 필드가 있다는 거임. 여기 안에 AuditLog 객체가 있음.

감사 로그는 크게 4가지로 나뉨.

관리자 활동 감사로그
> API 호출 및 메타데이터 변경, IAM 권한 변경 등등 사용자가 GCP에서 관리자로써 진행하는 작업을 기록함. 로그 수집 중단되지 않음.

데이터 엑세스 감사 로그
> 메타데이터 읽기, API호출, 리소스 데이터 생성,수정, 읽기 등등 데이터 엑세스 기록 전체를 기록한다.

시스템 이벤트 감사 로그
> GCP 내부의 작업에 대한 로그 항목을 포함함. 로그 수집이 중단되지 않음.

정책 거부 감사 로그
> 보안 관련된 로그임. 보안 정책 위반으로 GCP가 엑세스를 거부할 때 기록됨.

참고 문헌

- https://cloud.google.com/logging/docs/overview?hl=ko
- https://cloud.google.com/blog/products/devops-sre/log-analytics-in-cloud-logging-is-now-ga?hl=en
- https://docs.datadoghq.com/integrations/google_stackdriver_logging/
- https://docs.datadoghq.com/integrations/google_cloud_platform/