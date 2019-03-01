---
layout: post
title: "Raytracer 2: Custom Random Number Generator Function"
date: "2019-02-26 20:42:39 +0300"
---

Last post we implement some cool features to the ray tracer. For going furthermore
first we need to improve the performance of our program. In this way, we can iterate faster.

## Some Measurements
Before doing any optimization, let's measure the current code. I'm using the built-in
[clock API](http://www.cplusplus.com/reference/ctime/clock/) instead of using some
benchmarking library. We are measuring big long loops. The clock API's resolution
enough for our purposes now. The current result when 32 rays per pixel is:
  - On my mac laptop (Core i7-7660U 2.50GHz): **18.1 Mray/s** which means **0.000055 ms/ray**

## Xorshift

For the first step of optimizing, I decided to replace built-in ```rand()``` with
a custom random number generator function. Because ```rand()``` not very fast generator and
changing that is a good start for optimization.

[Xorshift](https://en.wikipedia.org/wiki/Xorshift) is a random number generator
function that produces reasonably random numbers very fast. It's a few lines of code
and very easy to implement.

```c++  
inline uint32_t XOrShift32(uint32_t *state)
{
    uint32_t x = *state;
    x ^= x << 13;
    x ^= x >> 17;
    x ^= x << 5;
    *state = x;
    return x;
}
```

After implementing the function performance up to **21.0 Mray/s, 0.000048 ms/ray**
on the same rays per pixel size as before. That's a **~%16** performance gain for such a small change.
And the rendered image is nearly the same as before. No artifacts.

I'm finishing the blog post here to keep it small. Next up: I will try to implement multithreading.
