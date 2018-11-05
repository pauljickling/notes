# References

A reference is an alternative name for your variable. This means any changes made to the reference are also a change to

```
#include <iostream>

using std::cout; using std::endl;

int main() {
  int i = 5;
  int &i2 = i;
  cout << i << endl; // 5
  cout << i2 << endl; // 5
  cout << &i << endl; // 0x7fffcbf6af9c
  cout << &i2 << endl; // 0x7fffcbf6af9c

  i2 = 10;
  cout << i << endl; // 10
  cout << i2 << endl; // 10
  cout << &i << endl; // 0x7fffcbf6af9c
  cout << &i2 << endl; // 0x7fffcbf6af9c
}
```

It is important to note that the ampersand works differently depending on whether it is used in a variable declaration, or if it is used in some other kind of statement.

If an ampersand is used while declaring a variable, it means that this variable name is a reference for the original variable name. That is, it is an alternative name that refers to the same object.

By contrast, when used in other kinds of statements, the ampersand is used to give you the memory address of the object. This is referred to with the eminently memorable name of the *address-of-operator*. Really rolls off the tongue. Anyway, note how for &i and &i2 the address remains unchanged. That's because both variable names refer to the same object in memory. This is in contrast to pointers which create a different object in memory to point to the original variable.
