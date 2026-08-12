# C 포인터·구조체 포인터 정리 — Java 학습자 기준

## 0. 이 문서의 목적

이 문서는 Java를 먼저 배운 사람이 C의 포인터, 구조체 포인터, `.c`/`.h` 분리, 컴파일·링크, 헤더 가드를 이해하기 위한 정리 문서입니다.

핵심 기준은 다음입니다.

> Java의 참조형 변수는 “객체를 가리키는 값”처럼 동작한다.  
> C의 포인터는 “메모리 주소를 직접 저장하는 변수”다.

비슷한 감각은 있지만, C는 주소를 직접 얻고(`&`), 주소를 따라가서 값을 읽거나 수정(`*`)할 수 있다는 점이 다릅니다.

---

# 1. 일반 변수와 포인터

## 1.1 기본 예시

```c
int value = 10;
int *ptr = &value;
```

각 표현의 의미는 다음과 같습니다.

| 표현 | 의미 |
|---|---|
| `value` | `value` 변수에 저장된 값, 즉 `10` |
| `&value` | `value` 변수의 메모리 주소 |
| `ptr` | 주소를 저장하는 포인터 변수. 현재는 `value`의 주소를 저장함 |
| `*ptr` | `ptr`에 저장된 주소로 찾아갔을 때 그 위치에 있는 실제 값. 현재는 `10` |
| `&ptr` | 포인터 변수 `ptr` 자체의 주소 |

주의할 점은 `*ptr`과 `&ptr`이 완전히 다르다는 것입니다.

```c
int value = 10;
int *ptr = &value;
```

이 상황을 그림으로 보면 다음과 같습니다.

```text
value 변수
주소: 0x100
값:   10

ptr 변수
주소: 0x200
값:   0x100
```

따라서:

```c
value   // 10
&value  // 0x100
ptr     // 0x100
*ptr    // 10
&ptr    // 0x200
```

`*ptr`은 “포인터 변수 `ptr`이 저장한 주소로 이동했을 때, 그 위치에 있는 값”입니다.

---

## 1.2 `*ptr = 200;`의 의미

```c
int value = 10;
int *ptr = &value;

*ptr = 200;
```

이 코드는 `ptr` 자체를 바꾸는 것이 아닙니다.

```text
ptr에 저장된 주소로 찾아가서,
그 위치의 값을 200으로 바꿔라.
```

결과적으로 `value`가 바뀝니다.

```c
printf("%d\n", value); // 200
```

---

# 2. 함수에서 값 전달과 주소 전달

C 함수의 매개변수는 기본적으로 값이 복사되어 전달됩니다.

## 2.1 값으로 받는 경우

```c
int change(int a) {
    a = 200;
    return a;
}

int main(void) {
    int a = 10;

    printf("%d\n", change(a)); // 200
    printf("%d\n", a);         // 10

    return 0;
}
```

`change(a)`의 반환값은 `200`입니다.  
하지만 원래 변수 `a` 자체가 바뀐 것은 아닙니다.

원래 변수까지 바꾸려면 반환값을 다시 대입해야 합니다.

```c
a = change(a);
```

---

## 2.2 주소로 받는 경우

```c
void change(int *p) {
    *p = 200;
}

int main(void) {
    int a = 10;

    change(&a);

    printf("%d\n", a); // 200

    return 0;
}
```

흐름은 다음과 같습니다.

```text
1. int a = 10;
2. &a로 a의 주소를 얻는다.
3. change(&a)로 주소를 넘긴다.
4. 함수의 int *p가 그 주소를 받는다.
5. *p = 200으로 그 주소에 있는 값을 바꾼다.
6. 결과적으로 원래 a가 200이 된다.
```

---

# 3. 초기화되지 않은 포인터가 위험한 이유

```c
int *p;
*p = 10;
```

이 코드는 위험합니다.

`int *p;`라고만 선언하면 `p`가 아직 유효한 주소를 가지고 있다는 보장이 없습니다.  
즉, `p` 안에는 알 수 없는 쓰레기 주소가 들어 있을 수 있습니다.

그 상태에서:

```c
*p = 10;
```

을 실행하면 다음과 같은 의미가 됩니다.

```text
알 수 없는 주소로 찾아가서 거기에 10을 써라.
```

이 경우 다음 문제가 생길 수 있습니다.

- 프로그램이 바로 종료될 수 있음
- 엉뚱한 메모리를 덮어쓸 수 있음
- 운 좋게 되는 것처럼 보이다가 나중에 이상한 버그가 생길 수 있음
- 디버깅이 매우 어려워질 수 있음

따라서 포인터는 반드시 유효한 주소를 넣은 뒤 사용해야 합니다.

```c
int value = 0;
int *p = &value;

*p = 10;
```

또는 동적 메모리를 할당해야 합니다.

```c
#include <stdlib.h>

int *p = malloc(sizeof(int));

if (p != NULL) {
    *p = 10;
    free(p);
}
```

C에는 Java의 JVM/GC처럼 메모리를 자동으로 안전하게 관리해주는 구조가 없습니다.  
하지만 이 문제의 핵심은 단순한 메모리 낭비가 아니라, “유효하지 않은 주소를 따라가서 값을 쓰는 것”입니다.

---

# 4. 구조체와 구조체 포인터

## 4.1 Java의 클래스와 C의 구조체 비교

Java:

```java
class Car {
    int price;
    int engine;
    int speed;

    Car(int price, int engine, int speed) {
        this.price = price;
        this.engine = engine;
        this.speed = speed;
    }
}

Car car1 = new Car(3, 5, 6);
```

C:

```c
typedef struct {
    int price;
    int engine;
    int speed;
} Car;

Car car1 = {3, 5, 6};
Car *car1_ptr = &car1;
```

C의 구조체는 Java의 클래스처럼 메서드, 생성자, 상속, 접근제어자를 기본으로 가지는 객체 시스템이 아닙니다.  
기본적으로는 “여러 데이터를 하나로 묶은 자료형”입니다.

---

## 4.2 `Car car1`과 `Car *car1_ptr`

```c
Car car1 = {3, 5, 6};
Car *car1_ptr = &car1;
```

| 표현 | 의미 |
|---|---|
| `car1` | `Car` 구조체 변수 자체 |
| `&car1` | `car1` 구조체 변수의 주소 |
| `car1_ptr` | `Car` 구조체의 주소를 저장하는 포인터 변수 |
| `*car1_ptr` | `car1_ptr`이 가리키는 구조체 변수 자체 |

---

## 4.3 `.`과 `->`

구조체 변수 자체로 필드에 접근할 때는 `.`을 사용합니다.

```c
car1.price = 100;
```

구조체 포인터로 필드에 접근할 때는 `->`를 사용합니다.

```c
car1_ptr->price = 100;
```

`->`는 다음 표현의 축약형입니다.

```c
(*car1_ptr).price = 100;
```

정리하면 다음과 같습니다.

| 상황 | 문법 |
|---|---|
| 구조체 변수 자체 | `car1.price` |
| 구조체 포인터 | `car1_ptr->price` |
| 구조체 포인터를 풀어쓴 표현 | `(*car1_ptr).price` |

---

## 4.4 구조체 변수는 포인터 없이도 수정 가능하다

구조체 변수 자체를 가지고 있으면 포인터 없이도 필드 수정이 가능합니다.

```c
Car car1 = {3, 5, 6};

car1.price = 100;
car1.speed = 200;
```

포인터가 필요한 대표적인 상황은 “함수 안에서 원본 구조체를 수정하고 싶을 때”입니다.

---

# 5. 구조체를 함수에 넘길 때

## 5.1 구조체를 값으로 받는 경우

```c
typedef struct {
    int priority;
} Task;

void update_task(Task task) {
    task.priority = 10;
}

int main(void) {
    Task task = {3};

    update_task(task);

    printf("%d\n", task.priority); // 3

    return 0;
}
```

`Task task`로 매개변수를 받으면 구조체 전체가 복사되어 함수에 들어갑니다.  
함수 안에서 수정해도 원본은 바뀌지 않습니다.

값으로 받는 경우가 적절한 상황:

- 함수 안에서 원본을 수정할 필요가 없을 때
- 단순히 조회하거나 출력할 때
- 구조체 크기가 작고 복사 비용이 크지 않을 때
- 일부러 복사본을 가지고 작업하고 싶을 때

---

## 5.2 구조체 포인터로 받는 경우

```c
typedef struct {
    int priority;
} Task;

void update_task(Task *task) {
    task->priority = 10;
}

int main(void) {
    Task task = {3};

    update_task(&task);

    printf("%d\n", task.priority); // 10

    return 0;
}
```

`Task *task`로 받으면 원본 구조체의 주소를 받습니다.  
따라서 함수 안에서 원본을 직접 수정할 수 있습니다.

포인터로 받는 경우가 적절한 상황:

- 함수 안에서 원본 구조체를 수정해야 할 때
- 구조체가 커서 복사 비용을 줄이고 싶을 때
- 함수 결과를 구조체에 반영해야 할 때
- 동적 메모리로 만든 구조체를 다룰 때

읽기만 할 때는 다음처럼 `const`를 붙이는 것이 좋습니다.

```c
void print_task(const Task *task) {
    printf("%d\n", task->priority);
}
```

`const Task *task`는 “이 함수 안에서는 `task`가 가리키는 구조체를 수정하지 않겠다”는 뜻입니다.

---

# 6. `task.priority`와 `task_ptr->priority`

```c
typedef struct {
    int priority;
} Task;

Task task = {3};
Task *task_ptr = &task;
```

여기서:

```c
task.priority
```

은 구조체 변수 `task`의 `priority` 필드에 직접 접근합니다.

```c
task_ptr->priority
```

은 `task_ptr`이 가리키는 구조체, 즉 `task`의 `priority` 필드에 접근합니다.

`task_ptr`에는 `&task`가 저장되어 있으므로, 둘 다 같은 구조체의 같은 필드를 가리킵니다.

```c
task_ptr->priority
```

는 다음과 같습니다.

```c
(*task_ptr).priority
```

즉:

```text
task.priority
= task의 priority 필드

Task *task_ptr = &task;
task_ptr->priority
= task_ptr이 가리키는 task의 priority 필드
```

따라서 둘은 같은 값을 읽고, 같은 위치를 수정합니다.

---

# 7. C의 생성자, getter/setter, 기본값

## 7.1 C에는 Java식 `new`가 없다

C에는 Java의 `new` 키워드가 없습니다.

일반 구조체 변수는 다음처럼 만듭니다.

```c
Car car1 = {3, 5, 6};
```

동적 메모리 할당을 사용하면 Java의 `new`와 조금 비슷한 형태가 됩니다.

```c
#include <stdlib.h>

Car *car1 = malloc(sizeof(Car));

if (car1 != NULL) {
    car1->price = 3;
    car1->engine = 5;
    car1->speed = 6;

    free(car1);
}
```

Java는 GC가 메모리를 정리하지만, C는 `malloc`으로 할당한 메모리를 `free`로 직접 해제해야 합니다.

---

## 7.2 C에는 Java식 생성자가 없다

C에는 클래스 생성자가 없습니다.  
대신 구조체 초기화 문법이나 초기화 함수를 사용합니다.

순서 기반 초기화:

```c
Car car1 = {3, 5, 6};
```

필드 이름 기반 초기화:

```c
Car car1 = {
    .price = 3,
    .engine = 5,
    .speed = 6
};
```

생성자처럼 함수를 만들 수도 있습니다.

```c
Car create_car(int price, int engine, int speed) {
    Car car = {
        .price = price,
        .engine = engine,
        .speed = speed
    };

    return car;
}
```

사용:

```c
Car car1 = create_car(3, 5, 6);
```

---

## 7.3 C에는 Java식 getter/setter 문법이 없다

C에서는 보통 필드에 직접 접근합니다.

```c
car1.price = 100;
printf("%d\n", car1.price);
```

하지만 getter/setter 역할을 하는 함수를 만들 수는 있습니다.

```c
int get_car_price(const Car *car) {
    return car->price;
}

int set_car_price(Car *car, int price) {
    if (price < 0) {
        return 0;
    }

    car->price = price;
    return 1;
}
```

이렇게 하면 값 검증을 넣을 수 있습니다.

---

## 7.4 초기화하지 않으면 기본값이 들어가는가?

Java와 달리 C의 지역 변수는 자동으로 0으로 초기화되지 않습니다.

```c
void test(void) {
    Car car1;

    printf("%d\n", car1.price); // 쓰레기값 가능
}
```

초기화 상태는 다음과 같습니다.

| 선언 방식 | 초기값 |
|---|---|
| 지역 변수 `Car car;` | 초기화되지 않음, 쓰레기값 가능 |
| 지역 변수 `Car car = {0};` | 모든 필드가 0 계열 값으로 초기화 |
| 전역 변수 `Car car;` | 0으로 초기화 |
| `static Car car;` | 0으로 초기화 |

초보자 기준으로는 다음 습관이 안전합니다.

```c
Car car1 = {0};
```

또는 필드명을 직접 적어 초기화합니다.

```c
Car car1 = {
    .price = 3,
    .engine = 5,
    .speed = 6
};
```

---

# 8. 타입 오류와 예외 처리

## 8.1 `int`에 `char`는 들어갈 수 있다

```c
int x = 'A';

printf("%d\n", x); // 보통 65
```

문자 하나는 내부적으로 숫자 코드값으로 처리될 수 있습니다.

## 8.2 `int`에 문자열은 넣으면 안 된다

```c
int x = "hello"; // 잘못된 코드
```

문자열은 보통 다음처럼 다룹니다.

```c
char name[100] = "hello";
```

또는:

```c
const char *name = "hello";
```

문자열 리터럴을 수정하지 않을 때는 `const char *`로 받는 것이 안전합니다.

---

## 8.3 C에는 Java식 try-catch가 없다

C에는 Java의 `try-catch`가 없습니다.

대신 반환값으로 성공/실패를 표현하는 경우가 많습니다.

```c
int divide(int a, int b, int *result) {
    if (b == 0) {
        return 0; // 실패
    }

    *result = a / b;
    return 1; // 성공
}
```

사용:

```c
int result;

if (divide(10, 2, &result)) {
    printf("%d\n", result);
} else {
    printf("나누기 실패\n");
}
```

---

# 9. C 구조체와 상속·인터페이스

C의 `struct`에는 Java의 상속, 인터페이스 구현 문법이 없습니다.

Java:

```java
class Car extends Vehicle implements Drivable {
    ...
}
```

C에는 이런 문법이 없습니다.

다만 흉내는 낼 수 있습니다.

## 9.1 상속 비슷하게 구조체 포함하기

```c
typedef struct {
    int weight;
    int max_speed;
} Vehicle;

typedef struct {
    Vehicle base;
    int engine;
    int price;
} Car;
```

사용:

```c
Car car = {0};

car.base.weight = 1000;
car.base.max_speed = 200;
car.engine = 5;
car.price = 3000;
```

이것은 Java의 상속이라기보다는 “구조체 안에 다른 구조체를 포함하는 방식”입니다.  
즉 합성, composition에 가깝습니다.

## 9.2 인터페이스 비슷하게 함수 포인터 사용하기

C에서는 함수 포인터로 인터페이스와 비슷한 구조를 흉내 낼 수 있습니다.

```c
typedef struct {
    void (*start)(void);
    void (*stop)(void);
} Drivable;
```

다만 자료구조 학습 초반에는 함수 포인터까지 깊게 들어가지 않아도 됩니다.

---

# 10. `.h`와 `.c` 분리

## 10.1 파일 역할

| 파일 | 역할 |
|---|---|
| `.h` | 다른 파일에게 보여줄 선언, 약속, 사용법 |
| `.c` | 실제 함수 구현 코드 |
| `main.c` | 프로그램 시작점이 들어 있는 소스 파일 |

`.h`는 Java의 class와 완전히 같은 것은 아닙니다.  
더 정확히는 “공개 API 목록” 또는 “사용 설명서”에 가깝습니다.

---

## 10.2 예시 구조

```text
project/
├── main.c
├── task.c
└── task.h
```

### task.h

```c
#ifndef TASK_H
#define TASK_H

typedef struct {
    int id;
    char title[100];
    int done;
} Task;

void complete_task(Task *task);
void print_task(const Task *task);

#endif
```

### task.c

```c
#include <stdio.h>
#include "task.h"

void complete_task(Task *task) {
    task->done = 1;
}

void print_task(const Task *task) {
    printf("id: %d\n", task->id);
    printf("title: %s\n", task->title);
    printf("done: %d\n", task->done);
}
```

### main.c

```c
#include "task.h"

int main(void) {
    Task task = {
        .id = 1,
        .title = "C 포인터 공부",
        .done = 0
    };

    print_task(&task);
    complete_task(&task);
    print_task(&task);

    return 0;
}
```

---

## 10.3 `#include`의 의미

```c
#include "task.h"
```

이 코드는 `task.h`의 내용을 현재 파일에 포함시킵니다.

하지만 `#include "task.h"`가 `task.c`의 실제 구현까지 자동으로 연결해주는 것은 아닙니다.

즉 `main.c`에서 `task.h`를 include하면 컴파일러는 다음을 알 수 있습니다.

```text
Task라는 타입이 있구나.
complete_task라는 함수가 있구나.
print_task라는 함수가 있구나.
```

하지만 실제 구현은 `task.c`에 있으므로 컴파일할 때 `task.c`도 같이 넣어야 합니다.

```bash
gcc main.c task.c -o app
```

만약 `main.c`만 컴파일하면 링크 단계에서 다음과 비슷한 오류가 날 수 있습니다.

```text
undefined reference to `complete_task'
```

---

# 11. 헤더 가드

헤더 파일에는 보통 다음 형태가 들어갑니다.

```c
#ifndef TASK_H
#define TASK_H

// 헤더 내용

#endif
```

이것을 헤더 가드라고 합니다.

---

## 11.1 `#ifndef TASK_H`

```c
#ifndef TASK_H
```

`ifndef`는 `if not defined`의 줄임말입니다.

의미:

```text
만약 TASK_H가 아직 정의되어 있지 않다면,
아래 내용을 처리해라.
```

즉 `TASK_H`를 정의 시작한다는 뜻이 아니라, `TASK_H`가 정의되어 있는지 검사하는 문장입니다.

---

## 11.2 `#define TASK_H`

```c
#define TASK_H
```

의미:

```text
이제 TASK_H라는 이름을 정의해라.
```

여기서는 값이 중요한 것이 아닙니다.  
“TASK_H가 정의되었는가?”만 중요합니다.

---

## 11.3 `#endif`

```c
#endif
```

의미:

```text
#ifndef 조건문의 끝
```

C의 일반 `if`문에서 닫는 중괄호 `}` 같은 역할이라고 보면 됩니다.

---

## 11.4 헤더 가드 동작 흐름

처음 `task.h`가 포함될 때:

```text
1. TASK_H가 아직 정의되어 있지 않음
2. #ifndef TASK_H 통과
3. #define TASK_H 실행
4. 구조체와 함수 선언 포함
```

두 번째로 `task.h`가 다시 포함될 때:

```text
1. TASK_H가 이미 정의되어 있음
2. #ifndef TASK_H 조건 실패
3. 내부 내용 건너뜀
```

이렇게 해서 같은 헤더가 여러 번 포함되어 구조체나 함수 선언이 중복되는 문제를 막습니다.

---

# 12. 컴파일과 링크

## 12.1 한 번에 컴파일하기

```bash
gcc main.c task.c -o app
```

| 부분 | 의미 |
|---|---|
| `gcc` | C 컴파일러 실행 |
| `main.c task.c` | 컴파일할 소스 파일들 |
| `-o app` | 결과 실행 파일 이름을 `app`으로 지정 |

---

## 12.2 나눠서 컴파일하기

```bash
gcc -c main.c -o main.o
gcc -c task.c -o task.o
gcc main.o task.o -o app
```

| 옵션 | 의미 |
|---|---|
| `-c` | 컴파일만 하고 링크하지 않음. `.o` 목적 파일 생성 |
| `-o 이름` | 출력 파일 이름 지정 |

---

## 12.3 추천 컴파일 옵션

```bash
gcc -std=c11 -Wall -Wextra -g main.c task.c -o app
```

| 옵션 | 의미 |
|---|---|
| `-std=c11` | C11 표준 기준으로 컴파일 |
| `-Wall` | 일반적인 경고 켜기 |
| `-Wextra` | 추가 경고 켜기 |
| `-g` | 디버깅 정보 포함 |
| `-o app` | 실행 파일 이름을 `app`으로 지정 |

처음 공부할 때는 `-Wall -Wextra`를 켜는 습관이 좋습니다.

---

# 13. 명명 규칙

C에는 Java처럼 강하게 통일된 표준 명명법이 있는 것은 아니지만, 많이 쓰이는 관습은 있습니다.

| 대상 | 추천 스타일 | 예시 |
|---|---|---|
| 변수명 | `snake_case` | `total_count`, `task_id` |
| 함수명 | `snake_case` | `create_task`, `print_task` |
| 구조체 타입명 | `PascalCase` 또는 `snake_case_t` | `Task`, `Car`, `task_t` |
| 매크로 | `UPPER_SNAKE_CASE` | `MAX_SIZE`, `TASK_H` |
| 헤더 가드 | `UPPER_SNAKE_CASE` | `TASK_H`, `PROJECT_TASK_H` |
| 파일명 | `snake_case.c`, `snake_case.h` | `task_manager.c`, `task_manager.h` |

초보자 기준 추천 규칙:

```c
// 구조체 타입
Task
Car
Book

// 변수
int task_count;
int total_price;

// 함수
void print_task(const Task *task);
void complete_task(Task *task);

// 매크로
#define MAX_TITLE_LENGTH 100
```

---

# 14. 문답용 질문 채점

## 질문 1

### 문제

```c
int value = 10;
int *ptr = &value;
```

일 때 `value`, `&value`, `ptr`, `*ptr`은 각각 무엇을 의미하나요?

### 작성 답안 요약

```text
value : int형 변수
&value : 변수 value의 주소
ptr : 포인트 변수
&ptr : 포인트 변수가 가리키는 곳에 있는 값
```

### 채점

부분 정답입니다.

맞은 부분:

- `&value`를 `value`의 주소라고 한 것은 맞습니다.
- `ptr`을 포인터 변수라고 한 것도 방향은 맞습니다.

수정해야 할 부분:

1. `value`는 “int형 변수”라기보다 이 문맥에서는 “변수 value에 저장된 값, 즉 10”이라고 답하는 것이 더 정확합니다.
2. 문제는 `*ptr`을 물었는데, 답안에는 `&ptr`이라고 적었습니다.
3. `&ptr`은 “포인터 변수가 가리키는 곳의 값”이 아닙니다.
4. `&ptr`은 포인터 변수 `ptr` 자체의 주소입니다.
5. “포인트 변수”보다는 “포인터 변수”가 맞습니다.

### 정답

```text
value  : value 변수에 저장된 값, 즉 10
&value : value 변수의 주소
ptr    : value의 주소를 저장하고 있는 포인터 변수
*ptr   : ptr에 저장된 주소로 찾아갔을 때 그 위치에 있는 값, 즉 10
```

추가로:

```text
&ptr   : 포인터 변수 ptr 자체의 주소
```

---

## 질문 2

### 문제

아래 코드에서 `task.priority`와 `task_ptr->priority`가 같은 값을 가리키는 이유는 무엇인가요?

```c
typedef struct {
    int priority;
} Task;

Task task = {3};
Task *task_ptr = &task;
```

### 작성 답안 요약

작성 답안은 핵심적으로 다음 내용을 포함했습니다.

- `Task` 구조체에 `priority` 필드가 있음
- `Task task = {3};`으로 `priority`가 3으로 초기화됨
- `Task *task_ptr = &task;`로 `task`의 주소를 포인터에 저장함
- `task_ptr->priority`는 `(*task_ptr).priority`와 같음
- 구조체를 함수에 값으로 넘기면 복사되고, 포인터로 넘기면 원본 수정이 가능함

### 채점

대체로 잘 이해했습니다.  
다만 표현을 몇 군데 고쳐야 합니다.

맞은 부분:

- `Task` 구조체에 `priority` 필드가 있다는 설명은 맞습니다.
- `Task task = {3};`으로 `priority`가 3으로 초기화된다는 설명은 맞습니다.
- `Task *task_ptr = &task;`가 `task`의 주소를 저장한다는 설명은 맞습니다.
- `task_ptr->priority`가 `(*task_ptr).priority`와 같다는 설명은 정확합니다.
- 함수에 값으로 넘기면 복사되고, 포인터로 넘기면 원본을 수정할 수 있다는 연결도 좋습니다.

수정해야 할 부분:

1. `task.priority`는 “구조체 복사본에 직접 접근”이 아닙니다. 현재 코드에서는 `task`라는 구조체 변수 자체에 직접 접근하는 것입니다.
2. 복사본이라는 표현은 함수에 `Task task` 형태로 넘겼을 때 더 적절합니다.
3. `task_ptr`은 “포인터 구조체”가 아니라 “구조체를 가리키는 포인터 변수”입니다.
4. “구조체는 call by value”라는 표현보다는 “C 함수 인자는 기본적으로 값 복사로 전달된다”가 더 정확합니다.

### 정답

```text
Task task = {3};은 priority가 3인 Task 구조체 변수 task를 만든 것이다.
Task *task_ptr = &task;는 task_ptr이라는 포인터 변수에 task의 주소를 저장한 것이다.

따라서 task.priority는 task 구조체 변수의 priority 필드에 직접 접근하는 표현이고,
task_ptr->priority는 task_ptr이 가리키는 구조체, 즉 task의 priority 필드에 접근하는 표현이다.

task_ptr->priority는 (*task_ptr).priority와 같은 의미다.
결국 둘 다 같은 구조체 task의 같은 필드 priority를 가리키므로 같은 값을 읽고 수정한다.
```

---

## 질문 3

### 문제

함수에서 `Task`를 값으로 받는 것과 `Task *`를 받는 것은 언제 다르게 선택해야 하나요?

### 작성 답안 요약

작성 답안은 다음 방향이었습니다.

- 간단한 수정이나 일회성 조작은 `Task`로 받는다.
- 반복되는 함수나 꼭 실행되어야 하는 패턴은 `Task *`를 사용한다.
- 함수로 구조체 필드를 조작하려면 구조체 포인터로 접근해야 한다.
- 실제 값이 수정되었는지 확인해야 하므로 포인터 구조체를 사용해야 한다.

### 채점

부분 정답입니다.  
“함수에서 원본을 수정하려면 구조체 포인터가 필요하다”는 핵심은 맞습니다.  
하지만 선택 기준을 “반복되는가”, “일회성인가”, “꼭 실행되어야 하는가”로 잡은 부분은 정확하지 않습니다.

수정해야 할 부분:

1. `Task`로 받느냐 `Task *`로 받느냐의 핵심 기준은 반복 여부가 아닙니다.
2. 핵심 기준은 “원본을 수정해야 하는가?”와 “복사 비용을 줄이고 싶은가?”입니다.
3. `Task`로 받으면 함수 안에서 필드를 바꿔도 복사본만 바뀌고 원본은 바뀌지 않습니다.
4. `Task *`로 받으면 원본 주소를 받기 때문에 함수 안에서 원본을 수정할 수 있습니다.
5. 수정하지 않고 읽기만 할 때는 `const Task *`를 많이 사용합니다.

### 정답

```text
Task를 값으로 받는 경우는 함수 안에서 원본을 수정할 필요가 없을 때 사용한다.
예를 들어 단순 출력, 조회, 계산처럼 복사본으로 충분한 경우에 사용할 수 있다.
다만 구조체가 크면 복사 비용이 생길 수 있다.

Task *를 받는 경우는 함수 안에서 원본 구조체를 수정해야 할 때 사용한다.
또는 구조체가 커서 복사 비용을 줄이고 싶을 때도 포인터로 받는다.

읽기만 하고 수정하지 않을 때는 const Task *를 사용하면 좋다.
```

예시:

```c
void print_task(const Task *task) {
    printf("%d\n", task->priority);
}

void update_task(Task *task) {
    task->priority = 10;
}
```

---

# 15. 최종 압축 정리

## 포인터

```c
int value = 10;
int *ptr = &value;
```

```text
value  = 값
&value = value의 주소
ptr    = value의 주소를 저장한 포인터 변수
*ptr   = ptr이 저장한 주소로 찾아갔을 때의 값
&ptr   = ptr 변수 자체의 주소
```

## 구조체

```c
typedef struct {
    int priority;
} Task;

Task task = {3};
Task *task_ptr = &task;
```

```text
task.priority       = 구조체 변수 task의 필드 접근
task_ptr->priority  = task_ptr이 가리키는 구조체의 필드 접근
(*task_ptr).priority = task_ptr->priority와 같은 의미
```

## 함수 선택

```c
void func(Task task)
```

- 구조체가 복사됨
- 원본 수정 안 됨
- 조회, 출력, 계산에 적합

```c
void func(Task *task)
```

- 원본 주소를 받음
- 원본 수정 가능
- 수정 함수에 적합

```c
void func(const Task *task)
```

- 원본 주소를 받지만 수정하지 않겠다는 의미
- 읽기 전용 함수에 적합

## `.h`와 `.c`

```text
.h = 선언, 약속, 외부에 공개할 사용법
.c = 실제 구현
```

`#include "task.h"`는 선언을 가져오는 것이고, 실제 구현까지 연결하려면 컴파일할 때 `.c` 파일도 같이 넣어야 합니다.

```bash
gcc main.c task.c -o app
```
