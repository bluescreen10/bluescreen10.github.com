---
layout: post
title: Are panics faster?
---

A couple days ago in a reddit [thread](https://www.reddit.com/r/golang/comments/12pq003/panics_are_up_to_40_faster_than_returning_errors/), the author claimed that
using panics instead of returning errors was up to 40% faster. The reactions 
were, as expected, mostly negative, after all two of the [go proverbs](
https://go-proverbs.github.io/) are *"Errors are values"* and *"Don't panic"*.

I'm not here to argue the merits of using panics vs. errors from a syntactic
point of view. I actually believe that if you decided to use panics instead of
returning errors, your code aesthetics wouldn't change much. That's because most
of the standard and third-party libraries are built using multiple returns,
which forces the consumer to check the error and only then panic. Leaving that 
aside, the point of this article is to verify the bold claim that using panics
would give you a 40% performance improvement.

Before doing any measurement, let's think about what we should expect:
- Returning errors require returning another value. Since Go switched to
[register-based](https://go.dev/doc/go1.17#compiler) calling convention in 
version `1.17`, the cost of the extra return value should be negligible.
- Checking an errors involves doing `if err != nil` tests. This is converted to 
`test` and `jmp` assembly instructions by the compiler. Branch predictors on 
modern processors have come a long way, but their cost won't be zero.
- On the other hand `panic` aborts the execution immediately and jumps directly
to `defer` blocks. If error is not handled by these blocks, the execution is
terminated. Code that uses panics doesn't return errors and therefore the caller
doesn't need to check for them (duh!). Typically, a deferred function is set up
at the top of the call stack to catch any panics.

Armed with this intuition, we should expect that using panics to be faster than
code that uses the traditional error handling techniques, at least in cases
where there are no errors. To test this hypothesis, I've built a function called
`Divide` that divides two integers only if the dividend is not zero and if the
numerator if bigger than the denominator. Note: I know the second check is not
strictly necessary, but I wanted the function to be non-trivial.

```go
//go:noinline
func Divide(a, b int) int {
	if b == 0 || math.Abs(float64(b)) > math.Abs(float64(a)) {
		return 0
	}
	return a / b
}
```

This is our baseline: there is no error handling needed and no panics. I wanted
to see the effects on performance when adding error handling, so I built a 
second function that returns an error and called this `Divide2`.

```go
//go:noinline
func Divide2(a, b int) (int, error) {
	if b == 0 || math.Abs(float64(b)) > math.Abs(float64(a)) {
		return 0, ErrDivideByZero
	}
	return a / b, nil
}
```

Finally, I've added a function that would `panic` instead of returning an error
value. Note that I've try to mimic the behavior between this and the previous
function.

```go
//go:noinline
func DividePanic(a, b int) int {
	if b == 0 || math.Abs(float64(b)) > math.Abs(float64(a)) {
		panic(ErrDivideByZero)
	}
	return a / b
}
```

## Tests

Using these functions, I set up micro-benchmarks to test the cost of: adding
error values, checking errors, using panics, and using panics with defer 
a block. Each tests performs a thousands iterations and is set up in a way that 
guarantees no error will occur.

Without further ado, here are the results:

```
goos: darwin
goarch: amd64
cpu: Intel(R) Core(TM) i5-8259U CPU @ 2.30GHz
=== RUN   BenchmarkDivide
BenchmarkDivide
BenchmarkDivide-8                 109248             10878 ns/op               0 B/op          0 allocs/op
=== RUN   BenchmarkDivide2
BenchmarkDivide2
BenchmarkDivide2-8                110515             10728 ns/op               0 B/op          0 allocs/op
=== RUN   BenchmarkDivide2Check
BenchmarkDivide2Check
BenchmarkDivide2Check-8           108925             10947 ns/op               0 B/op          0 allocs/op
=== RUN   BenchmarkPanic
BenchmarkPanic
BenchmarkPanic-8                  110127             10797 ns/op               0 B/op          0 allocs/op
=== RUN   BenchmarkPanicRecover
BenchmarkPanicRecover
BenchmarkPanicRecover-8           110226             10724 ns/op               0 B/op          0 allocs/op
```

*Note: at the end of the article you'd find a link to a gist with the benchmark code[^1]*

## Observations
From the results we can observe the following:
- Each iteration takes around 11ns.
- And as you would expect the `if err != nil` check has a little cost <1%. It's
minimal but not zero. 
- Surprisingly, adding the extra return value makes the code ~1% faster(!?) 
compared to the baseline. I've run this multiple times, and most of the time, it
is faster. I've even looked at the generated assembly code, and it's almost 
identical. I don't know the exact reasons why, but I suspect the processor is 
better able to optimize the execution of the second function.
- Using panics is faster than returning/checking errors by ~1%. This confirms
our intuition, but it isn't anywhere near 40%. Maybe if you had deep call 
stacks, it would add up to something, but I don't believe it would be 40% 
faster.
- Setting up a `defer` block (when you have lots of iterations) seems to have
negligible impact.

## Conclusion
As we suspected, using panic instead of returning errors is slightly faster.
However this was measured using a micro-benchmark; for real programs, I
suspect you won't be able to measure any significant difference.

If we are to believe the results of the original article, I am more inclined to
think that they are because of other compiler optimizations, rather than the
removal of if-statements.

_Notes:_

[^1]: [Benchmark code](https://gist.github.com/bluescreen10/6776ac37657c3a9be15423fcfa638e35)
