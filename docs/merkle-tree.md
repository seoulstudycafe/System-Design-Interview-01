# Merkle Tree
## 시공간 복잡도
- 이진 트리 구조의 특성 상 탐색 시 시간 복잡도 O(logN)
- 공간 복잡도는 포화 이진 트리임을 전제하면 O(N)

## 보안
- 충돌저항성: 루트의 해시 값이 하위 값을 포함하여 해싱된 값이기 때문에, 압축된 데이터 중 하나라도 다르게 되면 루트 값이 다름.
- 이 덕분에 데이터 위변조 방지 및 데이터 불변을 증명한다는 점에서 보안이 높다고 평가됨.

## 적용 사례
- 블록 체인
  - 블록체인이 갖고 있는 트랜잭션 목록을 머클트리로 구성하여 블록체인의 무결성을 보장한다.
  - O(logN) 이라는 탐색 속도 덕분에 블록체인 내부의 거래를 쉽고 빠르게 찾도록 도와준다.
- Anti Entropy Protocol in 분산 시스템

## 한계점
- 블룸 필터와 비교했을 때 여전히 연산 비용이 비싼 편이다.

---
참고자료
- https://www.banksalad.com/contents/%EC%89%BD%EA%B2%8C-%EC%84%A4%EB%AA%85%ED%95%98%EB%8A%94-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-%EB%A8%B8%ED%81%B4%ED%8A%B8%EB%A6%AC-Merkle-Trees-%EB%9E%80-ilULl
  - 블록체인의 헤더로서 머클트리가 쓰이는 이유
- https://borntodevelop.tistory.com/entry/Solidity-%EB%A8%B8%ED%81%B4%ED%8A%B8%EB%A6%AC-Merkle-Tree-Root-Proof-Solidity-08-Openzeppelin#google_vignette
  - 블록체인 관점에서 머클트리를 사용하는 보안 상 장점
- https://medium.com/curg/hex-bloom%EC%9D%80-%EB%A8%B8%ED%81%B4-%ED%8A%B8%EB%A6%AC%EB%A5%BC-%EB%8C%80%EC%B2%B4%ED%95%A0-%EC%88%98-%EC%9E%88%EC%9D%84%EA%B9%8C-44475ca53c7b
  - 머클트리의 대안 hex-bloom 소개