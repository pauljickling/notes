# Pointers

Pointers create a new object and point to the address of other objects. A pointer variable is declared using the * symbol, and it needs to be assigned to an address in memory.

```
#include <iostream>

using std::cout; using std::endl;

int main() {
  int i = 5;
  int *p = &i;
  cout << p << endl; // 0x7ffded57385c
  cout << i << endl; // 5
  cout << &i << endl; // 0x7ffded57385c
  cout << &p << endl; // 0x7ffded573860
  cout << *p << endl; // 5
}
```

Lets go through the `cout` statements one by one to see what is going on.

1. The output of our pointer variable is the memory address for variable i.

2. The output is the assigned value of variable i.

3. The output of the address-of-operator i is the memory address for i. Note how it is the same value as the the output in our first `cout` statement.

4. By contrast, the address-of-operator for our pointer variable contains a different address in memory. This is in contrast to how references work since references are alternative names, whereas pointers are separate objects.

5. Finally note our final `cout` statement. Just like the ampersand has a different meaning depending on whether it is used to declare variables, or used in some other kind of statement, so too with the * symbol. In variable statements, it is used to declare pointers, but in other statements it is used to *dereference* the address, or to put it in plain English, find the declared value of the object at that memory address. So our final `cout` statement gives us an out put of 5 since p points to i, and i has been set to a value of 5.

One final note, you cannot point to a reference. That's just not how these things work, and the compiler will complain if you try.
