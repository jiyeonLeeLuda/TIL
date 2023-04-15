# DB 선택

**updated 2023.04.15**

## DB의 종류

DB는 데이터베이스 모델에따라 종류를 구분할 수 있는데,
대표적인 데이터 베이스 모델은

- Relational DBMS
- Key-value stores
- Document stores
- Graph DBMS
- Time Series DBMS 등이 있고,

SQL을 사용하는 관계형 DB (Relational DBMS), 즉 RDBMS와
그밖의 모델을 통칭하여 NoSQL이라 구분한다.

또한 데이터를 저장하는 위치에 따라,
onDisk , inMemory DB로 나뉜다.

## 커머스, 실시간 경매 서비스에 필요한 DB는 무엇인가?

해당 서비스를 관리하기 위해서는

- 거래 데이터를 기록하는 RDBMS와
- 세션 스토리지 겸 캐싱역활을 하는 NoSQL이 필요할 것이라 예상.

### RDBMS 선정 기준

- 비용 (프로덕션 배포가 가능하고, 무료 라이센스인지)
- 언어 지원 (Java를 지원하는지 - JDBC같은 커넥터 api를 제공하는지)
- 실시간 경매의 트랜잭션을 보장해야한다.
- 대용량 트래픽을 처리할 수있어야 한다 : 어떤 확장방식 지원하는지 체크 (Partitioning methods)
  - 확장 관련 용어 (더 알아보기)
    - Replication
    - Partitioning
    - Clustering
    - Sharding

### NoSQL의 선정 기준

- 비용 (프로덕션 배포가 가능하고, 무료 라이센스인지)
- 언어 지원 (Java를 지원하는지 - JDBC같은 커넥터 api를 제공하는지)
- 세션 스토리지 겸 캐싱역활을 할 수 있는 모델이여야 한다. (key-value, document 유력)
- 빠른 읽고 쓰기가 되어야 한다. (imMemory DB 유력)

### DB 순위 모델별 Top 5 (2023.04.15기준)

출처 [db-engines](https://db-engines.com/en/system/MySQL)

#### RDBMS

1. Oracle
2. MySQL
3. Microsoft SQL server
4. PostgreSQL
5. IBM Db2

#### key-value Store

1. Redis
2. Amazon DynamoDB
3. Microsft Azure Cosmos DB
4. Memcashed
5. etcd

#### Document

1. MongoDB
2. Amazon DynamoDB
3. Databricks
4. Microsft Azure Cosmos DB
5. Couchbase

## 더 알아보기

### 확장관련 용어

> Replication - Copying an entire table or database onto multiple servers. Used for improving speed of access to reference records such as master data.

> Partitioning - Splitting up a large monolithic database into multiple smaller databases based on data cohesion. Example - splitting a large ERP database into modular databases like accounts database, sales database, materials database etc.

> Clustering - Using multiple application servers to access the same database. Used for computation intensive, parallelized, analytical applications that work on non volatile data.

> Sharding - Splitting up a large table of data horizontally i.e. row-wise. A table containing 100s of millions of rows may be split into multiple tables containing 1 million rows each. Each of the tables resulting from the split will be placed into a separate database/server. Sharding is done to spread load and improve access speed. Facebook/twitter tables fit into this category.

출처 [What is the difference between replication, partitioning, clustering, and sharding?](https://www.quora.com/What-is-the-difference-between-replication-partitioning-clustering-and-sharding)

### NoSQL 각각의 모델별 장단점 파악

#### Key-Value Store

- 가장 단순한 방식. = 모델링 하기 어려운 데이터도 담기 편하다.
  = 관계형에서 설계시 소모되는 시간을 줄일 수 있음.
- => 단순한 구조로 저장하기 때문에 쓰기 성능도 빠르다.=> 데이터의 신속한 기록이 중요할 때, 유용함.=> 키값으로 데이터베이스에 접근해야 하는 경우 유리.

- 실제 서비스 사용 예)아마존 쇼핑카트 (Amazon Dynamo)

#### Document

- 도큐멘트 = 반구조적(반정형) 데이터 라고도함.
- 이 자료형은 Json과 유사한 형식으로 나타낼 수 있다.  = 키: 벨류 형태, 계층적 구조로 데이터를 저장할 수 있다.
- 동일한 콜렉션에 있는 도큐멘트형 자료형을 만들더라도, 각각의 도큐멘트의 특징이나 데이터 값은 다를 수 있다.

- 실제 서비스 사용 예)
  - Linked-In ( 온라인 이력서 공유 및 인맥 서비스)
  - Dropbox Mailbox ( 드롭박스의 메일박스)

#### Graph

- 데이터를 저장할 때, 특수한 형태로 데이터를 저장한다. 노드, 속성, 엣지로 저장할 데이터의 정보를 나눠 담아 저장.

  - 노드 =데이터 객체
  - 속성 = 객체 특징 정보
  - 엣지 = 객체간의 관계

- => 데이터를 저장할 때, 데이터간의 관계가 저장되기 때문에 "관계성"으로 데이터를 조회 할 경우 속도가 빠름.= 복잡한 연산이 필요한 "관계 분석" 처리시 그래프 모델이 유리 .

- 실제 서비스 사용 예) Neosocial which connects Facebook
