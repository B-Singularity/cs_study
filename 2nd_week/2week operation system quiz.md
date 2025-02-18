다음은 파이썬으로 작성된 재귀 함수의 코드이다.

~~~python
def factorial(n):
    if n == 0:
        return 1
    return n * factorial(n - 1)

result = factorial(5)
print(result)
~~~
이 코드를 실행하면 factorial(5)가 호출되고, 이를 해결하기 위해 factorial(4), factorial(3), ..., factorial(0)까지 재귀적으로 호출되었다가 반환된다.
PVM(Python Virtual Machine)은 이 과정을 실행하기 위해 바이트코드를 생성하고, 내부적으로 스택을 활용한다. 이를 간단한 어셈블리어(Pseudo x86-64)로 변환하면 아래와 같은 코드로 나타낼 수 있다.

~~~
factorial:
    cmp rdi, 0          ; if n == 0
    je base_case        ; return 1 if true

    push rdi            ; push n to stack
    dec rdi             ; n - 1
    call factorial      ; recursive call: factorial(n-1)
    
    pop rax             ; restore original n
    imul rax, rdi       ; multiply result with n
    ret                 ; return result

base_case:
    mov rax, 1          ; return 1
    ret
~~~

위 어셈블리 코드는 x86-64 기반의 의사 코드(Pseudo x86-64 Assembly) 로, 파이썬의 재귀 호출이 실제로 어떻게 스택을 활용하며 동작하는지를 나타낸다.

다음 중 PVM이 재귀 함수를 처리하는 과정에 대한 설명으로 틀린 것은?

① factorial(5)를 호출하면 PVM은 새로운 스택 프레임을 생성하여 함수의 매개변수 및 반환 주소를 저장한다.

② 각 재귀 호출 시, 새로운 스택 프레임이 생성되며 factorial(0)까지 도달한 후 반환되면서 프레임이 차례로 해제된다.

③ PVM은 바이트코드를 실행하는 동안 스택 기반 가상 머신(Stack-based VM) 방식으로 동작하며, 스택을 사용하여 연산을 수행한다.

④ PVM은 실행 중 불필요한 재귀 호출을 감지하여 자동으로 tail-call optimization (TCO)을 적용하여 최적화한다.

⑤ 바이트코드는 CPU가 직접 실행할 수 없으며, PVM이 이를 해석하여 실행하는 방식으로 동작한다.



정답: ④

설명:
④번 문항이 틀린 이유는, 파이썬의 PVM은 Tail Call Optimization(TCO)을 지원하지 않기 때문이다.
재귀 호출이 계속되면 스택 프레임이 증가하며 결국 "RecursionError" 가 발생할 수 있다.
TCO는 특정 언어(C, Scheme 등)에서 재귀를 반복문으로 변환하여 최적화하는 기법이지만,
파이썬에서는 이를 적용하지 않고 재귀 호출마다 새로운 스택 프레임을 계속 생성한다.