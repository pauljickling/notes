# Scalar Types

C++ is a statically types language, meaning you must declare the type for your variables. To use most non-scalar types such as strings and vectors you must declare them in your require statements, whereas scalar types you get "for free".

## auto

`auto` defaults to whatever type makes the most sense. I generally avoid auto whenever I can because I find it may lead to unexpected results.

## int

`int` handles integers. By default they are signed. It is 16 bits.

## short

`short` is a short 16 bit integer.

## long

`long` handles 32 bit integers.

## long long

`long long` handles 64 bit integers.

## unsigned

`unsigned` handles unsigned integers. If you refer to `unsigned` on its own it defaults to int. So if you wanted to use a 32 bit unsigned integer you would use `unsigned long` and if you wanted to use a 64 bit unsigned integer you would use `unsigned long long`.

## float

`float` handles decimal numbers with "single-precision".

## double

`double` handles floats with double precision. Generally the preferred type to use for decimal numbers according to *C++ Primer*.

## char

`char` is used for single characters. Chars use single quotes, unlike strings which use double quotes. 8 bit.

## wchar_t

`wchar_t` stands for wide character and is 16 bits.

## char16_t

`char16_t` is for Unicode 16 bit chars.

## char32_t

`char32_t` is for Unicode 32 bit chars.

## bool

`bool` is used for Booleans. Uses true and false (lowercase), and evaluating bool types returns 1 or 0.
