# 컴퓨팅

## ASG

- **트래픽 증가** **이미 알고 있는 급증 시간 → 예약된 ASG**
- **특정 사용률에서 성능이 가장 좋다 → 동적 조정 ASG**

### [**ASG 조정 방법](https://docs.aws.amazon.com/ko_kr/autoscaling/ec2/userguide/scaling_plan.html#scaling_typesof) ( 수동/ 동적/ 예약/ 예측 )**

- 예측(기계 학습을 사용하여 CloudWatch의 기록 데이터를 기반으로 용량 필요량을 예측)
- `Forecast` / `Analysis` / `future adaptation` = **Predictive scaling(예측적 확장)** 의 키워드

![1](https://user-images.githubusercontent.com/70079416/185668191-e454c7c7-d59c-459c-ac56-d76ddc6061e6.png)

![2](https://user-images.githubusercontent.com/70079416/185668186-a5139b9a-c20e-43ed-8d59-e41d965e5b52.png)

![3](https://user-images.githubusercontent.com/70079416/185668183-13072956-7f91-4fd5-891c-b68025440b68.png)

- **단계 &  대상 추적** 모두 **인스턴스 워밍업** 기능 제공 ↔ **단순 조정 정책**

**⇒** `**시작시` `부팅시` `실행 초기에`** **트래픽/지연 증가** → **인스턴스 워밍업 기능**을 제공하는 **단계 or 대상추적 선택! (단순 X)**

- 대상 추적 정책 `CPU 목표값 지정` `성능에 영향을 미치지 않으면서`

### **단계 확장 작업 vs 단순 조정 정책**

![4](https://user-images.githubusercontent.com/70079416/185668182-cc562d33-32eb-484f-b603-7f9195e5dac7.png)

### ASG 수명주기 후크

Auto Scaling 인스턴스 수명 주기의 이벤트를 인식하는 솔루션을 생성한 다음 해당 수명 주기 이벤트가 발생할 때 인스턴스에 대한 사용자 정의 작업을 수행할 수 있습니다.

- (1-8 예시) EC2 Auto Scaling 그룹을 통해 프로비저닝된 모든 인스턴스가 시작 및 종료되는 즉시 감사 시스템에 보고서를 성공적으로 전송하도록 해야 합니다.

- Auto Scaling : 여러 리전을 포괄할 수 없다! (3-44 A)

### 로그(CloudWatch vs CloudTrail)

**CloudWatch**는 AWS 서비스 및 리소스의 상태와 성능을 보고

- “AWS에서 무슨 일이 일어나고 있습니까?” 및 특정 서비스 또는 애플리케이션에 대한 모든 이벤트를 기록합니다.
- **SNS, EC2 Auto Scaling, CloudTrail, IAM** 감시

**CloudTrail**은 AWS 환경 내에서 발생한 모든 작업의 로그

- "AWS에서 누가 무엇을 했습니까?" 서비스 또는 리소스에 대한 API 호출.

### EKS vs ECS

- EKS : 외부 클라우드와도 완벽 호환 (온프레미스  + 클라우드 → 범용)
- ECS : AWS 내에서만 (클라우드 독점)

**ECS 시작 유형**

- Fargate 시작 유형 : 프로비저닝 X, 서버리스
- EC2 시작 유형 : EC2에서 컨테이너화된 애플리케이션
- 외부 시작 유형 : 온프레미스 서버나 가상 머신(VM)에서 컨테이너화된 애플리케이션

### Fargate

`ECS` `EKS` `서버리스` `리소스 조정` `보안 및 관리`

**1-49**

- 추가 인프라 없이 = AWS 완전관리형 서비스
- ECS/EKS → 완전관리형 서비스가 아님 → 그래서 컨테이너 자동배포 해주는 Fargate가 필요

### 전송 비용 - 동일 AZ 무료

- **동일 가용 영역**에서 Amazon EC2, Amazon RDS, Amazon Redshift, Amazon ElastiCache 인스턴스 및 Elastic Network Interface 간에 전송되는 데이터는 무료

### Savings Plan

- **EC2(72%)**가 **Compute(66%)**보다 주문형 요율이 더 적다
- 주문형 요율: 약정 사용량을 초과할 때 내는 비용

### 배치 그룹 (동일 리전)

- 클러스터 : `노드간 통신시 낮은 지연시간` `높은 네트워크 처리량` `단일 az만`
- 파티션 : `다른 파티션의 인스턴스 그룹과 기본 하드웨어 공유 X` `대규모 분산 처리(워크로드)` `다중 az 가능`
- 분산 : `장애 최소화`  `(분산 배치 그룹 간)동일 하드웨어 사용 할 수 있음` `다중 az 가능`

![5](https://user-images.githubusercontent.com/70079416/185668180-dd959202-b22c-4c09-bc74-5d6939be11ec.png)

|  | 전용 호스트 | 전용 인스턴스 | AZ |
| --- | --- | --- | --- |
| 클러스터 | X | O | 단일 AZ 만 |
| 파티션 | X | O | 다중 AZ 가능 |
| 분산 | X | X | 다중 AZ 가능 |

• 배치 그룹에서는 전용 호스트를 시작할 수 없습니다.

### 전용 호스트 VS 전용 인스턴스

- 전용 호스트 → 자사 라이선스 이용 = 다른 회사와 공유 안 함
- [**전용 호스트**](https://aws.amazon.com/ko/ec2/dedicated-hosts/)
    
    : Amazon EC2에서 Microsoft 및 Oracle 같은 공급업체의 적격 소프트웨어 라이선스를 사용할 수 있으므로, **고객이 자사의 보유 라이선스를 활용하는 유연성과 비용 효율성을 보장**받으면서 AWS의 복원력, 간편성 및 탄력성을 활용할 수 있다.
    
- [**전용 인스턴스**](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/dedicated-instance.html)
    
    : 단일 고객 전용 하드웨어의 VPC에서 실행되는 Amazon EC2 인스턴스
    

### 배스천 호스트 (1-29)

- 온프레미스 - 배스천 - 애플리케이션
- 배스천 호스트의 현재 보안 그룹을 회사의 내부 IP 범위에서 인바운드 액세스만 허용하는보안 그룹으로 교체합니다.
    - private EC2 가 접근할 수 있어야 하니까
- 애플리케이션 인스턴스의 현재 보안 그룹을 배스천 호스트의 공용 IP 주소에서만 인바운드SSH 액세스를 허용하는 보안 그룹으로 교체합니다.
    - 배스천은 외부와 통신해야하니까 공용 IP
    - Bastion - 공용 IP 세트!

### 돈 관련 서비스

- **Cost Explorer(비용 탐색기)**
    - 각 애플리케이션에 대한 비용 분석 + **보고서**
    - 세분화된 필터링 기능 : 인스턴스 유형에 따라 비용 **심층 분석**
- **Biling and Cost Management**
    - 사용자량 모니터링 도구 → 보고서 X
    - 단순 그래프를 csv 형식으로 업데이트
- **AWS Budget**
    - 비용, 사용량 추적

### KMS 암호화 객체에 접근 문제

**1-63**

Q. KMS 키로 Lambda 환경 변수 암호화. 환경 변수를 해독하는 데 필요한 권한 확인.

- Lamdba 실행 역할에 KMS 권한 추가 ⇒ KMS 사용권한을 Lambda 함수에게 부여
- KMS 키 정책에서 Lambda 실행 역할 허용

**1-69**

Q. KMS 키로 암호화되는 파일. Lambda로 S3에서 파일 다운로드하고 해독해야 함.

- KMS 키의 정책에서 Lambda IAM 역할에 대한 암호 해독 권한을 부여
- KMS 해독 권한이 있는 새 IAM 역할을 생성하고 실행 역할을 Lambda 함수에 연결

### 인스턴스 구입 옵션

- 온디맨드 `이용한만큼` `시작/종료 시 딜레이 존재`
- 스팟 `남은 공간 대여` `비용 효율` `예고치 않게 종료될 수 있음(안정성x)` `상태 비저장`
- 예약 `미리 구입` `1, 3년 약정 존재`

### 트래픽 테스트

![6](https://user-images.githubusercontent.com/70079416/185668179-199716b4-e9fc-44b8-8cc9-284414d68adf.png)

- Cloud Formation 스택 롤백
- 스냅샷/보조 옵션으로 리소스 유지

### Eventbridge

`이벤트 수집 및 알림 전송` `서버리스` `lambda 친구` `kms 키 비활성화+제거`

### Elastic beanstalk

`애플리케이션 배포` `관리형` `.NET 지원` `코드 변경 촤소화`

# 스토리지

### 스토리지 종류

![7](https://user-images.githubusercontent.com/70079416/185668176-8f544e2f-ef81-4ec9-9362-f3a891b42eea.png)

### EFS, FSx 키워드

<aside>
📢 docker 기본운영체제 = Linux
EFS도 상당히 고성능인데 이것만으로 감당이 되지 않는 고성능 스토리지가 필요 → FSx

- 웹 서버, 일반적 공유 스토리지 시나리오 → EFS
- NFS → EFS
- Linux & **대용량**, 고성능(HPC), **초고속** & 공유 스토리지 → FSx Lustre
- SMB, 공유 스토리지 → FSx Windows
</aside>

<aside>
🍀 **EFS**

- 여러 인스턴스 동시 액세스
- NFS 프로토콜 지원
- 자동 확장 축소 + 완전 관리형(운영 수고 덜어줌)
- 빅데이터 및 분산 병렬 처리 환경에서 공유 데이터 엑세스 스토리지로 활용
- 비용 효율과는 완전 무관 → 비싸!! 
- 비용 효율이랑 같이 나오려면 EFS Standard/onezone-IA (2-132)
</aside>

### Datasync 키워드

- **온프레미스 - AWS 간** 데이터 전송
- **AWS 스토리지 서비스 간** 데이터 전송(**S3, EFS, FSx**)

`데이터 복제 자동화 가속화` `데이터 보안(private)` `완전 관리형 솔루션` **`데이터 암호화 및 데이터 무결성 검증`**

`NFS 프로토콜 지원` `데이터 주기적 백업`

- 온라인 데이터 전송
- 전송 중 간헐적인 인터넷 연결 중단 처리 가능
- **NFS, SMB, S3 API, HDFS** (SFTP 제공 X) (2-111)

### S3 수명주기

![8](https://user-images.githubusercontent.com/70079416/185668172-6e5d4d34-e332-4d8a-a202-cdca4d1663a7.png)

- 비용
    - STANDARD > STANDARD-IA > ONE ZONE-IA > GLACIER

**→ 저렴한 대신에 오래 써라!**

- S3 Glacier : 최소 비용 청구 단위 **90일**
- S3 Intelligent-Tiering, One Zone-IA : **30일**
- S3 표준 : 상관 X **(30일 이하로 저장할 땐 이게 젤 비용효율적)**

### Glacier 정리

- 별다른 언급 없으면 standard로 생각하기

<aside>
📢 **S3 Glacier** 3 종류 - Instant Retrieval / Flexible Retrieval / Deep Archive

**Deep Archive**

**Instant Retrieval** : backup 인데 access it within milliseconds

**Flexible Retrieval** 

- expedited=신속 : 1~5 minutes
- **standard : 3~5 hours**
- bulk : 5~12 hours (free)
- **standard : 12 hours**
- bulk : 48 hours
</aside>

### API Gateway

- API Gateway → Lambda와 짝을 이루는 경우가 多
- 비용 효율 → 서버리스
- 스로틀(throttle) 제한 : 사용자가 특정 기간동안 수행할 수 있는 API 수(1-55 C)

### EBS 볼륨 유형

**HDD**: 주요 성능 특성이 처리량, 대규모 스트리밍 워크로드에 최적화

- **처리량 최적화 HDD** - 자주 액세스하는 처리량 집약적 워크로드에 적합한 저비용 HDD
- **콜드 HDD** - 자주 액세스하지 않는 워크로드에 적합한 가장 저렴한 HDD

**SSD**: 주요 성능 특성이 IOPS, 작은 I/O 크기의 읽기/쓰기 작업을 자주 처리하는 트랜잭션 워크로드에 최적화

- 범용 SSD — 가격과 성능 간의 균형을 제공. 대부분의 워크로드에 권장.
- Provisioned IOPS SSD — 지연 시간이 짧거나 처리량이 많은 미션 크리티컬 워크로드에 적합한 고성능을 제공.

**범용 SSD(gp3)**

광범위한 워크로드에 적합한 비용 효과적인 스토리지. 스토리지 가격에 포함된 3,000 IOPS 및 125Mib/s의 일관된 기준 속도를 제공하고, 추가 비용으로 추가 IOPS(최대 16,000) 및 처리량(최대 1,000MiB/s)을 프로비저닝할 수 있다.

****Provisioned IOPS SSD 볼륨****

스토리지 성능과 일관성에 민감한 I/O 집약적 워크로드, 특히 데이터베이스 워크로드의 요구 사항을 충족하도록 설계되었다. 프로비저닝된 IOPS SSD 볼륨은 볼륨을 생성할 때 지정한 일관된 IOPS 속도를 사용하며 Amazon EBS는 프로비저닝된 성능의 99.9%를 제공한다.

- **io1 볼륨**: 99.8~99.9%의 볼륨 내구성을 제공, 연간 장애율(AFR)은 0.2% 이하. (1년 동안 실행 중인 볼륨 1,000개당 최대 2개의 볼륨 장애가 발생)
- **io2 볼륨**: 99.999%의 볼륨 내구성을 제공, AFR은 0.001% 이하. (1년 동안 실행 중인 볼륨 100,000개당 1개의 볼륨 장애가 발생)

[ [Amazon EBS 볼륨 유형](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/ebs-volume-types.html) ]

### Direct Connect vs VPN

- 온프레 + AWS 하이브리드 환경

Direct Connect

- 온프레미스와 클라우드 간 안정적 동기화
- **온-클 데이터 센터 간** 중단없는 안정적, **속도 일정**한 데이터 전송, 연결

VPN 

- **비용 효율적**, 속도 불안정 (인터넷 사용)

## **Storage Gateway**

- 온프레미스 + 클라우드 하이브리드 환경

<aside>
🍀 **Storage Gateway 3종류**

**파일 / 볼륨(저장, 캐시) / 테이프**

**파일 게이트웨이**

- 프로토콜 : **NFS, SMB**
- 하이브리드 클라우드 워크플로의 경우 온프레미스 애플리케이션에서 생성되는 데이터를 기계 학습 또는 빅 데이터 분석과 같은 AWS 서비스로 처리할 수 있습니다.
- S3, FSx랑 같이 쓰임
- AD(Active Directory)와 통합 가능 (2-15)

**볼륨 게이트웨이**

- 블록 스토리지, EBS 스냅샷, 캐시된 볼륨 같은 말 필요
- iSCSI 블록 스토리지 볼륨을 제공
- S3에서 온프레미스 데이터를 자동으로 저장 및 관리하며, 캐시 모드 또는 저장 모드로 작동

**캐시 볼륨**

기본 데이터가 Amazon S3에 저장

하위 데이터셋 키워드

자주 액세스하는 데이터는 액세스 지연 시간을 줄이기 하기 위해 로컬로 캐시

→ 온프레미스와 클라우드 같이 사용(데이터 저장)

**저장 볼륨**

기본 데이터가 로컬로 저장. **액세스 지연 시간을 줄이기 위해** 전체 데이터 세트가 온프레미스에서 제공 → 온프레미스에 용량 부족하다 이런 말 나오면 저장 볼륨 X

S3에 비동기식으로 백업

**테이프 게이트웨이**

- 백업 용도로 데이터를 영구 저장. Glacier에 저장되는 보존용(처박아두는 느낌). 스토리지 낭비하지 않기 위한. 온프레미스 작살나면 보존시키는 용
- 프로토콜: iscsi, 가상의 테이프(VT, VTL) 키워드
</aside>

### Storage Gateway vs DataSync

<aside>
📢 데이터가 온프레미스에도 있고, 클라우드에도 있어야 한다고 판단 → **Storage Gateway**
온프레미스에서 안 쓰고 클라우드로 전부 옮긴다 → **DataSync** (왔다갔다 할 때, 주기적 동기화, 주기적 백업 NFS 프로토콜)

</aside>

### Global Accelarator = 성능 개선 & 글로벌

- 트래픽 성능 60% 개선
- 고정 IP 제공
- tcp udp외 다 됨

### Transfer Accelarator = 전세계 + 중앙집중식

- 클라이언트와 S3 버킷 간의 장거리 파일 전송

## SnowFamily

**Snowball** → 네트워크 대역폭 낮을 때, 전송중 데이터 암호화

- Snowball 디바이스는 물리적으로 견고한 디바이스로 보호됩니다. AWS Key Management Service(AWS KMS)로 전송 중 데이터를 안전하게 보호합니다.

**Snowmobile** → 몇백 TB ~ PB

### Snowball edge 최적화 디바이스

- **Snowball Edge 스토리지 최적화 디바이스 :** 블록 스토리지 및 Amazon S3 호환 객체 스토리지 모두와 40개의 vCPU를 제공합니다. 이들은 로컬 스토리지 및 대규모 데이터 전송에 적합합니다.
- **Snowball Edge 컴퓨팅 최적화 디바이스 :** 52개의 vCPU 및 객체 스토리지와 **한 개의 GPU(선택 사항)**를 제공하며, 고급 Machine Learning 및 외진 환경에서 풀 모션 비디오 분석과 같은 용도에 적합합니다. 데이터를 AWS로 다시 보내기 전에 **간헐적 연결**(제조, 산업, 운송 등) 환경 또는 매우 먼 위치(군사 또는 해양 작전 등)에서의 데이터 수집, Machine Learning 및 처리와 저장에 이러한 디바이스를 사용할 수 있습니다. 또한, 이러한 디바이스는 랙에 장착하고 함께 클러스터링하여 대규모의 임시 시설을 구축할 수 있습니다.

### Transfer Family

- 프로토콜 : **SFTP, FTPS, FTP**
- **반복적인 파일 전송**을 **S3** 및 **EFS 안팎**으로 안전하게 확장합니다. 완전 관리형.
- 직접 송수신
- 원격 직원 및 외부 파트너 직원이 쉽게 액세스 (2-79)

### 데이터 전송과 연결

- 모든 데이터를 클라우드로 마이그레이션할 시, **DataSync**
    
    + 데이터 용량이 많을 시 **snowball**(테라바이트 단위), **snowmobile**(페타바이트 단위)
    
- 온프레미스와 클라우드 간의 안정적 연결: **Direct Connect (속도일정, 안정성, 비용 비쌈)**
    
    + 비용 효율성, 속도 안정성 무관: **VPN**
    

<aside>
📌 **동일 리전 내 복제(SRR)
- 각종 로그를 단일 버킷으로 집계** — 만일, 애플리케이션 로그를 여러 버킷 또는 여러 계정에 저장하는 경우, 모든 로그를 쉽게 리전 내 단일 버킷으로 복제 할 수 있습니다. 이를 통해 단일 계정으로 로그를보다 간단하게 처리 할 수 있습니다.
- VPC 집계할 때도 쓴다.

</aside>

### EFS 수명 주기 정책

**EFS Standard & EFS Standard-IA**

리전 내에서 지리적으로 분리된 여러 가용 영역에 파일 시스템 데이터와 메타데이터를 중복 저장하여 최고 수준의 가용성과 내구성을 제공한다.

**EFS One Zone & EFS One Zone-IA**

단일 가용 영역 내의 데이터에 지속적인 가용성을 제공하도록 설계되었다. 모든 데이터 복사본에 영향을 주는 재해 또는 기타 장애가 발생하거나 가용 영역이 파괴되는 경우 이러한 스토리지 클래스에 저장된 데이터가 손실될 수 있다.

⇒ **Standard & One Zone 클래스**는 자주 액세스하는 파일에 사용한다. **IA 스토리지 클래스**는 매일 액세스하지 않는 파일의 스토리지 비용을 줄여주고 Amazon EFS가 제공하는 고가용성, 높은 내구성, 탄력성 및 POSIX 파일 시스템 액세스에 영향을 주지 않는다. 전체 데이터 세트에 손쉽게 액세스해야 하고 파일에 자주 액세스하지 않을 때 스토리지 비용을 자동으로 절감하려는 경우에 추천한다.

[ [EFS 스토리지 클래스](https://docs.aws.amazon.com/ko_kr/efs/latest/ug/storage-classes.html) ]

### S3 객체 잠금

<aside>
🔒 S3 객체 잠금은 고정된 시간 동안 또는 무기한으로 객체의 삭제 또는 덮어쓰기를 방지할 수 있다.

1. 거버넌스 모드: 일부 사용자에게만 객체 **수정 및 삭제** 권한 부여
2. 규정 준수 모드: 루트 사용자 포함 그 누구도 객체 수정 및 삭제 불가
</aside>

### S3 암호화 키 유형

<aside>
🔒 S3 암호화에는 SSE-C SSE-S3 SSE-KMS 키워드가 존재함

1. **SSE-C** : 사용자는 암호화 키를 관리하고(직접 키를 가져옴) Amazon S3는 암호화(디스크에 쓸 때) 및 해독(객체에 액세스할 때)을 관리
2. **SSE-S3** : Amazon S3 관리형 키를 사용한 서버측 암호화 (AWS에서 완벽하게 관리되고 처리)
3. **SSE-KMS** : 암호화를 하는 Key를 AWS에서 관리하는 방식의 암호화 방식(+감시 및 접근 시도에 대한 확인이 가능함)
</aside>

# 네트워크

## Route 53

<aside>
📢 DNS 응답 코드 로그 → Route 53을 사용하여 쿼리 로깅 구성 (3-56)

</aside>

<aside>
🍀 DNS 별칭 레코드 + 다른 지역 ALB 장애 조치 자동화 → **Route53 상태 확인(health check**)

</aside>

![9](https://user-images.githubusercontent.com/70079416/185668166-77d2ad3e-feb5-4d66-80cf-def7ce30d741.jpeg)

### **지리적 위치 vs 지리 근접**

- 지리적 위치 라우팅
    
    ![10](https://user-images.githubusercontent.com/70079416/185668162-5b3d3505-6cac-4dbd-887c-11c94b00f78a.png)
    
- [**지리 근접**](https://ssunw.tistory.com/entry/Route-53-%EB%9D%BC%EC%9A%B0%ED%8C%85-%EC%A0%95%EC%B1%85-%EC%A7%80%EB%A6%AC-%EA%B7%BC%EC%A0%91?category=959406)
    - 한 리전에서 다른 리전으로 트래픽 보낼 때 유용
    - **트래픽 흐름을 사용하는 경우에만 사용**할 수 있습니다.
    - 지리 근접 라우팅을 사용하면 **리소스의 위치를 기반으로 트래픽을 라우팅**하고 **필요에 따라 한 위치의 리소스에서 다른 위치의 리소스로 트래픽을 보낼 수 있습니다.**

### Route 53 레코드 타입

**A** : hostname → IPv4

**AAAA** : hostname → IPv6

**CNAME** : hostname → 다른 hostname

**ALIAS** : hostname → AWS 리소스

**NS** : Name servers for Hosted Zone (control how traffic is routed for a domain)

- **[CNAME vs ALIAS](https://docs.aws.amazon.com/ko_kr/Route53/latest/DeveloperGuide/resource-record-sets-choosing-alias-non-alias.html)**
    
    **Alias (별칭) 레코드:**
    
    - **hostname → AWS 리소스** 로 리디렉션 (app.mydomain.com → bbb.**amazonaws**.com)
        - S3 버킷, CloudFront 배포, 동일한 Route 53 호스팅 영역의 다른 레코드
    - 예를 들어, acme.example.com이라는 이름의 Amazon S3 버킷으로 쿼리를 리디렉션하는 acme.example.com이라는 별칭 레코드를 생성할 수 있습니다. [example.com](http://example.com/) 호스팅 영역의 [zenith.example.com](http://zenith.example.com/) 레코드로 쿼리를 리디렉션하는 [acme.example.com](http://acme.example.com/) 별칭 레코드를 생성할 수도 있습니다.
    - non root domain, root domain 모두 가능
    - **무료**
    - health check 가능
    
    **CNAME 레코드:**
    
    - **hostname → 다른 hostname** 으로 리디렉션 (app.mydomain.com → bbb.anything.com)
    - 예를 들어 acme.example.com에서 [zenith.example.com](http://zenith.example.com/) 또는 acme.example.org로 쿼리를 리디렉션하는 CNAME 레코드를 생성할 수 있습니다. 쿼리를 리디렉션할 도메인의 DNS 서비스로 Route 53을 사용할 필요가 없습니다.
    - non root domain에서만 가능
    - **유료**

### VPC 엔드포인트 요금 비교

✨ **게이트웨이 엔드포인트**

게이트웨이 엔드포인트는 라우팅 테이블의 경로에 대한 대상인 게이트웨이로, Amazon `S3` 또는 `DynamoDB`로 전달되는 트래픽에 사용됩니다. 게이트웨이 엔드포인트 사용에 따르는 요금은 없습니다.

✨ **NAT 게이트웨이**

VPC에 NAT 게이트웨이를 생성하는 경우 게이트웨이를 프로비저닝하고 사용하는 각 ‘NAT 게이트웨이 시간’에 대한 요금이 부과됩니다. 데이터 처리 요금은 트래픽 소스나 대상과 관계없이 NAT 게이트웨이를 통해 처리된 각 기가바이트에 적용됩니다. 1시간 미만의 각 NAT 게이트웨이 사용 시간은 1시간으로 청구됩니다. 또한 NAT 게이트웨이를 통해 전송된 모든 데이터에 대한 표준 AWS 데이터 전송 요금도 발생합니다. NAT 게이트웨이에 대한 요금이 청구되지 않도록 하려면 AWS 관리 콘솔, 명령줄 인터페이스 또는 API를 사용하여 NAT 게이트웨이를 삭제하면 됩니다.

**✨ 인터페이스 엔드포인트**

AZ별 VPC 엔드포인트당 요금(USD/시간): 0.01 USD

### CloudFront 배포 가격 등급

- **전체 가격 등급(Price Class All) > 가격 등급 200(200 CloudFront) > 가격 등급 100(100 CloudFront)**
- 가격이 저렴할수록 대기시간 길어진다.

### 보안그룹 규칙 - 소스 또는 대상

- **소스 또는 대상**: 허용할 트래픽에 대한 소스(인바운드 규칙) 또는 대상(아웃바운드 규칙)입니다. 다음 중 하나를 지정하십시오.
    - **단일 IPv4 주소**. CIDR 블록 표기법으로 표시된 IPv4 주소의 범위.
    - **단일 IPv6 주소**. CIDR 블록 표기법으로 표시된 IPv6 주소의 범위.
    - **[접두사 목록](https://docs.aws.amazon.com/vpc/latest/userguide/managed-prefix-lists.html)의 ID**.
    - **보안 그룹의 ID**입니다(여기에서는 지정폧
    

### 보안그룹 & 네트워크 ACL

![11](https://user-images.githubusercontent.com/70079416/185668153-f664ac2f-faea-4641-95c2-8b9da83e8f1d.png)

- 보안 그룹 : **Allow** only
- 네트워크 ACL : **Allow & Deny**

### VPC 엔드포인트

<aside>
📢 VPC 엔드포인트는 **비용** 아님 **보안**. 인터넷을 통한 노출이 없길 원하거나 전송비용 최소화.

</aside>

`S3 / DynamoDB` 연결 → **S3 / DynamoDB 용 VPC 게이트웨이 엔드포인트**

**인터페이스 VPC 엔드포인트**

- VPC - AWS 서비스 연결 / AWS 서비스 - AWS 서비스(3-61)
- 인터넷 없이 연결
- Kinesis Data Firehose용 인터페이스 VPC엔드포인트 (3-19)

### **NLB**

<aside>
🍀 NLB의 특징은 다음과 같다

- **초당 수백만 개의 요청**을 처리
- **TCP/UDP** 기반 라우팅
- 극히 **낮은 지연시간** : request만 처리 하기 때문에(response는 내부 리소스에서 직접)
- 보안그룹X
- **고정IP** 제공(Public,Private 모두 제공)
</aside>

### IPv6

<aside>
📌 - IPv6 + 인터넷 통신 = [외부 전용 인터넷 게이트웨이](https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/egress-only-internet-gateway.html)(송신 전용 인터넷 게이트웨이)

</aside>

### ElastiCache 키워드

<aside>
💡 동일 데이터베이스를 반환하라는 호출이 자주 있다 → ‼️ElastiCache‼️

</aside>

### NAT 게이트웨이 & VPC 엔드포인트

<aside>
💡 VPC 엔드포인트: 인터넷 통과 ❌

NAT 게이트웨이: 인터넷 연결 ⭕️

</aside>

**NAT 게이트웨이** - 인스턴스에서 인터넷에 액세스 못할 때 (3-39)

![12](https://user-images.githubusercontent.com/70079416/185668148-8fa63974-9e5a-4a8d-b532-6abe5c67173d.png)

- NAT GW에는 보안그룹 X
- 인스턴스에 대한 보안그룹 확인

### NAT Gateway 랑 배스천 호스트 방향

- NAT 게이트웨이 : **EC2 → NAT GW→ 외부**
- 배스천 호스트 : **외부 → 배스천 → EC2**

### Transit Gateway

<aside>
💡 설정 및 유지 관리 + 운영 오버헤드 최소화 / 중앙 관리형 + VPC, VPN  여러 계정 관리 및 설정

</aside>

- VPC - 온프레미스 연결 (3-25)
    - 중앙 허브를 통해 Amazon Virtual Private Cloud(VPC)와 온프레미스 네트워크를 연결합니다. 복잡한 피어링 관계를 제거하여 네트워크를 간소화합니다.
- 데이터 자동 암호화
- 퍼블릭 인터넷 통하지 않음

### 인터넷 게이트웨이

- **vpc - 인터넷** 통신
- vpc 당 하나만 배포
- `IPv4 및 IPv6` 트래픽을 지원

# 데이터베이스

## Aurora 관련

### **Aurora**

`고가용성` `MySQL` `PostgreSQL` `글로벌 분산` `재해 복구` `서버리스` `완전 관리형` `비용 절감` 

- **지속적인 쓰기 가용성** → **Aurora Multi-Master Cluster**
- Aurora가 RDS보다 **안정성** 👍
- Aurora 복제본에  Aurora Auto Scaling 사용

### Aurora Serverless

`비용효율` `가변적인 워크로드` `확장성` `고가용성` `내구성`

- Aurora Serverless 가 RDS 보다 비싸다
    - [Aurora Serverless vs RDS](https://www.techtarget.com/searchcloudcomputing/answer/When-should-I-use-Amazon-RDS-vs-Aurora-Serverless)

### Aurora 복제본

aurora 복제본 + 리더 엔드포인트 ⇒ Aurora 읽기 전용 복제본

- **1초 미만 RPO** → 다중AZ , Aurora
    - 다중 리전 → Aurora 글로벌 데이터베이스
- Aurora 복제본 기본으로 제공. Aurora 가용성 추가 향상 → 교차리전복제 (2-100)

## Dynamo DB

- 장바구니 → Dynamo DB
- 높은 읽기 처리량과 마이크로초 지연시간
- 3 AZ 자동생성
- 스로틀링(throttling)(조절) 오류 원인 (4-26)
    - 핫 파티션
    - 용량 부족 → 용량을 늘리자 (읽기 복제본 X)

### **DAX**

`가용성` `완전 관리형` `인 메모리 캐시` `초당 수백만개의 요청을 몇 밀리초~마이크로초` `애플리케이션 로직 변경 x`

## RDS

- ASG 내에 Read-Replica 지원 안함
- 자동으로 스토리지 확장이 가능
- RDS DB 인스턴스 암호화는 생성 후에 불가능 → 인스턴스 스냅샷으로 기존 데이터 암호화
- 다중 리전에서 읽기 전용 복제본 생성 가능
- 교차 리전 시나리오에서는 AWS 리전 간 네트워크 채널이 더 길어지기 때문에 원본 DB 클러스터와 읽기 전용 복제본 간의 지연 시간이 증가합니다.
    
    → **교차 리전에서는 읽기 복제본 X / 교차 리전 복제본 O (지연시간 고려 시)**
    
- 비용 : Aurora > RDS (비용 효율)

### **[AWS ElastiCache vs RDS ReadReplica](https://stackoverflow.com/questions/24728634/aws-elasticache-vs-rds-readreplica)**

- 동일한 쿼리 반복이면 ElastiCache가 유리
- 쿼리가 동적으로 계속 변한다 → Read Replica
- ElastiCache는 훨씬 빠르지만, RAM에서 바로 값을 가져오기 때문에 저장할 수 있는 데이터가 한정적이다.
- 읽기 전용 복제본은 마스터에서 지속적으로 동기화됩니다. 따라서 결과는 아마도 마스터보다 0 - 3초(로드에 따라 다름) 뒤쳐질 것입니다.
    
    

### [지정시간복구](https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/PointInTimeRecovery.Tutorial.html)(PITR)

- **S3, RDS 만 지원!**
- 자동 백업 활성화 + 특정 시점 복구 set 많이 쓰임

### RDS vs DynamoDB

|  | DynamoDB | RDS |
| --- | --- | --- |
| 가용성 | 리전만 선택 가능, AZ는 3개 자동으로 생성 | Multi-AZ 옵션을 활성화해야함(+비용) |
| KMS | aws 관리형, 고객 관리형 | aws 관리형(AES-256 encryption for data on RDS DB server) |
| 사용 사례 | 실시간, 장바구니, 모바일, 컨텐츠 관리, 높은 I/O 처리, 구조화되지 않은 게임 데이터 | 구조가 있고 관계를 가진 데이터, 이미 존재하는 데이터,  |
|  확장 | 수평 | 수직 |

## Redshift

- OLAP
- Postgre SQL 기반
- **단일 리전 배포만** 지원 (다중 AZ 배포 X)
- 가용영역 (내구성 보장) 시 → **교차 리전 스냅샷 활성화**! (4-48)

🔷 **읽기 “autosacling”**의 키워드 ⇒ **읽기 확장성, 읽기 요청에서 조절 발생, 용량단위 수 설정**

🔷 PostgreSQL + **여러 리전+데이터 항상** ⇒ **교차리전+읽기전용 복제본** 

### ElastiCache Redis vs Memcached

| Redis  | Memcached |
| --- | --- |
| Multi-AZ & 자동 장애조치 | Multi-node (sharding) |
| Read Replicas & 고가용성 | Multi-thread 아키텍처 |
| 데이터 지속성 백업 | 가용성 X 지속성 X 백업 X  |
| 데이터 복제 가능 | 데이터 복제 불가능 |
- 뭐든 좋은 건 Redis
- multi-node, multi-thread 만 memcached

- rds 읽기 전용 복제본에 ASG 배치 X (4-12)

# 보안

## 암호화

- **클라이언트 측 암호화** : 사용자가 AWS로 데이터를 보내기 전에 데이터를 암호화한다.
- **서버 측 암호화** : AWS가 서비스를 통해 데이터를 받은 후 해당 데이터를 사용자 대신 암호화하는 것이다.

<aside>
📢 **서버 측 암호화**:
S3 Managed Keys (SSE-S3)
KMS Managed Keys (SSE-KMS)
Customer Provided Keys (SSE-C)

**클라이언트 측 암호화**:
KMS managed master encryption keys (CSE-KMS)
Customer managed master encryption keys (CSE-C)

</aside>

### 서버 측 암호화

**고객 제공 키를 사용한 서버 측 암호화(SSE-C)**

- 고객이 키 관리, 암호화는 AWS에서

**Amazon S3 관리형 키를 사용한 서버 측 암호화(SSE-S3)**

- AWS 관리형 키로 암호화 + 키 추적, 감사 X

**AWS Key Management Service에 저장된 KMS 키를 사용한 서버 측 암호화(SSE-KMS)**

- AWS 관리형 키로 암호화 + 키 추적, 감사 기능 O
- 추가 비용 발생

### KMS 유형

![13](https://user-images.githubusercontent.com/70079416/185668126-574bf95d-d958-47d2-9c58-ae4467e53043.png)

### 세션

 세션 선호도(고정 세션)

- 고정 세션을 사용하면 사이트 사용자를 개별 사용자의 세션을 관리하는 **특정 웹 서버로 라우팅**할 수 있습니다.

분산 세션 관리

- **확장성을 해결**하고 개별 웹 서버에서 액세스할 수 있는 **세션에 대한 공유 데이터 저장소를 제공**하기 위해 웹 서버 자체에서 HTTP 세션을 추상화할 수 있습니다.
- 키/값 데이터 저장소는 매우 빠르고 밀리초 미만의 대기 시간을 제공하는 것으로 알려져 있지만 **추가된 네트워크 대기 시간과 추가 비용**이 단점입니다.
- 일부 키/값 저장소는 **읽기 전용 복제본**을 통해 복제를 제공합니다. 메모리 내 키/값 저장소용 **ElastiCache** 제품에는 복제를 지원할 수 있는 Redis용 ElastiCache와 복제를 지원하지 않는 Memcached용 ElastiCache가 포함됩니다.

## 데이터 보안 서비스 키워드

<aside>
💡 Shield: DDoS 공격 차단
Macie: 민감한 데이터 보호
Inspector: EC2 한정, 보안 평가
GuardDuty: 전체 AWS 계정, (가용성이 좋은) 잠재적 위협 감지, ‘암호 화폐 관련 공격’의 키워드

</aside>

### 데이터베이스 암호화 - 스냅샷 이용해서

- 기존 DB 인스턴스에서 스냅샷 복사
- 스냅샷 암호화 (이때 KMS 키 선택할 수 있음)
- 암호화된 스냅샷 새 DB인스턴스에 복원 (existing DB에 스냅샷 복원 X)

### 자격 증명 → Secrets Manager

- 자격 증명 교체, 관리, 검색
- 절대 S3, 인스턴스 스토어 같은 DB에 저장 x, 필요시 IAM 사용

### 키 → KMS

### ACM(AWS Certificate Manager)

AWS 서비스와 연결된 내부 리소스에 사용할 퍼블릭 및 프라이빗 **SSL/TLS 인증서**를 손쉽게 프로비저닝, 관리 및 배포

### IAM 정책

- deny > allow 우선
- 명시 안되면 deny

### 읽기 전용 복제본 pre 작업

1. 읽기 전용 복제본을 생성하기 전에 장기 실행 트랜잭션이 완료되기를 기다리는 것이 좋습니다. 
2. 백업 보존 기간을 0이 아닌 다른 값으로 설정하여 원본 DB 인스턴스의 자동 백업을 활성화해야 합니다. 

### 개념 정리 ,,for 지우

<aside>
✏️ Secrets Manager에서 Lambda 함수 자동으로 생성
서버리스: 예측 불가능한 가변 워크로드, 특정 인스턴스 선택 X, 액세스 패턴이 드문 등
SSL 인증서 → ACM 키워드
캐시는 ‘읽기’만 가능❗️, 자주 액세스하는 데이터
Lambda 함수로 사기 IP 식별 후 WAF로 트래픽 허용 및 차단
AD(Active Directory) + SSO(Single Sign-On) 조합
S3 ACL은 여러 사용자 관리에 부적합 → 권장 X
WAF + CloudFront → 특정 국가에 대한 IP 차단에 적합

</aside>

### 역할(Role)

- 리소스 : 다른 서비스에 ~~ 하기위해
- AWS 계정 : 계정 A 가 계정 B의 서비스에 액세스
- Open ID (facebook, google, cognito)
- SAML 2.0 연동 (사내 인증 서비스)

### SAML 2.0

온프레미스 + { Active Directory, 사용자 인증 } ⇒ SAML 2.0

**Single Sign-On** : 통합 로그인 서비스

- Cognito는 지원 X

STS (Simple Token Service) = 임시 자격증명 ↔ IAM : 영구 자격증명

### Cognito

- **사용자 풀:** 앱 사용자의 가입 및 로그인 옵션을 제공하는 사용자 디렉터리입니다.
- **자격 증명 풀**을 통해 사용자에게 기타 AWS 서비스에 액세스할 수 있는 권한을 부여할 수 있습니다.

### 서비스 제어 정책(SCP)

- 조직의 모든 계정의 권한 관리
- SCP는 관리 계정의 사용자 또는 역할에 영향을 미치지 않습니다. 조직의 멤버 계정에만 영향을 줍니다. → **계정 관리자가 영향을 받는 계정의 IAM 사용자 및 역할**

### AWS Organizations

- 중앙 집중식 기업 디렉터리 서비스
- aws 계정 단위로 관리 → $ 통합 청구

### CloudTrail 데이터 보호 4가지

- [AWS KMS 관리형 키(SSE-KMS)로 CloudTrail 로그 파일 암호화](https://docs.aws.amazon.com/ko_kr/awscloudtrail/latest/userguide/encrypting-cloudtrail-log-files-with-aws-kms.html)
- [CloudTrail에 대한 Amazon S3 버킷 정책](https://docs.aws.amazon.com/ko_kr/awscloudtrail/latest/userguide/create-s3-bucket-policy-for-cloudtrail.html)
- [CloudTrail 로그 파일 무결성 검증](https://docs.aws.amazon.com/ko_kr/awscloudtrail/latest/userguide/cloudtrail-log-file-validation-intro.html)
- [AWS 계정 간 CloudTrail 로그 파일 공유](https://docs.aws.amazon.com/ko_kr/awscloudtrail/latest/userguide/cloudtrail-sharing-logs.html)

**CloudTrail** = 계정의 API 호출과 관련된 이벤트를 기록하는 서비스

사용자, 역할 또는 AWS 서비스가 수행하는 작업은 CloudTrail에 이벤트로 기록됩니다.

# 분석

### AWS Config

- AWS 리소스 구성 기록 및 평가 / **모니터링**
- CloudTrail 통합
    - CloudTrail 로그의 변경 사항을 유발한 이벤트 API의 세부 정보도 얻을 수 있습니다(예: 요청한 사람, 시간 및 원본 IP 주소).
- 키 수명 확인 (5-57)

### AWS Batch

- 제출된 배치 작업량과 리소스 요건을 기준으로 컴퓨팅 리소스를 프로비저닝

### Kinesis

- Kinesis Data Firehose: `다른 데이터 원천(S3, Redshift, Elasticsearch)으로` `캡쳐` `변환` `전송`
- Kinesis Data Analytics: `**SQL 쿼리**` `수집` `변환` `분석` `실시간` `Firehost, Streams, S3로부터 받은` `ETL`
- Kinesis Data Streams: `실시간 스트리밍` `확장 가능` `내구성`  `게임` `IoT` `모바일`

### AWS Step Functions

→ **시각적 워크플로우를 사용해 분산 애플리케이션 및 마이크로서비스의 구성 요소를 손쉽게 조정**하도록 해주는 **서버리스 웹 서비스**

- Standard Workflow
- Express Workflow : IoT, 실시간 데이터 처리

### AWS Glue

→ 분석, 기계 학습 및 애플리케이션 개발을 위해 **데이터를 쉽게 탐색, 준비, 그리고 조합할 수 있도록 지원하는 서버리스 데이터 통합 서비스**

### Redshift

- SQL
- 데이터 웨어하우스, 운영 데이터베이스, S3 데이터 레이크
- 정형 데이터 및 반정형 데이터를 분석
- 쿼리 속도 향상
- ML tools, BI 분석 툴

**`ingestion(수집)`, `edge devices` → Kinesis or Snowball edge devices**

<aside>
📢 S3 에 대한 로그 + 삭제한 사람 쿼리 → Athena 쿼리로 CloudTrail 로그 분석

</aside>

PostgreSQL용 Amazon RDS : JSON 형식 데이터 최대 30일 보관 가능 (5-7)

S3 : Lambda 사용하여 JSON 변환 작업 필요