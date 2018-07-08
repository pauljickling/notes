## Big O Notation

Big O notation is a way to express the efficiency of an algorithm by comparing the number of operations an algorithm performs to arrive at its resolution. The value of Big O notation is the expression captures the growth of an algorithm as its input size increases.

Although Big O notation can be used to express various conditions, it is most frequently only interested in the worst case scenario for an algorithm's performance. However there are occasions when it is useful to consider the average case scenario (it is almost never useful to consider the best case scenario).

Big O notation also tends to simplify certain parameters. For example, an algorithm might have some initial setup that takes several operations to perform before the algorithm runs the loops that consist of its primary operation. Big O notation would typically ignore these initial operations since it doesn't affect the rate of growth of the algorithm.

### Syntax

The syntax for Big O notation consists of the letter O, and then the expression of the number of operations (n).

### Frequent Big O Expressions

From slowest rate of growth to most rapid:

* O(1) - constant, no growth
* O(log n) - log growth
* O(n) - linear growth
* O(n * log n) - polylogarithmic growth
* O(n<sup>2</sup>) - quadratic growth
* O(2<sup>n</sup) - exponential growth
* O(n!) - factorial growth
