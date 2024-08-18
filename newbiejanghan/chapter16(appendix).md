### Scale at Facebook

https://www.infoq.com/presentations/Scale-at-Facebook/

### Building Timeline: Scaling up to hold your life story

##### why denormalization

1. 정규화된 여러 테이블 간 조인이 필요한 데이터 형이 필요한 경우 대안은 캐싱과 비정규화가 있다.
2. 만약 캐시에 특정 속성에 대한 데이터가 없다면 랜덤한 fetch io가 발생하면서 문제가 속도가 저하된다. 이를 막기 위해 타임라인 기능에 필요한 전체 데이터 셋을 하나의 pk로 묶는 비정규화를 진행햇다.

##### chanllenges to migrate

1. legacy data format conversion
2. copy from slow network storage: 오래된 데이터를 속도가 느린 저장소에서 복사해와야 하는 문제. readonly mysql server를 해킹한 후 수백대를 띄워 최대 io 부하로
   copy 해서 수달 짜리를 수 주에 끝냄
3. Massive join queries that did tons of random IO. We consolidated join tables into a tier of flash-only databases.
   Traditionally PHP can perform database queries on only one server at a time, so we wrote a parallelizing query proxy
   that allowed us to query the entire join tier in parallel.
4. Future-proofing the data schema. We adopted a data model that’s compatible with Multifeed. It’s more flexible and
   provides more semantic context around data with the added benefit of allowing more code reuse.

### Erlang at Facebook

##### why with erlang

https://m.cwn.kr/news/articleView.html?idxno=7725

1. Concurrency
    1. 저렴한 동시성 처리.
    2. 프로세서마다 서로 독립적인 메모리를 할당함.
2. Distribution?
3. Fault Isolation
4. Error logging
5. Hot cod swapping
6. Monitoring and Error Recorvery
    1. 감시자 프로세서가 오류난 프로세서를 재시작하는 기술.
7. Remote Shell
8. Erlang top

### Facebook Chat

##### Real-time presence notification

###### learned? false. too hard

### Facebook's photo storage: FInding a needle in Haystack

https://www.usenix.org/legacy/event/osdi10/tech/full_papers/Beaver.pdf

##### Background & previous design

###### NFS-based design

1. CDN은 long-tail content에는 부적합. sns은 long-tail content를 fetching 해올 일이 많다.

### Netflix A/B test

##### how to allocate members to a test

1. Batch
    1. 분석가가 직접 쿼리 짜서 테스트 실행에 용이
    2. 신규 고객 또는 유저 실시간 행동에 따른 테스트 배치가 불가
2. Real-Time
    1. 유저 행동에 따라 평가되는 규칙을 분석가들이 설정하도록 하는 방식
    2. 규칙 평가 후 테스트 적용하는데 걸리는 시간 지연을, 어차피 수행되어야 할 시간 지연 타이밍에 병렬적으로 실시하여 유저 체감 latency에는 큰 영향을 안 주게 하였음.
    3. 분석가에게 필요한, 적절한 수의 회원이 테스트에 할당되는데 소요되는 시간 정보를 평가하기가 어렵다.

##### architecture

1. A/B server
    1. DB로부터 메타데이터 조회,
    2. Sampling, Allocation 로직 실행 후 Db에 persist
    3. Cache에 탑재
    4. kafka로 allocation 관련 정보 publish
2. kafka subscribers
    1. A/B Testing visualization & analysis tool, Ignite
    2. Spartk streaming: ingests and transforms data from kafka streams to persist data in elasticSearch
    3. elasticsearch: real time updates to ABlaze
    4. ABlaze: test schedule view (frontend platform)

##### futures

1. global 사용자 증가, 다양한 모바일 기기 증가. 특정 모바일 기기의 대역폭은 어떤 테스트의 결과물을 받아볼지 대기할만큼 안정적이지 않아서 real-time보다는 batch allocation을 쓰고 있는
   상황. real-time allocation의 속도가 더 빨라질 필요가 있음.
2. real-time allocation rules 설정에 따른 테스트 할당률 ( 시간 내에 할당 받아서 유저가 해당 테스트에 맞는 서빙을 받는 비율 )를 판단하고 개선하기 위해, 유저가 할당받는데 필요한 시간을
   예측하고자 함.

##### learned

1. A/B 테스트 시스템 개요
2. latency 걸리는 여러 작업을 동시가 아닌 병렬적으로 처리하여 유저 눈 속이기
