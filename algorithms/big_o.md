## Big O Notation

Big O notation is a way to express the efficiency of an algorithm by comparing the number of steps an algorithm performs to arrive at its resolution. The value of Big O notation is the expression captures the growth of an algorithm as its input size increases.

Although Big O notation can be used to express various scenarios, it is most frequently only interested in the worst case scenario for an algorithm's performance.

Big O notation also tends to simplify certain parameters. For example, an algorithm might have some initial setup that takes several steps to perform before the algorithm runs the loops that consist of its primary operation. Big O notation would typically ignore these initial steps since they have no impact on the algorithm's rate of growth.

### Syntax

The syntax for Big O notation consists of the letter O, and an expression where n is the input that causes the number of steps to increase as it grows.

### Frequent Big O Expressions

From slowest rate of growth to most rapid:

* O(1) - constant, no growth regardless of input
* O(log n) - log growth
* O(n) - linear growth
* O(n * log n) - polylogarithmic growth
* O(n<sup>c</sup>) - polynomial growth
* O(c<sup>n</sup>) - exponential growth
* O(n!) - factorial growth

In general, if the growth is greater than polylogarithmic growth you have a problem, and should reconsider your approach to solving the problem. Of course, some problems are intrinsically hard and will result in polynomial growth or worse. In those instances it is best to try and find approximate solutions.
