# Решение

Решение основывается на формуле Евклида:

$$
gcd(a, b) = gcd(b, a \bmod b) \\
gcd(a, 0) = a
$$

Второе утверждение является условием остановки рекуррентного алгоритма. И собственно, реализация:

{% code-tabs %}
{% code-tabs-item title="solution\_02\_meta.cpp" %}
```cpp
#include <iostream>

template<size_t LEFT, size_t RIGHT>
struct GCD{
    enum {
        RESULT = GCD<RIGHT, LEFT % RIGHT>::RESULT
    };
};

template<size_t LEFT>
struct GCD<LEFT, 0> {
    enum {
        RESULT = LEFT
    };
};

int main(int argc, char** argv) {
    std::cout << GCD<27, 36>::RESULT;
    return 0;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

