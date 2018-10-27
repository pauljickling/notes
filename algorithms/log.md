## Logarithm Notation

In mathematics logarithms answer the question: what exponent is needed to solve for the parameter?

log<sub>2</sub>(8) = 3

When written without a base number, 10 is assumed.

log(100) = log<sub>10</sub>(100) = 2

However in Big O notation it is typical to not write the base of the logarithm, but a base of 10 is not assumed. This is because in complexity analysis one is typically not interested in what the base is, but simply that the asymptotic growth is logarithmic. From this point of view it does not especially matter if an algorithm's growth is log base 6 or log base 20.
