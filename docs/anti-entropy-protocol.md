# Anti-entropy protocols
> entropy: 무질서  
> anti-entropy: 무질서를 줄이는 것 = consistency

일시적으로 발생한 데이터 무질서를 백그라운드 프로세스를 통해 해소하는 일련의 규약

## 규칙

### Balance between consistency and availability
- `Eventual Consistency`
- consistency vs availability trade-off에서 균형을 유지하는 것.
- short-term inconsistency 를 허용하고, 시간이 지나면 데이터가 일관성을 갖는다는 것.

### 버전 관리
- 데이터 간 충돌 발생 시 timestamp, version number 등을 통해 해결해야 한다.

## Implements
1. Synchronization
2. Merkle Tree
3. Gossip Protocol
4. Hinted handoff
5. Read Repair
6. Active and Passive Anti Entropy Mechanism: 비일관성 발견 시 수리가 시작되는 active, 데이터 복제본 read 요청이 들어왔을 때 수리하는 passive.

## Merkle Tree


---
참고자료
- https://www.linkedin.com/pulse/anti-entropy-protocols-yeshwanth-n-2c/
- https://pawan-bhadauria.medium.com/distributed-systems-part-3-managing-anti-entropy-using-merkle-trees-443ea3fc6213 
  - Hash ring 에 놓여있는 노드 간 데이터의 일부분이 필연적으로 다를 수 밖에 없는 경우, 공통 부분만 merkle tree 를 적용하라는 테크닉을 소개함
- https://highscalability.com/gossip-protocol-explained/ 
  - 노드 간 네트워크를 통해 메세지를 주고 받는 Gossip Protocol 을 Anti-Entropy 이외의 관점에서도 소개하고 있음. 
- https://systemdesignschool.io/blog/anti-entropy
  - 개념 위계가 잘 잡혀있는 문서
