#### 모두 link와 관련된 내용
- c++ static memory 관련 내용이 아니라 build 과정에 관여되는 static 에 대한 내용

#### extern이란?
- 바깥족에서 link를 찾아라

#### static이란?
- 안쪽으로만 link를 열어라

#### variable 예제
main.cpp 에서 a = 0;
foo.cpp 에서 a = 100;

- 두 파일을 link 하는 과정에서 충돌이 일어난다.
- 이 때 main.cpp 의 a에 extern 키워드로 정의를 하면 "바깥쪽 어딘가에 있어" 라는 뜻
- a.out에서는 extern으로 가지고 있다 linking 되는 순간 foo.cpp의 a를 사용

- static은 같은 translation unit(object 파일) 안에서만 사용할 수 있는 것(밖에서 못 찾도록)

#### function 예제
- main.cpp 에서 foo() declaration, foo.cpp 에서 foo() definition 을 한다면 잘 동작하는 것을 볼 수있다. (같이 링킹되었다는 가정)
- 함수에 대해서는 default로 extern이 붙여있는 것!
- static 함수는 마찬가지로 같은 translation unit (obj) 바깥으로 링크를 주지 않는 것!
- 따라서 안전한 빌드 프로세스를 진행할 수 있다.

#### function 예제2

```
// cat.h
#pragma once
class cat{
public : 
    void speak();
private : 
    int age = 10;
};

// cat.cpp
#include "cat.h"
#include <iostream>

static void bye(){
    std::cout << "bye" <<std::endl;
}

void Cat::speak(){
    std::cout<<"meow" << std::endl;
    std::cout<<"i'm" <<age<<std::endl;
    bye();
}

```

- 위에서 bye()는 private한 함수 였지만, 어떤 멤버 variable도 접근하지 않기 때문에, free function(private이 아닌) 으로 만들고 static 함수로 만든다면 cat.obj 파일에서만 사용이 가능하며, 이것이 일반적인 방법이다.

#### extern "C" keyword

```
int foo(double a){
    static_cast<int>(a);
}
```

- 이 코드를 g++ foo.cpp -c 로 빌드하면 foo.o 파일이 생긴다.
- nm foo.o 하면 해당 파일의 symbol을 볼 수 있다.
- Z3food 로 보인다. 이것은 name mangling 이라는 c++의 컴파일 프로세스이다.
- 왜? function overloading을 위해서 한다.
- C++가 아닌 c에서는 function overloading 기능이 없다.
- c interface를 가진 binary를 제공하기 위해서 붙인다.

- 이 떄 extern "C" 함수를 붙여주면 name mangling을 수행하지 않는다.
- header의 declaration에서만 extern을 붙여도 동작한다.
- scope 안에다가 extern c를 넣어줄 수도 있다.


