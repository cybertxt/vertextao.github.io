---

layout: post
title: "C++ Lambda Practice"
date: 2017-03-02 22:31:00 +0800

---

First of all, you should read the *lambda expression*'s [reference](http://en.cppreference.com/w/cpp/language/lambda).

## Lambda is a function

* different format, but the same.

    ```c++
    auto f1 = [](int i)->void {
        printf("%d\n", i);
    };

    void f2(int i) {
        printf("%d\n", i);
    }

    f1(1);
    f2(1);
    ```
    as a caller, *f1* is the same with *f2*

* advantage: convenient

  - fastly define a simple function

  ```c++
  int main() {
	auto foo_min = [](int a, int b) -> int {
		return a < b ? a : b;
	};
	auto m = foo_min(1, 2);
  }
  ```

  - callback

  ```c++
  int do_read(..., int(*on_read)(const char* buf, int len))
  {
      ...
  }

  do_read(..., [](const char* buf, int len)->int{
      // handle buf ...
  });
  ```

## Lambda is more than a function

* **lambda is a closure.** what is a closure?

    > a closure is a record storing a **function** together with an **environment**:

    reference from [wikipedia](https://en.wikipedia.org/wiki/Closure_(computer_programming))

* example for closure

    ```c++
    auto foo(int i) {
        return [i](int n)->void {
            printf("%d\n", i + n);
        };
    }

    auto f1 = foo(1);
    auto f2 = foo(2);

    f1(1);
    f2(1);
    ```

* how about lambda meets c-style function type?

  - capture nothing

    ```c++
    typedef void (*func_t)();
    auto f = []()->void {
        printf("hello\n");
    };
    ```
    *f* CAN be converted to *func_t*

  - capture something

    ```c++
    typedef void (*func_t)();
    int i = 5;
    auto f = [i]()->void {
        printf("%d\n", i);
    };
    ```

    *f* CAN NOT be converted to *func_t*, because this lambda involves a closure that *func_t* CAN NOT express.

* what's the type?

    speaking in code ...

    ```c++
    typedef std::function<void(int)> the_type;
    std::vector<the_type> funcs;

    for(int i = 0; i < 5; ++i) {
        funcs.push_back([i](int n)->void{
            printf("%d\n", i + n);
        });
    }

    for (auto f : funcs) {
        f(10);
    }
    ```

    output

    ```
    10
    11
    12
    13
    14
    ```
    