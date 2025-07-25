# Q&A

### Q 해시 테이블을 설명하세요
**A**
- 해시 테이블은 무한에 가까운 데이터(키)들을 유한한 개수의 해시 값으로 매핑한 테이블입니다.
- 이를 통해 작은 크기의 캐시 베모리로도 프로세스를 관리하도록 할 수 있습니다.

### Q 그래프와 트리의 차이점은 무엇인가요?
**A**
- 그래프는 정점과 간선으로 이루어진 자료 구조를 말하며, 
- 트리는 그래프 중 하나로 그래프의 특징처럼 정점과 간선으로 이루어져 있고 트리 구조로 배열된 일종 의 계충적 데이터의 집합입니다. 
- 루트 노드, 내부 노드, 리프 노드 등으로 구성되며 V-1=E 등의 특징이 있습니다.
- 
### Q
이진 탐색트리는 어떤 문제점이 있고 이를 해결하기 위한 트리 중한 가지를 설명해보세요
**A**
이진 탐색 트리는 선형적으로 구성될 때 시간 복잡도가 0(n)으로 커지는 문제점이 있습니다. 선형적으로 구성하지 않고 균형 잡힌 트리로 구성하기 위해 나온 트리로 AVL 트리와 레드 블랙 트리가 있으며, 이 중 AVL 트리를 설명하겠습니다
AVL 트리(Adelson-Velsky and Landis tree)는 스스로 균형을 잡는 이진 탐색 트리입 니다. 두 자식 서브트리의 높이는 항상 최대 1만큼 차이 난다는 특징이 있습니다. 탐색, 삽입, 삭제 모두 시간 복잡도가 0(ogn)이며 삽입. 삭제를 할 때마다 균형이 맞지 않는 것을 맞추기 위해 트리 일부를 왼쪽 혹은 오른쪽으로 회전시키며 균형을 작습니다.
