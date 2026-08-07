# 로컬 Codex / 프로젝트 문서화 규칙

> 저장소 목적: C 언어로 자료구조를 학습하고, 학습한 자료구조를 `학습/WBS 일정 최적화기` 사이드 프로젝트에 적용한다.  
> 이 문서는 로컬 Codex, VS Code, SourceTree, GitHub에서 작업할 때 기준으로 삼을 프로젝트 규칙이다.

---

## 1. 저장소 기본 정보

### 1.1 추천 레포 이름

```text
c-data-structure-roadmap-scheduler
```

### 1.2 GitHub Repository Description

```text
C 기반 자료구조 학습과 작업 의존성/우선순위 기반 일정 추천 CLI 프로젝트
```

영문으로 쓰고 싶다면 다음을 사용한다.

```text
A C-based data structure learning repository and CLI roadmap scheduler using task dependencies and priority queues.
```

### 1.3 저장소 운영 목적

이 저장소는 단순히 C 코드만 올리는 저장소가 아니다.

- 자료구조 수업 전 C 기반 자료구조를 한 바퀴 예습한다.
- 학습한 내용을 Markdown으로 정리한다.
- 직접 구현한 자료구조를 사이드 프로젝트 기능에 연결한다.
- 최종적으로 발표와 면접에서 설명 가능한 수준까지 정리한다.
- README와 문서 구조를 포트폴리오용으로 정리한다.

### 1.4 공개 범위

현재 저장소는 `Public`으로 운영해도 된다.

단, README에 다음 의도를 명확히 적는다.

```md
> 이 저장소는 자료구조 수업 전 C 언어 기반 자료구조를 학습하고,
> 이를 사이드 프로젝트에 적용하는 과정을 기록하는 학습용 저장소입니다.
```

---

## 2. 프로젝트 구조 규칙

### 2.1 최종 권장 구조

```text
c-data-structure-roadmap-scheduler/
├─ README.md
├─ LICENSE
├─ .gitignore
├─ .gitattributes
├─ CMakeLists.txt
│
├─ docs/
│  ├─ README.md
│  │
│  ├─ learning/
│  │  ├─ README.md
│  │  ├─ 00-learning-roadmap.md
│  │  ├─ 01-c-basic.md
│  │  ├─ 02-pointer-struct-memory.md
│  │  ├─ 03-array-dynamic-array.md
│  │  ├─ 04-linked-list.md
│  │  ├─ 05-stack-queue.md
│  │  ├─ 06-tree.md
│  │  ├─ 07-graph.md
│  │  ├─ 08-sorting.md
│  │  ├─ 09-complexity.md
│  │  └─ 10-interview-notes.md
│  │
│  ├─ project/
│  │  ├─ README.md
│  │  ├─ 00-wbs-development.md
│  │  ├─ 01-problem-definition.md
│  │  ├─ 02-requirements.md
│  │  ├─ 03-adt-design.md
│  │  ├─ 04-algorithm-design.md
│  │  ├─ 05-module-structure.md
│  │  ├─ 06-io-scenario.md
│  │  ├─ 07-test-plan.md
│  │  ├─ 08-complexity-analysis.md
│  │  ├─ 09-memory-management.md
│  │  └─ 10-retrospective.md
│  │
│  ├─ daily-log/
│  │  ├─ README.md
│  │  └─ 2026-08.md
│  │
│  ├─ troubleshooting/
│  │  ├─ README.md
│  │  └─ build-debug-memory.md
│  │
│  └─ blog-drafts/
│     ├─ README.md
│     ├─ 01-c-pointer-struct.md
│     ├─ 02-stack-queue.md
│     ├─ 03-graph-topological-sort.md
│     └─ 04-project-retrospective.md
│
├─ include/
│  ├─ task.h
│  ├─ task_list.h
│  ├─ linked_list.h
│  ├─ stack.h
│  ├─ queue.h
│  ├─ graph.h
│  ├─ priority_queue.h
│  ├─ scheduler.h
│  └─ storage.h
│
├─ src/
│  ├─ main.c
│  ├─ task.c
│  ├─ task_list.c
│  ├─ linked_list.c
│  ├─ stack.c
│  ├─ queue.c
│  ├─ graph.c
│  ├─ priority_queue.c
│  ├─ scheduler.c
│  └─ storage.c
│
├─ tests/
│  ├─ README.md
│  ├─ test_task_list.c
│  ├─ test_stack.c
│  ├─ test_queue.c
│  ├─ test_graph.c
│  └─ test_scheduler.c
│
├─ data/
│  ├─ sample_tasks.csv
│  └─ sample_dependencies.csv
│
└─ scripts/
   ├─ build.sh
   ├─ run.sh
   └─ clean.sh
```

### 2.2 현재 단계에서 최소로 필요한 구조

현재는 아래 구조까지만 있어도 된다.

```text
c-data-structure-roadmap-scheduler/
├─ README.md
├─ .gitignore
├─ .gitattributes
├─ CMakeLists.txt
├─ docs/
│  ├─ learning/
│  │  └─ 00-learning-roadmap.md
│  ├─ project/
│  │  └─ 00-wbs-development.md
│  ├─ daily-log/
│  │  └─ 2026-08.md
│  ├─ troubleshooting/
│  └─ blog-drafts/
├─ include/
├─ src/
│  └─ main.c
├─ tests/
├─ data/
└─ scripts/
```

---

## 3. 학습 문서와 프로젝트 문서 분리 규칙

### 3.1 `docs/learning/`

`docs/learning/`은 공부한 내용을 정리하는 공간이다.

여기에는 다음 내용을 적는다.

- C 문법 복습
- 포인터
- 구조체
- 동적 메모리
- 배열
- 연결리스트
- 스택
- 큐
- 트리
- 그래프
- 정렬
- 시간복잡도
- 면접 질문 정리

예시:

```text
docs/learning/05-stack-queue.md
```

이 파일에는 스택과 큐의 개념, 직접 구현 코드 흐름, 시간복잡도, 헷갈린 점, 면접 질문을 정리한다.

### 3.2 `docs/project/`

`docs/project/`는 실제 사이드 프로젝트 설계와 검증 문서를 모으는 공간이다.

여기에는 다음 내용을 적는다.

- 문제 정의
- 기능 요구사항
- ADT 설계
- 알고리즘 설계
- 모듈 구조
- 입출력 시나리오
- 테스트 계획
- 복잡도 분석
- 메모리 관리 규칙
- 회고

예시:

```text
docs/project/04-algorithm-design.md
```

이 파일에는 프로젝트에서 작업 의존성을 그래프로 표현하고, 위상 정렬로 실행 가능한 순서를 계산하는 방식을 정리한다.

### 3.3 같은 주제라도 위치를 구분한다

| 주제 | 학습 문서 | 프로젝트 문서 |
|---|---|---|
| 그래프 | 그래프 개념, 인접 리스트, DFS, BFS | 작업 의존성 모델링, 사이클 탐지, 위상 정렬 |
| 스택 | LIFO, push/pop, 배열 기반 구현 | DFS 보조 구조 또는 Undo 기능 적용 |
| 큐 | FIFO, 원형 큐 | 위상 정렬에서 진입차수 0 작업 처리 |
| 힙 | 완전 이진 트리, heapify | 오늘 할 일 추천 우선순위 큐 |

---

## 4. 로컬 개발 환경 규칙

### 4.1 현재 환경

Windows 기준으로 다음 환경을 사용한다.

```text
Editor: VS Code
Git GUI: SourceTree
Compiler: GCC from MSYS2 UCRT64
Debugger: GDB from MSYS2 UCRT64
Build System: CMake
Build Tool: Ninja
Terminal: VS Code Terminal / PowerShell / MSYS2 UCRT64
```

### 4.2 PATH 규칙

MSYS2 설치 경로가 `C:\tools\MSYS2`라면 시스템 환경 변수 Path에 다음이 있어야 한다.

```text
C:\tools\MSYS2\ucrt64\bin
```

PowerShell 또는 VS Code 터미널을 새로 열고 아래 명령으로 확인한다.

```powershell
gcc --version
gdb --version
cmake --version
ninja --version
```

모두 버전이 출력되면 정상이다.

### 4.3 설치 확인 기준

아래 명령이 모두 성공해야 개발환경 설정이 완료된 것으로 본다.

```powershell
gcc --version
gdb --version
cmake --version
ninja --version
git --version
```

---

## 5. C 프로젝트 실행 규칙

### 5.1 가장 단순한 실행 방식

처음에는 `gcc`로 단일 파일을 바로 컴파일한다.

```powershell
gcc .\src\main.c -o roadmap_scheduler.exe
.\roadmap_scheduler.exe
```

이 방식은 환경이 정상인지 확인하는 용도다.

### 5.2 CMake 실행 방식

C 파일이 여러 개로 늘어나면 CMake를 사용한다.

```powershell
cmake -S . -B build -G Ninja
cmake --build build
.\build\roadmap_scheduler.exe
```

### 5.3 CMake 명령 의미

| 명령 | 의미 |
|---|---|
| `cmake -S . -B build -G Ninja` | 현재 폴더의 CMakeLists.txt를 읽고 build 폴더에 빌드 설정 생성 |
| `cmake --build build` | build 폴더의 설정을 기준으로 실제 컴파일 |
| `.\build\roadmap_scheduler.exe` | 컴파일된 실행 파일 실행 |

### 5.4 VS Code에서 출력 확인 위치

C 프로그램 실행 결과는 VS Code 하단의 `TERMINAL` 탭에서 확인한다.

| 탭 | 용도 |
|---|---|
| TERMINAL | 명령어 입력, 프로그램 실행 결과 확인 |
| PROBLEMS | 컴파일 오류, 경고, 문법 문제 확인 |
| OUTPUT | VS Code 확장 프로그램 로그 확인 |
| DEBUG CONSOLE | 디버깅 중 변수/표현식 확인 |

처음에는 `TERMINAL`만 보면 된다.

---

## 6. C 코드 작성 규칙

### 6.1 기본 언어 표준

```text
C11
```

`CMakeLists.txt`에는 다음을 사용한다.

```cmake
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
```

### 6.2 파일 분리 규칙

| 파일 | 역할 |
|---|---|
| `.h` | 구조체 정의, 함수 선언 |
| `.c` | 함수 구현 |
| `main.c` | 프로그램 시작점, 메뉴 흐름 |

예시:

```text
include/stack.h
src/stack.c
```

### 6.3 헤더 가드 규칙

모든 `.h` 파일에는 헤더 가드를 사용한다.

```c
#ifndef STACK_H
#define STACK_H

// declarations

#endif
```

### 6.4 함수 반환 규칙

실패 가능성이 있는 함수는 성공/실패를 반환한다.

```c
int stack_push(Stack *stack, int value);
int stack_pop(Stack *stack, int *out_value);
```

권장 반환 규칙:

```text
1 = 성공
0 = 실패
```

또는 프로젝트 전체에서 일관되게 다음을 사용할 수 있다.

```text
0 = 성공
음수 = 실패
```

둘 중 하나를 정하면 전체 코드에서 섞지 않는다.

### 6.5 메모리 관리 규칙

`malloc`, `calloc`, `realloc`을 사용한 경우 반드시 해제 책임을 문서화한다.

```c
void task_list_free(TaskList *list);
void stack_free(Stack *stack);
void queue_free(Queue *queue);
void graph_free(Graph *graph);
```

규칙:

- 할당한 모듈이 해제 함수도 제공한다.
- `free` 후 포인터를 `NULL`로 정리한다.
- 실패 가능성이 있는 동적 할당은 `NULL` 체크를 한다.
- 메모리 해제 위치를 `docs/project/09-memory-management.md`에 기록한다.

### 6.6 네이밍 규칙

| 대상 | 규칙 | 예시 |
|---|---|---|
| 파일명 | snake_case | `task_list.c` |
| 함수명 | snake_case | `task_list_add` |
| 구조체명 | PascalCase | `TaskList` |
| 상수 | UPPER_SNAKE_CASE | `MAX_TASK_TITLE_LENGTH` |
| 변수 | snake_case | `task_count` |

### 6.7 주석 규칙

주석은 모든 줄에 과도하게 달지 않는다.

주석이 필요한 경우:

- 포인터 소유권이 헷갈릴 수 있는 부분
- 복잡도에 영향을 주는 반복문
- DFS, 위상 정렬, heapify 같은 알고리즘 핵심 부분
- 예외 처리 의도가 필요한 부분

---

## 7. 자료구조 구현 규칙

### 7.1 직접 구현 우선

이번 프로젝트의 목적은 자료구조 학습이므로 주요 자료구조는 직접 구현한다.

직접 구현 대상:

- 동적 배열
- 연결리스트
- 스택
- 큐
- 그래프
- 우선순위 큐
- 정렬 함수

### 7.2 라이브러리 사용 제한

C 표준 라이브러리는 사용할 수 있다.

사용 가능:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdbool.h>
```

자료구조 자체를 외부 라이브러리로 대체하지 않는다.

### 7.3 각 자료구조별 문서 필수 항목

각 자료구조를 구현할 때 `docs/learning/`에 다음을 기록한다.

```md
## 개념 요약

## ADT 함수 목록

## 구현 방식

## 시간복잡도

## 공간복잡도

## 프로젝트 적용 위치

## 테스트 케이스

## 면접 질문
```

---

## 8. 테스트 및 검증 규칙

### 8.1 테스트 우선순위

처음에는 `assert` 기반 테스트로 시작한다.

이후 시간이 남으면 Unity Test Framework를 추가한다.

### 8.2 테스트 파일 규칙

테스트 파일은 `tests/`에 둔다.

```text
tests/test_stack.c
tests/test_queue.c
tests/test_graph.c
tests/test_scheduler.c
```

### 8.3 최소 테스트 기준

각 자료구조는 최소 다음 테스트를 가진다.

| 구분 | 예시 |
|---|---|
| 정상 케이스 | push 후 pop |
| 예외 케이스 | 빈 스택 pop |
| 경계값 | capacity 초과 |
| 순서 검증 | Stack은 LIFO, Queue는 FIFO |

### 8.4 기능 검증 기준

프로젝트 기능은 다음 기준을 만족해야 한다.

- 작업 추가 후 조회 가능
- 중복 ID 처리 가능
- 없는 작업 ID 입력 시 오류 처리
- 작업 의존성 추가 가능
- 자기 자신 의존성 거부
- 순환 의존성 감지
- 위상 정렬 결과 출력
- 선행 작업이 남은 작업은 추천 대상에서 제외
- CSV 저장/불러오기 가능

---

## 9. CSV 저장 규칙

### 9.1 CSV 의미

CSV는 이미지 파일이 아니다.

CSV는 `Comma-Separated Values`의 약자로, 쉼표로 값을 구분하는 텍스트 파일이다.

예시:

```csv
id,title,priority,due_date,status
1,C 포인터 복습,5,2026-08-10,TODO
2,스택 구현,4,2026-08-14,TODO
```

### 9.2 파일 위치

```text
data/sample_tasks.csv
data/sample_dependencies.csv
```

### 9.3 작업 CSV 형식

```csv
id,title,priority,due_date,estimated_hours,status
1,C pointer review,5,2026-08-10,3,TODO
2,Implement stack,4,2026-08-14,2,TODO
```

### 9.4 의존성 CSV 형식

```csv
from_task_id,to_task_id
1,2
```

의미:

```text
1번 작업이 끝나야 2번 작업을 할 수 있다.
```

---

## 10. Git / SourceTree 운영 규칙

### 10.1 브랜치 전략

혼자 진행하므로 복잡한 브랜치 전략은 사용하지 않는다.

권장:

```text
main
feature/docs-setup
feature/c-project-skeleton
feature/task-list
feature/stack-queue
feature/graph-scheduler
feature/storage-csv
```

처음에는 `main`에서 바로 작업해도 되지만, 기능 단위로 정리하고 싶으면 `feature/*` 브랜치를 사용한다.

### 10.2 커밋 메시지 규칙

형식:

```text
type: message
```

사용할 type:

| type | 의미 |
|---|---|
| docs | 문서 추가/수정 |
| chore | 설정, 구조 생성 |
| feat | 기능 구현 |
| fix | 오류 수정 |
| test | 테스트 추가/수정 |
| refactor | 동작 변경 없는 구조 개선 |

예시:

```text
docs: add initial learning roadmap and wbs
chore: initialize c project skeleton
feat: implement task list with dynamic array
feat: implement stack using array
test: add stack test cases
docs: add stack and queue learning notes
fix: handle empty queue dequeue error
```

### 10.3 SourceTree 사용 규칙

- 커밋 전 변경 파일을 확인한다.
- 불필요한 빌드 산출물은 커밋하지 않는다.
- `build/`, `.exe`, `.o` 파일은 `.gitignore`로 제외한다.
- 문서와 코드는 가능하면 커밋을 분리한다.
- Push 전 README 링크가 깨지지 않았는지 확인한다.

---

## 11. `.gitignore` 권장 내용

```gitignore
# Build outputs
build/
cmake-build-*/
out/
bin/
obj/

# Compiled files
*.exe
*.o
*.obj
*.dll
*.so
*.dylib
*.a
*.lib

# CMake
CMakeCache.txt
CMakeFiles/
cmake_install.cmake
Makefile
compile_commands.json

# VS Code
.vscode/
!.vscode/extensions.json

# OS
.DS_Store
Thumbs.db

# Logs
*.log

# Temporary files
*.tmp
*.bak
```

---

## 12. `.gitattributes` 권장 내용

```gitattributes
* text=auto

*.c text eol=lf
*.h text eol=lf
*.md text eol=lf
*.txt text eol=lf
*.csv text eol=lf
*.cmake text eol=lf
CMakeLists.txt text eol=lf
```

---

## 13. README.md 초안

아래 내용을 루트 `README.md`에 사용한다.

````md
# C Data Structure Roadmap Scheduler

> 이 저장소는 자료구조 수업 전 C 언어 기반 자료구조를 학습하고, 이를 사이드 프로젝트에 적용하는 과정을 기록하는 학습용 저장소입니다.

## 1. 프로젝트 개요

C 언어로 자료구조를 학습하고, 학습/개발 작업의 선후행 관계를 분석하는 일정 추천 CLI 프로젝트입니다.

사용자는 작업, 예상 소요 시간, 마감일, 우선순위, 선행 작업 관계를 입력할 수 있습니다.  
프로그램은 작업 의존성을 그래프로 분석하고, 위상 정렬과 우선순위 큐를 활용해 가능한 작업 순서와 오늘 해야 할 작업을 추천합니다.

## 2. 목표

- 자료구조 수업 전 C 기반 자료구조를 한 바퀴 학습한다.
- 배열, 연결리스트, 스택, 큐, 트리, 그래프, 정렬을 직접 구현한다.
- 작업 간 의존성을 그래프로 표현하고 위상 정렬로 가능한 작업 순서를 계산한다.
- 우선순위 큐를 활용해 오늘 해야 할 작업을 추천한다.
- 학습 내용과 프로젝트 적용 과정을 문서화한다.
- 발표와 면접에서 자료구조 적용 이유를 설명할 수 있도록 정리한다.

## 3. 핵심 자료구조

- Array / Dynamic Array
- Linked List
- Stack
- Queue
- Tree
- Graph
- Priority Queue
- Sorting

## 4. 프로젝트 구조

```text
docs/
  learning/        # 자료구조 학습 정리
  project/         # 프로젝트 설계/검증 문서
  daily-log/       # 일일 학습/개발 기록
  troubleshooting/ # 오류 해결 기록
  blog-drafts/     # Velog 업로드 전 Markdown 초안

src/               # C 구현 파일
include/           # C 헤더 파일
tests/             # 테스트 코드
data/              # 샘플 CSV 데이터
scripts/           # 빌드/실행 스크립트
```

## 5. 실행 환경

```text
OS: Windows
Editor: VS Code
Compiler: GCC from MSYS2 UCRT64
Debugger: GDB
Build System: CMake
Build Tool: Ninja
Git GUI: SourceTree
```

## 6. 실행 방법

### 6.1 단일 파일 테스트

```powershell
gcc .\src\main.c -o roadmap_scheduler.exe
.\roadmap_scheduler.exe
```

### 6.2 CMake 빌드

```powershell
cmake -S . -B build -G Ninja
cmake --build build
.\build\roadmap_scheduler.exe
```

## 7. 문서

- [학습 로드맵](docs/learning/00-learning-roadmap.md)
- [개발 WBS](docs/project/00-wbs-development.md)

## 8. 현재 상태

- [x] 저장소 구조 생성
- [x] 학습 로드맵 문서 추가
- [x] 개발 WBS 문서 추가
- [ ] C 프로젝트 기본 실행 확인
- [ ] Task 구조체 설계
- [ ] 동적 배열 기반 TaskList 구현
- [ ] Stack / Queue 구현
- [ ] Graph / Topological Sort 구현
- [ ] Priority Queue 기반 추천 기능 구현
- [ ] CSV 저장/불러오기 구현
- [ ] 최종 README 정리
````

---

## 14. 로컬 Codex에게 알려줄 작업 규칙

로컬 Codex에게 다음 내용을 프로젝트 규칙으로 전달한다.

```md
# Local Codex Rules

이 저장소는 C 언어 자료구조 학습과 사이드 프로젝트 개발을 함께 관리한다.

## 절대 지켜야 할 규칙

1. `docs/learning/`은 학습 정리용이다.
2. `docs/project/`는 프로젝트 설계/검증 문서용이다.
3. 학습 문서와 프로젝트 문서를 섞지 않는다.
4. C 코드는 `src/`와 `include/`에 작성한다.
5. `.h`에는 구조체 정의와 함수 선언을 둔다.
6. `.c`에는 실제 구현을 둔다.
7. 빌드 산출물은 커밋하지 않는다.
8. `build/`, `.exe`, `.o`는 Git 추적 대상이 아니다.
9. 자료구조는 가능한 직접 구현한다.
10. 기능을 구현하면 관련 문서도 함께 갱신한다.
11. 포인터, 동적 할당, 메모리 해제 부분은 주석 또는 문서로 설명한다.
12. 주요 기능은 최소한 정상/예외/경계값 테스트를 만든다.
13. README는 항상 현재 실행 방법과 프로젝트 상태를 반영해야 한다.
14. 새로운 기능을 추가할 때는 WBS와 설계 문서의 범위를 우선 확인한다.
15. 임의로 프로젝트 구조를 크게 변경하지 않는다.
```

---

## 15. 개발 우선순위

시간이 부족하면 다음 순서대로 진행한다.

```text
1. docs 구조 정리
2. README 작성
3. main.c 실행 확인
4. CMakeLists.txt 작성
5. Task 구조체 구현
6. TaskList 구현
7. Stack 구현
8. Queue 구현
9. Graph 구현
10. Cycle Detection 구현
11. Topological Sort 구현
12. Priority Queue 구현
13. CSV 저장/불러오기
14. 테스트 정리
15. README/회고 정리
```

---

## 16. 오늘 해야 할 일

현재 단계에서는 아래 작업까지만 완료하면 충분하다.

```text
1. README.md 생성
2. .gitignore 생성
3. .gitattributes 생성
4. CMakeLists.txt 생성
5. src/main.c 생성
6. gcc로 main.c 실행 확인
7. CMake로 build 실행 확인
8. SourceTree에서 첫 커밋
9. GitHub push
```

첫 커밋 메시지:

```text
docs: add initial roadmap and project structure
```

두 번째 커밋 메시지:

```text
chore: initialize c project skeleton
```

---

## 17. 면접/발표 대비 관점

최종적으로 다음 질문에 답할 수 있어야 한다.

- 왜 C로 구현했는가?
- 왜 자료구조를 직접 구현했는가?
- 배열과 연결리스트의 차이는 무엇인가?
- 스택과 큐는 프로젝트 어디에 쓰였는가?
- 작업 의존성을 왜 그래프로 표현했는가?
- DFS로 사이클을 어떻게 감지했는가?
- 위상 정렬은 어떤 상황에서 실패하는가?
- 우선순위 큐를 왜 사용했는가?
- 정렬과 우선순위 큐의 차이는 무엇인가?
- CSV 저장 방식의 장단점은 무엇인가?
- 이 프로젝트에서 가장 어려웠던 포인터/메모리 문제는 무엇인가?
- 시간복잡도를 어떻게 분석했는가?
- 이 프로젝트를 웹/Spring Boot로 확장한다면 어떻게 바꿀 것인가?

---

## 18. 참고 기준

- GitHub Pages 포트폴리오 또는 레포 목록 페이지에 노출될 것을 고려하여 README 첫 문단, Repository Description, 문서 링크를 명확하게 작성한다.
- 코드보다 문서가 앞서거나, 문서보다 코드가 앞설 수 있다. 다만 최종적으로는 코드와 문서가 서로 맞아야 한다.
- 블로그 공개는 필수가 아니다. `docs/blog-drafts/`는 내가 이해한 내용을 검증하기 위한 개인 초안 공간으로 사용한다.
