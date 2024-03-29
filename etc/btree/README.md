B Tree
===
>[시뮬레이션 (B Tree)](https://www.cs.usfca.edu/~galles/visualization/BTree.html)<br>[시뮬레이션 (B+ Tree)](https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html)

### 구조
```
                  ROOT
                   |
     -----------------------------
     |             |             |
   BRANCH        BRANCH        BRANCH
     |             |             |
 ---------     ---------     ---------
 |       |     |       |     |       |
LEAF    LEAF  LEAF    LEAF  LEAF    LEAF
```

<br>

### 특징
* 이진트리와 다르게 모든 리프 노드들이 같은 레벨을 가질 수 있도록 자동으로 균형을 맞춤
* 정렬된 순서를 보장하고, 멀티 레벨 인덱싱을 통한 빠른 검색이 가능
* 하나의 노드에 많은 수의 정보를 가지고 있을 수 있음
* 최대 `M`개의 자식을 가지고 있다면 `M차 B트리` 라고 하며 아래와 같은 특징을 지님
  1. 노드는 최대 `M`개 부터 `M / 2`개 까지의 자식을 가질 수 있음
  1. 노드에는 최대 `M−1`개 부터 `(M / 2) − 1`개의 키가 포함될 수 있음
  1. 노드의 키가 `x`개라면 자식의 수는 `x + 1`개
  1. 최소 차수는 자식 수의 하한값을 의미하며, 최소 차수가 `t`라면 `M = 2t − 1`, 최소 차수 `t`가 2라면 3차 B트리이며, key의 하한은 1개)

<br>

### B+ Tree
* B+ 트리는 B 트리와 거의 동일하지만 아래와 같은 차이점이 있음
  1. 리프 노드를 리프 노드에 연결하는 포인터가 있음
  1. 리프 노드에만 데이터 유지

<br>

### B+ Tree의 특징
* 범위 검색에 좋음 (리프 노드만 보면 되기 때문)
* 자식 노드에는 키만 있기 때문에 많은 키가 페이지 (블록) 에 배치되어 계층 구조가 줄어들고 이로 인해 계산량이 줄어듬

<br>
