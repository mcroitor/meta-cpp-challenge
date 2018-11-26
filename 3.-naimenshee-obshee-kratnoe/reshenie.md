# Решение

Решение сводится к задаче 2 и к известной формуле:

$$
lcm(a, b) = \frac{a \times b} { gcd(a, b)}
$$

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

template<size_t LEFT, size_t RIGHT>
struct LCM {
    enum {
        RESULT = LEFT * RIGHT / GCD<LEFT, RIGHT>::RESULT
    };
}

int main(int argc, char** argv) {
    std::cout << GCD<27, 36>::RESULT;
    return 0;
}
```



