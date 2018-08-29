# Serverless Data Tracking & Analytics In Startup in Vingle

## 1. 인트로

### 데이터 다루기가 어렵다.(정제..수집..등등)

다양한 툴이 존재하지만 시스템을 만드는 것과 데이터를 설계하는 것이 어렵다.

| Building System                     | Data Architecture                                 |
| ----------------------------------- | ------------------------------------------------- |
| 실제로 해야하는 일이 많다.          | 무엇을 수집하고 어떻게 수집해야하나?              |
| 여러가지 툴을 잘 접목해야한다.      | 얼마나 자주 얼마나 빨리 접근 가능해야하나?        |
| 하나의 툴을 쓴다고 해서 끝나지 않음 | Query 가 가능한 일관된 Table Schema               |
| 시스템에 문제가 생길때를 고려애향함 | 정답이 없고 주어진 문제 / 상황 / 팀에 따라 다르다 |

### Building System

해야하는 잡일들 중의 상당 부분을 다른 서비스(AWS...)로 대체한다.
실질적으로 해야하는 일에 집중해야한다.

-> Serverless

---

## 2. 데이터 설계

설계원칠

1. 정형화된, 일관성 있는 형식
2. 다양한 형태 / 종류의 Action 과 Event 를 담을 수 있어야함(비명시적/명시적/직접적/간접적)
3. Font end Team 에서 직관적으로 이해하기 쉬워야 함.

Step.1 데이터를 문장으로 정리

ex) 이 **유저**가 **아이폰**으로 **피드**에서 **3 번째**에서 **카드**를 **클릭**해서 **10 초동안 봤다**.
(structure --> schema) user content location referral action

JSON 으로 주고 받을때 STR 으로 하고  
RDS 에 입력할 때 NUM TO STR 로 하는게 안전하다.  
URL 을 적극활용하면 좋다. 어떤화면 + 추가정보(query)

---

## 3. 데이터 수집 & 정제 & 저장

목표 : 어떤 유저든 쓰는 만큼만 어떤 상황에든 문제없이 안전하게 쓸 수 있어야한다.

User -> API Gateway -> Lambda -> Kinesis Firehose -> S3

> Amazon Kinesis Data Firehose 는 스트리밍 데이터를 데이터 스토어 및 분석 도구에 가장 쉽고 안정적으로 로드하는 방법입니다. 스트리밍 데이터를 캡처하고 변환한 후 Amazon S3, Amazon Redshift, Amazon Elasticsearch Service 및 Splunk 로 로드하여 이미 사용하고 있는 기존 비즈니스 인텔리전스 도구 및 대시보드를 통해 거의 실시간으로 분석할 수 있습니다. (압축을 알아서 해주기 때문에 데이터를 읽을때 과금이 적어지기 때문에 유리!)

---

## 4. 데이터 사용 Data Analytics & Application

S3 쌓아두면 가능성은 무궁무진 (Data Lake)

1. Query

- Create External Table
- Athena Query

2. Personalize (Personalize to user's pattern)

- Create External Table
- S3 -> Aurora Mysql 로 로딩 (LOAD DATA FROM ...)
- Query with userId in Mysql

3. Machine lerning

- Follow user expectation
- Amazon ML

---

## 5. 가격

쓴만큼만 딱 나옴

---

## 6. 결론

1. 유저의 경험과 서비스를 개선하려면 결국 Application Layer 를 개선해야함
2. Data Pipeline 을 직접 운영하지 않고 비지니스 로직에 집중해야한다.
3. Serverless 를 적극적 도입해야하고

> Amazon EMR 은 AWS 에서 Apache 하둡 및 Apache Spark 와 같은 빅 데이터 프레임워크 실행을 간소화하는 관리형 클러스터 플랫폼입니다. 이러한 프레임워크와 함께 Apache Hive 및 Apache Pig 와 같은 관련 오픈 소스 프로젝트를 사용하여 분석용 데이터와 비즈니스 인텔리전스 워크로드를 처리할 수 있습니다. 또한 Amazon EMR 를 사용하여 Amazon Simple Storage Service(Amazon S3) 및 Amazon DynamoDB 와 같은 기타 AWS 데이터 스토어 및 데이터베이스에서 많은 양의 데이터를 양방향으로 변환하고 이동할 수 있습니다.
