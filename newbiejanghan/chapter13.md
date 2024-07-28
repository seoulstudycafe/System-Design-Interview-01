# Chapter 13 검색어 자동완성 시스템
### Google 검색 기능
##### 신기한 점
이전 검색 기록이 새 검색어 추천에 반영된다.
network 확인해보니 검색어 인풋마다 발생하는 get query parameter로 이전 검색어가 계속해서 들어간다.

##### 어쩌다 발견한 점
정확히 일치하는 검색어에 대한 브라우저 캐시가 동작 중이다.
"subscribe" 검색 이후 network 로그를 보면
subscrib 까지만 검색 요청 발생
이후 다른 단어 타이핑 시
subscribee 부터 다시 검색 요청이 발생

### 설계
##### 요구사항
1. 연관 검색어 추천 기능 => trie 자료 구조
2. 낮은 latency
3. 실시간 검색 순위 반영
4. 이전 검색 기록 반영
##### Componets
1. Trie 자료 구조
    - query: String
    - score
2. Controller
    - HTTP 통신
3. Service
    - 로그 기반의 query frequency 데이터와 실시간 검색 기록 기반의 trend score 를 계산하는 로직
4. Trie Cache
    - database snapshot
    - 1시간 단위의 실시간 검색 기록을 저장
5.  Trie database
    - 로그 기반으로 query 당 frequency를 저장하는 table