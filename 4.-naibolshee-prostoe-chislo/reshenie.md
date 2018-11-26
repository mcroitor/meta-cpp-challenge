# Решение

Решение при помощи шаблонов:

{% code-tabs %}
{% code-tabs-item title="solution\_04\_meta.cpp" %}
```cpp
#include <iostream>

template<bool CONDITION, size_t THEN_VALUE, size_t ELSE_VALUE>
struct IF;

template<size_t THEN_VALUE, size_t ELSE_VALUE>
struct IF<true, THEN_VALUE, ELSE_VALUE>{
    enum { RESULT = THEN_VALUE };
};

template<size_t THEN_VALUE, size_t ELSE_VALUE>
struct IF<false, THEN_VALUE, ELSE_VALUE>{
    enum { RESULT = ELSE_VALUE };
};

template<size_t LEFT, size_t RIGHT>
struct IS_NOT_DIVISIBLE {
    enum { RESULT = IF<LEFT % RIGHT == 0, 0, 1>::RESULT };
};

template<size_t LEFT, size_t RIGHT>
struct CHECK {
    enum { 
        RESULT = IS_NOT_DIVISIBLE<LEFT, RIGHT>::RESULT 
            * CHECK<LEFT, RIGHT - 1>::RESULT 
    };
};

template<size_t LEFT>
struct CHECK<LEFT, 2> {
    enum { RESULT = IS_NOT_DIVISIBLE<LEFT, 2>::RESULT };
};

template<size_t LEFT>
struct CHECK<LEFT, 1> {
    enum { RESULT = 1 };
};

template<size_t N>
struct IS_PRIME {
    enum { RESULT = CHECK<N, N / 2>::RESULT };
};

template<size_t N>
struct FIND_PRIME {
    enum { 
        RESULT = IF<IS_PRIME<N>::RESULT == 1, N, FIND_PRIME<N - 1>::RESULT>::RESULT 
    };
};

template<>
struct FIND_PRIME<2> {
    enum { RESULT = 2 };
};
        
int main(int argc, char** argv) {
    std::cout << "FIND_PRIME<52> = " << FIND_PRIME<52>::RESULT << "\r\n";
    std::cout << "FIND_PRIME<900> = " << FIND_PRIME<900>::RESULT << "\r\n";
    return 0;
}


```
{% endcode-tabs-item %}
{% endcode-tabs %}

Однако, современный подход позволяет использовать **constexpr**:

{% code-tabs %}
{% code-tabs-item title="solution04\_constexpr.cpp" %}
```cpp
#include <iostream>
#include <string>

constexpr bool is_prime(const int number){
    int divisor = number / 2;
    bool _is_prime = true;
    while(divisor > 1 && _is_prime == true){
        if(number % divisor == 0){
            _is_prime = false;
        }
        -- divisor;
    }
    return _is_prime;
}

constexpr int find_prime(int top){
    while(is_prime(top) == false){
        -- top;
    }
    return top;
}

int main()
{
    std::cout << "FIND_PRIME<52> = " << find_prime(52) << "\r\n";    
    std::cout << "FIND_PRIME<900> = " << find_prime(900) << "\r\n";
    std::cout << "FIND_PRIME<1000> = " << find_prime(1000) << "\r\n";
    return 0;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

