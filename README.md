<!--

@license Apache-2.0

Copyright (c) 2018 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# MT19937

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Create an iterator for a 32-bit [Mersenne Twister][mersenne-twister] pseudorandom number generator.



<section class="usage">

## Usage

```javascript
import iterator from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-iter-mt19937@esm/index.mjs';
```

#### iterator( \[options] )

Returns an iterator for generating pseudorandom numbers via a 32-bit [Mersenne Twister][mersenne-twister] pseudorandom number generator.

```javascript
var it = iterator();
// returns <Object>

var r = it.next().value;
// returns <number>

r = it.next().value;
// returns <number>

r = it.next().value;
// returns <number>

// ...
```

The function accepts the following `options`:

-   **normalized**: `boolean` indicating whether to return pseudorandom numbers on the interval `[0,1)`.
-   **seed**: pseudorandom number generator seed.
-   **state**: a [`Uint32Array`][@stdlib/array/uint32] containing pseudorandom number generator state. If provided, the function ignores the `seed` option.
-   **copy**: `boolean` indicating whether to copy a provided pseudorandom number generator state. Setting this option to `false` allows sharing state between two or more pseudorandom number generators. Setting this option to `true` ensures that a returned iterator has exclusive control over its internal pseudorandom number generator state. Default: `true`.
-   **iter**: number of iterations.

To return pseudorandom numbers on the interval `[0,1)`, set the `normalized` option.

```javascript
var it = iterator({
    'normalized': true
});

var r = it.next().value;
// returns <number>
```

To return an iterator having a specific initial state, set the iterator `state` option.

```javascript
var bool;
var it1;
var it2;
var r;
var i;

it1 = iterator();

// Generate pseudorandom numbers, thus progressing the generator state:
for ( i = 0; i < 1000; i++ ) {
    r = it1.next().value;
}

// Create a new iterator initialized to the current state of `it1`:
it2 = iterator({
    'state': it1.state
});

// Test that the generated pseudorandom numbers are the same:
bool = ( it1.next().value === it2.next().value );
// returns true
```

To seed the iterator, set the `seed` option.

```javascript
var it = iterator({
    'seed': 12345
});

var r = it.next().value;
// returns 3992670690

it = iterator({
    'seed': 12345
});

r = it.next().value;
// returns 3992670690
```

To limit the number of iterations, set the `iter` option.

```javascript
var it = iterator({
    'iter': 2
});

var r = it.next().value;
// returns <number>

r = it.next().value;
// returns <number>

r = it.next().done;
// returns true
```

The returned iterator protocol-compliant object has the following properties:

-   **next**: function which returns an iterator protocol-compliant object containing the next iterated value (if one exists) assigned to a `value` property and a `done` property having a `boolean` value indicating whether the iterator is finished.
-   **return**: function which closes an iterator and returns a single (optional) argument in an iterator protocol-compliant object.
-   **seed**: pseudorandom number generator seed.
-   **seedLength**: length of generator seed.
-   **state**: writable property for getting and setting the generator state.
-   **stateLength**: length of generator state.
-   **byteLength**: size (in bytes) of generator state.

</section>

<!-- /.usage -->

<section class="notes">

## Notes

-   If an environment supports `Symbol.iterator`, the returned iterator is iterable.
-   [Mersenne Twister][mersenne-twister] is **not** a cryptographically secure PRNG, as the PRNG is based on a linear recursion. Any pseudorandom number sequence generated by a linear recursion is **insecure**, due to the fact that one can predict future generated outputs by observing a sufficiently long subsequence of generated values.
-   The PRNG has a period of `2^19937 - 1`.
-   If PRNG state is "shared" (meaning a state array was provided during iterator creation and **not** copied) and one sets the underlying generator state to a state array having a different length, the iterator does **not** update the existing shared state and, instead, points to the newly provided state array. In order to synchronize the output of the underlying generator according to the new shared state array, the state array for **each** relevant iterator and/or PRNG must be **explicitly** set.
-   If PRNG state is "shared" and one sets the underlying generator state to a state array of the same length, the PRNG state is updated (along with the state of all other iterator and/or PRNGs sharing the PRNG's state array).

</section>

<!-- /.notes -->

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```html
<!DOCTYPE html>
<html lang="en">
<body>
<script type="module">

import iterator from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-iter-mt19937@esm/index.mjs';

var it;
var r;

// Create a seeded iterator for generating pseudorandom numbers:
it = iterator({
    'seed': 1234,
    'iter': 10
});

// Perform manual iteration...
while ( true ) {
    r = it.next();
    if ( r.done ) {
        break;
    }
    console.log( r.value );
}

</script>
</body>
</html>
```

</section>

<!-- /.examples -->

* * *

<section class="references">

## References

-   Matsumoto, Makoto, and Takuji Nishimura. 1998. "Mersenne Twister: A 623-dimensionally Equidistributed Uniform Pseudo-random Number Generator." _ACM Transactions on Modeling and Computer Simulation_ 8 (1). New York, NY, USA: ACM: 3–30. doi:[10.1145/272991.272995][@matsumoto:1998a].
-   Harase, Shin. 2017. "Conversion of Mersenne Twister to double-precision floating-point numbers." _ArXiv_ abs/1708.06018 (September). <https://arxiv.org/abs/1708.06018>.

</section>

<!-- /.references -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

* * *

## See Also

-   <span class="package-name">[`@stdlib/random-base/mt19937`][@stdlib/random/base/mt19937]</span><span class="delimiter">: </span><span class="description">A 32-bit Mersenne Twister pseudorandom number generator.</span>
-   <span class="package-name">[`@stdlib/random-iter/minstd`][@stdlib/random/iter/minstd]</span><span class="delimiter">: </span><span class="description">create an iterator for a linear congruential pseudorandom number generator (LCG) based on Park and Miller.</span>
-   <span class="package-name">[`@stdlib/random-iter/minstd-shuffle`][@stdlib/random/iter/minstd-shuffle]</span><span class="delimiter">: </span><span class="description">create an iterator for a linear congruential pseudorandom number generator (LCG) whose output is shuffled.</span>
-   <span class="package-name">[`@stdlib/random-iter/randi`][@stdlib/random/iter/randi]</span><span class="delimiter">: </span><span class="description">create an iterator for generating pseudorandom numbers having integer values.</span>
-   <span class="package-name">[`@stdlib/random-iter/randu`][@stdlib/random/iter/randu]</span><span class="delimiter">: </span><span class="description">create an iterator for generating uniformly distributed pseudorandom numbers between 0 and 1.</span>

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2024. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/random-iter-mt19937.svg
[npm-url]: https://npmjs.org/package/@stdlib/random-iter-mt19937

[test-image]: https://github.com/stdlib-js/random-iter-mt19937/actions/workflows/test.yml/badge.svg?branch=main
[test-url]: https://github.com/stdlib-js/random-iter-mt19937/actions/workflows/test.yml?query=branch:main

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/random-iter-mt19937/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/random-iter-mt19937?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/random-iter-mt19937.svg
[dependencies-url]: https://david-dm.org/stdlib-js/random-iter-mt19937/main

-->

[chat-image]: https://img.shields.io/gitter/room/stdlib-js/stdlib.svg
[chat-url]: https://app.gitter.im/#/room/#stdlib-js_stdlib:gitter.im

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/random-iter-mt19937/tree/deno
[deno-readme]: https://github.com/stdlib-js/random-iter-mt19937/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/random-iter-mt19937/tree/umd
[umd-readme]: https://github.com/stdlib-js/random-iter-mt19937/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/random-iter-mt19937/tree/esm
[esm-readme]: https://github.com/stdlib-js/random-iter-mt19937/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/random-iter-mt19937/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/random-iter-mt19937/main/LICENSE

[mersenne-twister]: https://en.wikipedia.org/wiki/Mersenne_Twister

[@matsumoto:1998a]: https://doi.org/10.1145/272991.272995

[@stdlib/array/uint32]: https://github.com/stdlib-js/array-uint32/tree/esm

<!-- <related-links> -->

[@stdlib/random/base/mt19937]: https://github.com/stdlib-js/random-base-mt19937/tree/esm

[@stdlib/random/iter/minstd]: https://github.com/stdlib-js/random-iter-minstd/tree/esm

[@stdlib/random/iter/minstd-shuffle]: https://github.com/stdlib-js/random-iter-minstd-shuffle/tree/esm

[@stdlib/random/iter/randi]: https://github.com/stdlib-js/random-iter-randi/tree/esm

[@stdlib/random/iter/randu]: https://github.com/stdlib-js/random-iter-randu/tree/esm

<!-- </related-links> -->

</section>

<!-- /.links -->
