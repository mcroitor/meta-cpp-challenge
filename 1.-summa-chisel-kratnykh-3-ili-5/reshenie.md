# Решение

## Идея решения

Простейшее решение состоит из перебора всех чисел от 1 до N, c проверкой, делится ли каждое число без остатка на 3 или 5. Этот перебор может быть записан рекуррентным соотношением:

$$
\sum\limits_1^N a_i = \sum\limits_1^{N-1} a_i + \begin{cases}N & N \equiv 0 \pmod{3} \lor N \equiv 0 \pmod{5} \\
0 & otherwise\end{cases}
$$

Таким образом, можно дело свести к рекурсии, которая хорошо выражается метапрограммированием. Осталось только научиться определять, какое значение надо прибавлять.

## Подготовка

Для решения нам понадобиться реализовать оператор ветвления при помощи шаблонов - эдакий аналог тернарной операции. Объявление конструкции:

```cpp
template<
  bool CONDITION,
  size_t THEN_VALUE,
  size_t ELSE_VALUE>
struct IF { 
  enum { RESULT = 0 };
}
```

Чтобы пользоваться этой конструкцией, необходимо создать специализации шаблона для первого параметра:

```cpp
template<size_t THEN_VALUE, size_t ELSE_VALUE>
struct IF<true, THEN_VALUE, ELSE_VALUE>{
    enum { RESULT = THEN_VALUE };
};

template<size_t THEN_VALUE, size_t ELSE_VALUE>
struct IF<false, THEN_VALUE, ELSE_VALUE>{
    enum { RESULT = ELSE_VALUE };
};
```

Получается удобный механизм, позволяющий делать выбор на этапе компиляции.

## Решение 1

{% code-tabs %}
{% code-tabs-item title="solution\_01\_meta.cpp" %}
```cpp
#include <iostream>

template<
  bool CONDITION, 
  size_t THEN_VALUE, 
  size_t ELSE_VALUE>
struct IF;

template<size_t N>
struct CALCULUS{
    enum {
        RESULT = IF<
            (N % 3 == 0 || N % 5 == 0) && N % 15 != 0,
            N, 
            0>::RESULT 
          + CALCULUS<N - 1>::RESULT
    };
};

template<>
struct CALCULUS<1>{
    enum { RESULT = 0 };
};

int main(int argc, char** argv) {
    size_t n = 100;
    std::cout << CALCULUS<n>::RESULT;
    return 0;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Решение О\(1\)

На самом деле, эта задача может иметь решение со сложностью О\(1\). 

Давайте вспомним формулу из теории множеств: _объединение двух множеств равно сумме множеств минус пересечение множеств._

$$
A \cup B = A + B - A \cap B
$$

Исходя из этого утверждения, необходимо найти сумму чисел, кратных 3, потом сумму чисел, кратных 5, сумму чисел кратных 15. Каждая сумма чисел будет суммой арифметической прогрессии.

{% code-tabs %}
{% code-tabs-item title="solution\_01\_meta\_O1.cpp" %}
```cpp
#include <iostream>

template<size_t N>
struct CALCULUS {
    enum {
        SUM3 = 3 * (N / 3 + 1) * (N / 3) / 2,
        SUM5 = 5 * (N / 5 + 1) * (N / 5) / 2,
        SUM15 = 15 * (N / 15 + 1) * (N / 15) / 2
    };
    enum {
        RESULT = SUM3 + SUM5 - SUM15
    };
};

int main(int argc, char** argv) {
    size_t n = 1000000000ull;
    std::cout << " calculus = " << CALCULUS<n>::RESULT;
    return 0;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Решение для нетерпеливых

Предыдущее решение можно легко переписать в виде обычных функций и получить изящное решение со сложностью О\(1\).

{% code-tabs %}
{% code-tabs-item title="solution\_01\_final.cpp" %}
```cpp
#include <iostream>

size_t sum(size_t n, size_t d){
    return d * (n / d + 1) * (n / d) / 2;
}

size_t calculus(size_t n){
    return sum(n, 3) + sum(n, 5) - 2 * sum(n, 15);
}

int main(int argc, char** argv) {
    size_t n;
    std::cin >> n;
    std::cout << calculus(n);
    return 0;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

