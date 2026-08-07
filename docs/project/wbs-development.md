# C-DS Roadmap Scheduler 개발 WBS

> 기간: 2026-08-07 ~ 2026-08-28  
> 기준 시간: 하루 평균 6시간 + α  
> 프로젝트 목표: 자료구조와 알고리즘을 적용한 C 기반 CLI 일정 최적화기 제작  
> 기획 기간: 2026-08-07 ~ 2026-08-09  
> 개발/검증 기간: 2026-08-10 ~ 2026-08-28  
> 버퍼: 총 5일 확보

---

## 1. 프로젝트 개요

## 1.1 프로젝트명

C-DS Roadmap Scheduler

## 1.2 한 줄 설명

작업, 마감일, 우선순위, 예상 소요 시간, 선행 작업 관계를 입력하면 자료구조와 알고리즘을 이용해 가능한 작업 순서와 오늘 해야 할 작업을 추천하는 C 기반 CLI 프로그램이다.

## 1.3 핵심 목적

이 프로젝트의 목적은 단순한 일정 관리 앱을 만드는 것이 아니다.  
자료구조 수업 전 주요 개념을 직접 구현하고, 실제 문제 해결에 적용해보는 것이 핵심이다.

따라서 기능 수보다 다음을 우선한다.

1. 자료구조 직접 구현
2. 적용 이유 설명 가능
3. 시간복잡도/공간복잡도 분석
4. 테스트와 검증
5. 발표/면접에서 설명 가능한 문서화

---

## 2. MVP 범위

## 2.1 필수 기능

| ID | 기능 | 설명 | 적용 자료구조/알고리즘 |
|---|---|---|---|
| F-01 | 작업 추가 | 작업명, 우선순위, 마감일, 예상 시간 입력 | 구조체, 배열/동적 배열 |
| F-02 | 작업 목록 조회 | 등록된 작업 목록 출력 | 배열, 정렬 |
| F-03 | 선행 작업 관계 추가 | A 작업 후 B 작업 가능 관계 등록 | 방향 그래프 |
| F-04 | 순환 의존성 검사 | A→B→C→A 같은 잘못된 관계 감지 | DFS, 스택 개념 |
| F-05 | 가능한 작업 순서 계산 | 선행 관계를 고려한 작업 순서 출력 | 큐, 위상 정렬 |
| F-06 | 오늘 할 일 추천 | 마감일/우선순위/선행조건 기준 추천 | 힙, 우선순위 큐 |
| F-07 | CSV 저장/불러오기 | 프로그램 종료 후 데이터 유지 | 파일 입출력 |
| F-08 | 실행 예시/샘플 데이터 | 시연 가능한 기본 데이터 제공 | CSV, 문서화 |

## 2.2 선택 기능

| 기능 | 조건 |
|---|---|
| SQLite 저장 | CSV 기능이 안정화된 뒤 시간이 남을 때만 |
| Undo | 스택 구현 이후 시간이 남을 때만 |
| Markdown 보고서 출력 | 최종 문서 시간이 충분할 때만 |
| 예쁜 CLI 메뉴 | 기능 검증 이후에만 |

## 2.3 이번 프로젝트에서 하지 않을 것

- 웹 프론트엔드 구현
- Spring Boot 서버 구현
- MySQL/Oracle 연동
- 로그인/회원 기능
- 복잡한 GUI
- 완벽한 일정 최적화 알고리즘

---

## 3. 예상 디렉터리 구조

```text
project-root/
├─ README.md
├─ CMakeLists.txt
├─ docs/
│  ├─ 00_plan/
│  │  ├─ learning-roadmap.md
│  │  ├─ wbs-development.md
│  │  ├─ daily-log.md
│  │  └─ decision-log.md
│  ├─ 01_learning/
│  ├─ 02_design/
│  ├─ 03_verification/
│  ├─ 04_blog-drafts/
│  └─ 05_interview/
├─ include/
│  ├─ task.h
│  ├─ task_list.h
│  ├─ linked_list.h
│  ├─ stack.h
│  ├─ queue.h
│  ├─ graph.h
│  ├─ sort.h
│  ├─ priority_queue.h
│  ├─ storage.h
│  └─ scheduler.h
├─ src/
│  ├─ main.c
│  ├─ task.c
│  ├─ task_list.c
│  ├─ linked_list.c
│  ├─ stack.c
│  ├─ queue.c
│  ├─ graph.c
│  ├─ sort.c
│  ├─ priority_queue.c
│  ├─ storage.c
│  └─ scheduler.c
├─ tests/
└─ data/
   ├─ sample_tasks.csv
   └─ sample_dependencies.csv
```

---

## 4. 개발 규칙

## 4.1 하루 진행 순서

```text
1. 오늘 WBS 확인
2. 필요한 개념 1~2개만 학습
3. 작은 기능 단위로 구현
4. 빌드 확인
5. 샘플 데이터로 실행
6. 테스트/예외 확인
7. 문서 업데이트
8. 오늘의 면접 질문 1~3개 작성
```

## 4.2 커밋 규칙 예시

```text
docs: add problem definition
feat: implement task list
feat: add stack ADT
feat: add graph dependency model
test: add queue edge case tests
refactor: separate scheduler module
docs: add complexity analysis
```

## 4.3 완료 기준

각 작업은 다음을 만족해야 완료로 본다.

- 빌드 성공
- 정상 케이스 실행 성공
- 예외 케이스 최소 1개 확인
- 관련 문서 업데이트
- 내가 말로 설명 가능

---

## 5. 일자별 WBS

## 8/7 금요일 - 기획 1일차

### 목표

프로젝트 주제, 문제 정의, MVP 범위를 확정한다.

### 할 일

- 프로젝트명 확정
- 프로젝트가 해결하려는 문제 정의
- 사용자가 입력할 데이터 정리
- 프로그램이 출력할 결과 정리
- MVP 필수 기능 8개 확정
- 하지 않을 기능 목록 작성
- 자료구조 적용 후보 작성

### 구현/작성 방법

- `문제 → 입력 → 처리 → 출력` 순서로 정리한다.
- 처음부터 기능을 크게 잡지 않는다.
- 발표할 때 한 문장으로 설명 가능한지 확인한다.

### 산출물

- `docs/02_design/problem-definition.md`
- `docs/00_plan/decision-log.md`
- `README.md` 초안

### 검증 방법

- 프로젝트를 1문장으로 설명할 수 있는가?
- 자료구조가 억지로 들어간 것이 아닌가?
- 8/28까지 구현 가능한 MVP인가?

---

## 8/8 토요일 - 기획 2일차

### 목표

자료구조와 알고리즘 설계를 문서화한다.

### 할 일

- `Task` 구조체 설계
- `TaskList` ADT 설계
- `Graph` ADT 설계
- `Stack`, `Queue`, `PriorityQueue` 함수 목록 작성
- 선행 작업 관계 표현 방식 결정
- 추천 점수 계산 기준 초안 작성

### 구현/작성 방법

- 코드 작성 전 함수 이름과 역할만 먼저 정한다.
- ADT 문서에는 내부 구조와 외부 제공 함수를 분리해서 쓴다.
- 자료구조별로 프로젝트 적용 위치를 표로 정리한다.

### 산출물

- `docs/02_design/adt-design.md`
- `docs/02_design/algorithm-design.md`
- `docs/02_design/module-structure.md`

### 검증 방법

- 각 자료구조가 어느 기능에 쓰이는지 설명 가능한가?
- 함수명만 보고 역할을 이해할 수 있는가?
- 순환 의존성 예시를 손으로 검증했는가?

---

## 8/9 일요일 - 기획 3일차

### 목표

8/10부터 바로 코딩할 수 있도록 기획 산출물을 마감한다.

### 할 일

- 입출력 시나리오 작성
- 샘플 작업 데이터 작성
- 샘플 의존성 데이터 작성
- 테스트 계획 작성
- 최종 디렉터리 구조 확정
- WBS 최종 점검

### 구현/작성 방법

- 실제 시연에 사용할 샘플 데이터를 미리 만든다.
- 예상 출력 결과를 손으로 계산한다.
- 테스트 케이스는 정상/예외/경계값으로 나눈다.

### 산출물

- `docs/02_design/io-scenario.md`
- `docs/03_verification/test-plan.md`
- `data/sample_tasks.csv`
- `data/sample_dependencies.csv`

### 검증 방법

- 샘플 데이터로 가능한 작업 순서를 손으로 계산했는가?
- 테스트 케이스에 순환 의존성 예시가 포함되어 있는가?
- 8/10에 바로 `main.c`를 만들 수 있는가?

---

## 8/10 월요일 - 개발환경 구성 + 프로젝트 뼈대

### 목표

C 프로젝트를 빌드하고 실행할 수 있는 상태를 만든다.

### 할 일

- VS Code C/C++ 확장 확인
- GCC 또는 MSVC 컴파일러 확인
- CMake 설치 확인
- 기본 폴더 생성
- `main.c` 작성
- `task.h`, `task.c` 작성
- Hello World 또는 메뉴 출력 빌드

### 구현 방법

- 처음에는 단일 파일로 컴파일한다.
- 이후 CMake로 빌드 구조를 전환한다.
- `main.c`에는 메뉴 출력만 둔다.
- 기능 구현은 `src/`, 선언은 `include/`에 둔다.

### 산출물

- `CMakeLists.txt`
- `src/main.c`
- `src/task.c`
- `include/task.h`
- `docs/01_learning/c-basic.md`

### 검증 방법

- 터미널에서 빌드 성공
- 실행 파일 실행 성공
- README에 빌드/실행 명령어 기록

---

## 8/11 화요일 - Task 구조체 + 배열 기반 저장

### 목표

작업 데이터를 구조체와 배열로 저장하고 조회한다.

### 할 일

- `Task` 구조체 정의
- 작업 추가 함수 구현
- 작업 목록 출력 함수 구현
- ID 중복 확인
- 작업 개수 관리

### 구현 방법

초기에는 고정 배열로 시작한다.

```c
#define MAX_TASKS 100
Task tasks[MAX_TASKS];
int task_count = 0;
```

구현 후보 함수:

```c
int add_task(Task tasks[], int *count, Task new_task);
void print_tasks(Task tasks[], int count);
int find_task_index_by_id(Task tasks[], int count, int id);
```

### 산출물

- `src/task.c`
- `include/task.h`
- `docs/01_learning/array-dynamic-array.md` 초안
- 작업 추가/조회 실행 로그

### 검증 방법

- 작업 0개 조회
- 작업 1개 추가 후 조회
- 여러 개 추가 후 조회
- ID 중복 입력
- 최대 개수 초과 입력

---

## 8/12 수요일 - 버퍼 1: C 복구/동적 배열 보강

### 목표

C 포인터, 구조체, 동적할당이 부족하면 복구한다.  
전날 작업이 밀리지 않았다면 동적 배열을 구현한다.

### 할 일

- 포인터/구조체 복습
- `malloc`, `realloc`, `free` 복습
- `TaskList` 동적 배열 설계 또는 구현
- 전날 문서 보강

### 구현 방법

진도가 괜찮으면 다음 구조로 개선한다.

```c
typedef struct {
    Task *items;
    int size;
    int capacity;
} TaskList;
```

### 산출물

- `src/task_list.c` 선택
- `include/task_list.h` 선택
- `docs/01_learning/pointer-struct-memory.md`
- 버퍼 사용 기록

### 검증 방법

- 이 날을 쉬어도 전체 일정이 무너지지 않아야 한다.
- 작업을 했다면 동적 배열 확장 테스트가 통과해야 한다.

---

## 8/13 목요일 - 연결리스트

### 목표

연결리스트를 구현하고 그래프 인접 리스트에 활용할 준비를 한다.

### 할 일

- Node 구조체 구현
- 리스트 삽입 구현
- 리스트 삭제 구현
- 리스트 탐색 구현
- 그래프 인접 리스트에 적용 가능성 검토

### 구현 방법

```c
typedef struct Node {
    int value;
    struct Node *next;
} Node;
```

구현 후보 함수:

```c
Node* list_push_front(Node *head, int value);
Node* list_remove(Node *head, int value);
int list_contains(Node *head, int value);
void list_free(Node *head);
```

### 산출물

- `src/linked_list.c`
- `include/linked_list.h`
- `docs/01_learning/linked-list.md`

### 검증 방법

- 빈 리스트 삽입
- 첫 노드 삭제
- 중간 노드 삭제
- 없는 값 삭제
- 전체 free 확인

---

## 8/14 금요일 - 스택/큐 1차 구현

### 목표

스택과 큐를 구현한다.

### 할 일

- 배열 기반 스택 구현
- 원형 큐 구현
- overflow/underflow 처리
- 간단 테스트 작성

### 구현 방법

스택 후보 함수:

```c
int stack_push(Stack *stack, int value);
int stack_pop(Stack *stack, int *out_value);
int stack_is_empty(Stack *stack);
```

큐 후보 함수:

```c
int queue_enqueue(Queue *queue, int value);
int queue_dequeue(Queue *queue, int *out_value);
int queue_is_empty(Queue *queue);
```

### 산출물

- `src/stack.c`
- `include/stack.h`
- `src/queue.c`
- `include/queue.h`
- `docs/01_learning/stack-queue.md`

### 검증 방법

- 스택 LIFO 확인
- 큐 FIFO 확인
- 빈 스택 pop 오류 처리
- 빈 큐 dequeue 오류 처리
- 가득 찬 큐 enqueue 오류 처리

---

## 8/15 토요일 - 그래프 기본 구조

### 목표

작업 간 선행 관계를 방향 그래프로 표현한다.

### 할 일

- Graph 구조체 설계
- 인접 리스트 또는 인접 배열 선택
- `add_dependency(from, to)` 구현
- 진입차수 배열 구현
- 의존성 출력 함수 구현

### 구현 방법

작업 A가 끝나야 작업 B가 가능하면 다음 방향으로 저장한다.

```text
A -> B
```

구현 후보 함수:

```c
int graph_add_edge(Graph *graph, int from, int to);
void graph_print(Graph *graph);
int graph_get_indegree(Graph *graph, int node);
```

### 산출물

- `src/graph.c`
- `include/graph.h`
- `docs/01_learning/graph-topological-sort.md` 초안

### 검증 방법

- A -> B 관계 추가
- A -> B, A -> C 관계 추가
- 존재하지 않는 작업 ID 오류 처리
- 자기 자신 의존성 오류 처리

---

## 8/16 일요일 - 버퍼 2: 자료구조 구현 복구

### 목표

연결리스트, 스택, 큐, 그래프 기본 구현 중 밀린 부분을 복구한다.

### 할 일

- 밀린 구현 보완
- 문서 보강
- 테스트 케이스 추가
- 포인터 오류 정리

### 산출물

- 버퍼 사용 기록
- `docs/00_plan/daily-log.md`
- 수정된 자료구조 코드

### 검증 방법

- 스택/큐/연결리스트 중 최소 2개는 테스트 완료 상태여야 한다.
- 그래프 기본 구조가 빌드되어야 한다.

---

## 8/17 월요일 - DFS 순환 의존성 검사

### 목표

잘못된 작업 의존성 순환을 감지한다.

### 할 일

- DFS 개념 정리
- 방문 상태 3단계 구현
- cycle detection 구현
- 순환 발생 시 오류 메시지 출력

### 구현 방법

방문 상태:

```c
typedef enum {
    UNVISITED,
    VISITING,
    VISITED
} VisitState;
```

순환 예시:

```text
A -> B -> C -> A
```

### 산출물

- `graph_has_cycle()` 구현
- `docs/03_verification/test-case.md` 업데이트
- 순환 의존성 테스트 로그

### 검증 방법

- 순환 없는 그래프 false
- 단순 순환 true
- 자기 자신 순환 true
- 분리된 그래프 정상 처리

---

## 8/18 화요일 - 위상 정렬

### 목표

선행 관계를 고려해 가능한 작업 순서를 계산한다.

### 할 일

- Kahn 알고리즘 학습
- 진입차수 0인 작업 큐 삽입
- 큐 기반 위상 정렬 구현
- 결과 순서 출력

### 구현 방법

흐름:

```text
1. 모든 작업의 indegree 계산
2. indegree가 0인 작업을 큐에 삽입
3. 큐에서 하나씩 꺼내 결과에 추가
4. 연결된 작업의 indegree 감소
5. 새롭게 indegree가 0이 된 작업을 큐에 삽입
6. 결과 개수가 전체 작업 수와 다르면 cycle 의심
```

### 산출물

- `scheduler_topological_sort()` 구현
- 위상 정렬 실행 결과
- `docs/01_learning/graph-topological-sort.md` 보강

### 검증 방법

- A -> B -> C 순서 확인
- A -> C, B -> C 구조 확인
- 선행 관계 없는 작업 포함 확인
- 순환 그래프 실패 처리

---

## 8/19 수요일 - 정렬 + CLI 메뉴 개선

### 목표

작업 목록을 기준별로 정렬하고 CLI 메뉴를 사용할 수 있게 만든다.

### 할 일

- 마감일순 정렬
- 우선순위순 정렬
- 예상 소요시간순 정렬
- CLI 메뉴 기본 흐름 정리

### 구현 방법

처음에는 구현이 쉬운 삽입정렬 또는 선택정렬을 사용한다.  
시간이 남으면 퀵정렬을 추가한다.

구현 후보 함수:

```c
void sort_tasks_by_due_date(TaskList *list);
void sort_tasks_by_priority(TaskList *list);
void sort_tasks_by_estimated_hours(TaskList *list);
```

### 산출물

- `src/sort.c`
- `include/sort.h`
- CLI 메뉴 실행 로그
- `docs/01_learning/sorting-heap-priority-queue.md` 초안

### 검증 방법

- 이미 정렬된 데이터
- 역순 데이터
- 같은 우선순위 데이터
- 빈 목록
- 작업 1개 목록

---

## 8/20 목요일 - 힙/우선순위 큐 + 오늘 할 일 추천

### 목표

추천 후보 작업을 우선순위 큐에 넣고 오늘 할 일을 출력한다.

### 할 일

- 힙 구조 학습
- 우선순위 큐 구현
- 추천 점수 계산식 작성
- 추천 작업 N개 출력

### 구현 방법

추천 후보 조건:

```text
1. 아직 완료되지 않은 작업
2. 선행 작업이 모두 완료된 작업
3. 마감일 또는 우선순위가 있는 작업
```

점수 예시:

```text
score = priority * 10 + due_date_weight - estimated_hours_weight
```

구현 후보 함수:

```c
int calculate_task_score(Task task);
void recommend_today_tasks(TaskList *tasks, Graph *graph, int limit);
```

### 산출물

- `src/priority_queue.c`
- `include/priority_queue.h`
- `src/scheduler.c`
- `include/scheduler.h`
- 추천 결과 실행 로그

### 검증 방법

- 선행 작업 미완료 작업 제외
- 완료된 작업 제외
- 우선순위 높은 작업 우선 추천
- 마감일 가까운 작업 우선 추천

---

## 8/21 금요일 - 버퍼 3: 그래프/추천 통합 복구

### 목표

그래프, 위상 정렬, 우선순위 큐 통합에서 발생한 지연을 복구한다.

### 할 일

- 빌드 오류 수정
- 추천 로직 보정
- 통합 실행 시나리오 점검
- 문서 보강

### 산출물

- 버퍼 사용 기록
- 통합 실행 로그
- `docs/02_design/io-scenario.md` 업데이트

### 검증 방법

- 샘플 데이터로 전체 흐름이 실행되어야 한다.
- 작업 추가 → 의존성 추가 → 순서 계산 → 추천 출력 흐름이 가능해야 한다.

---

## 8/22 토요일 - CSV 저장/불러오기

### 목표

작업과 의존성 데이터를 CSV 파일로 저장하고 불러온다.

### 할 일

- `tasks.csv` 형식 확정
- `dependencies.csv` 형식 확정
- 작업 저장 함수 구현
- 작업 불러오기 함수 구현
- 의존성 저장/불러오기 구현

### 구현 방법

파일 형식:

```csv
id,title,priority,due_date,estimated_hours,status
1,C 포인터 복습,5,2026-08-10,2,DONE
2,스택 구현,4,2026-08-14,3,TODO
```

구현 후보 함수:

```c
int save_tasks_to_csv(const char *path, TaskList *tasks);
int load_tasks_from_csv(const char *path, TaskList *tasks);
int save_dependencies_to_csv(const char *path, Graph *graph);
int load_dependencies_from_csv(const char *path, Graph *graph);
```

### 산출물

- `src/storage.c`
- `include/storage.h`
- `data/sample_tasks.csv`
- `data/sample_dependencies.csv`
- `docs/01_learning/file-io-csv.md`

### 검증 방법

- 저장 후 파일 내용 확인
- 프로그램 재실행 후 불러오기 확인
- 빈 파일 처리
- 잘못된 CSV 형식 처리

---

## 8/23 일요일 - 입력 검증/예외 처리

### 목표

잘못된 입력에도 프로그램이 비정상 종료되지 않게 만든다.

### 할 일

- 메뉴 입력 검증
- 숫자 입력 검증
- 없는 작업 ID 처리
- 중복 의존성 처리
- 자기 자신 의존성 처리
- 파일 없음 처리

### 구현 방법

- 입력 처리 함수를 분리한다.
- 성공/실패를 반환값으로 구분한다.
- 오류 메시지 형식을 통일한다.

예시:

```text
[ERROR] 존재하지 않는 작업 ID입니다.
[ERROR] 자기 자신을 선행 작업으로 등록할 수 없습니다.
[ERROR] 순환 의존성이 발생합니다.
```

### 산출물

- 입력 검증 코드
- `docs/03_verification/test-case.md` 보강
- `docs/03_verification/error-case.md`

### 검증 방법

- 문자 입력
- 빈 입력
- 범위 밖 숫자
- 없는 ID
- 중복 관계
- 파일 없음

---

## 8/24 월요일 - 버퍼 4: 저장/예외 처리 안정화

### 목표

CSV 저장과 예외 처리에서 밀린 부분을 복구한다.

### 할 일

- CSV 파싱 오류 수정
- 입력 검증 보강
- SQLite 확장 여부 결정
- README 초안 보강

### SQLite 결정 기준

| 조건 | 선택 |
|---|---|
| 핵심 기능이 모두 안정적이다 | SQLite 학습/실험 가능 |
| 그래프/추천/CSV 중 하나라도 불안정하다 | SQLite 포기, CSV 안정화 |

### 산출물

- 버퍼 사용 기록
- `docs/00_plan/decision-log.md` 업데이트
- README 실행 방법 초안

### 검증 방법

- SQLite를 하지 않기로 해도 실패가 아니다.
- CSV 기반 MVP가 안정적이면 목표에 충분하다.

---

## 8/25 화요일 - 테스트 케이스 정리

### 목표

핵심 기능별 테스트를 정리하고 실행한다.

### 할 일

- 자료구조별 테스트 케이스 정리
- assert 기반 테스트 작성
- 가능하면 Unity 도입 검토
- 실패 테스트 수정
- 테스트 실행 방법 문서화

### 구현 방법

테스트 대상:

```text
1. TaskList
2. Stack
3. Queue
4. Graph
5. Cycle Detection
6. Topological Sort
7. Priority Queue
8. CSV Storage
```

### 산출물

- `tests/` 코드
- `docs/03_verification/test-case.md`
- 테스트 실행 로그

### 검증 방법

- 정상 케이스 통과
- 예외 케이스 통과
- 경계값 케이스 통과
- 실패한 테스트는 원인과 수정 내용을 기록

---

## 8/26 수요일 - 복잡도 분석 + 메모리 점검 + README 초안

### 목표

자료구조 프로젝트답게 분석 문서를 완성한다.

### 할 일

- 주요 기능별 시간복잡도 정리
- 공간복잡도 정리
- malloc/free 위치 점검
- README 구조 작성
- 면접 질문 초안 작성

### 구현/작성 방법

복잡도 표 예시:

| 기능 | 자료구조 | 시간복잡도 | 설명 |
|---|---|---|---|
| 작업 ID 검색 | 배열 | O(n) | 전체 작업을 순회 |
| 작업 추천 | 힙 | O(n log n) | 후보 n개를 힙에 삽입 |
| 위상 정렬 | 그래프+큐 | O(V+E) | 정점과 간선을 한 번씩 처리 |

### 산출물

- `docs/03_verification/complexity-check.md`
- `docs/03_verification/memory-check.md`
- `README.md` 초안
- `docs/05_interview/interview-qna.md` 초안

### 검증 방법

- 코드 흐름과 복잡도 설명이 일치하는가?
- 모든 malloc에 대응하는 free가 있는가?
- README만 보고 실행 흐름을 이해할 수 있는가?

---

## 8/27 목요일 - 버퍼 5: 최종 복구/문서 정리

### 목표

최종 마감 전 부족한 부분을 복구한다.

### 할 일

- 미완성 핵심 기능 복구
- README 보강
- 블로그 초안 작성
- 발표 시나리오 작성
- 면접 Q&A 보강

### 산출물

- 버퍼 사용 기록
- `docs/04_blog-drafts/project-retrospective.md`
- `docs/05_interview/interview-qna.md`
- `docs/02_design/demo-scenario.md`

### 검증 방법

- 핵심 기능 8개 중 완료/미완료를 명확히 표시한다.
- 미완성 선택 기능은 과감히 제외한다.
- 발표 흐름이 3분 안에 가능한지 확인한다.

---

## 8/28 금요일 - 최종 마감

### 목표

기능을 동결하고 발표/면접 대비 가능한 상태로 정리한다.

### 할 일

- 전체 빌드
- 전체 테스트
- 샘플 데이터 실행
- README 최종 수정
- 문서 링크 확인
- 최종 회고 작성
- GitHub 커밋 정리

### 산출물

- 최종 `README.md`
- 최종 소스 코드
- 최종 테스트 결과
- `docs/04_blog-drafts/final-retrospective.md`
- `docs/05_interview/interview-qna.md`

### 검증 방법

- 새 폴더에서 clone 후 빌드 가능해야 한다.
- 샘플 데이터로 시연 가능해야 한다.
- 자료구조 적용 이유를 설명할 수 있어야 한다.
- 미완성 기능이 README에 명확히 적혀 있어야 한다.

---

## 6. 최종 발표/면접 대비 질문

최종적으로 다음 질문에 답할 수 있어야 한다.

## 6.1 프로젝트 질문

- 이 프로젝트는 어떤 문제를 해결하나요?
- 왜 웹 프로젝트가 아니라 C CLI 프로젝트로 만들었나요?
- 왜 CSV를 먼저 선택했나요?
- 이 프로젝트에서 가장 중요한 자료구조는 무엇인가요?
- 가장 어려웠던 구현은 무엇인가요?

## 6.2 자료구조 질문

- 배열과 연결리스트의 차이는 무엇인가요?
- 스택과 큐는 각각 어디에 사용했나요?
- 그래프를 사용한 이유는 무엇인가요?
- 위상 정렬은 어떤 조건에서 가능한가요?
- 힙을 사용한 이유는 무엇인가요?

## 6.3 C언어 질문

- 포인터를 어디에 사용했나요?
- 동적 메모리 할당을 왜 사용했나요?
- 메모리 누수를 어떻게 방지했나요?
- `.h`와 `.c` 파일을 왜 분리했나요?
- Java로 만들 때와 C로 만들 때 어떤 점이 달랐나요?

## 6.4 검증 질문

- 테스트 케이스는 어떻게 설계했나요?
- 순환 의존성은 어떻게 검출했나요?
- 시간복잡도는 어떻게 분석했나요?
- 잘못된 입력은 어떻게 처리했나요?

---

## 7. 리스크 대응표

| 리스크 | 증상 | 대응 |
|---|---|---|
| C 포인터에서 막힘 | 세그멘테이션 오류, 값 이상 | 8/12 버퍼 사용, 작은 예제로 분리 |
| 그래프 구현 지연 | 의존성 관계가 꼬임 | 인접 행렬로 단순화 가능 |
| 힙 구현 지연 | 추천 기능 미완성 | 정렬 기반 추천으로 대체 |
| CSV 파싱 지연 | 파일 읽기 오류 | 샘플 데이터 형식 단순화 |
| 테스트 도입 지연 | Unity 설정 실패 | assert 기반 테스트로 대체 |
| 문서화 지연 | README 부족 | 선택 기능 포기, 핵심 기능 설명 우선 |

---

## 8. 마감 기준

이 프로젝트는 다음 조건을 만족하면 완료로 본다.

- C 기반 CLI 프로그램이 빌드된다.
- 작업 추가/조회가 가능하다.
- 선행 작업 관계를 추가할 수 있다.
- 순환 의존성을 감지할 수 있다.
- 가능한 작업 순서를 출력할 수 있다.
- 오늘 할 일을 추천할 수 있다.
- CSV 저장/불러오기가 가능하다.
- README에 실행 방법이 있다.
- 주요 자료구조 적용 이유와 복잡도 분석이 문서화되어 있다.
- 발표/면접 예상 질문에 답변할 수 있다.
