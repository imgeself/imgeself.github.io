---
layout: post
title: "Raytracer 1: Little More Advanced Shading"
date: "2019-02-19 22:13:28 +0300"
---

Last post, I made a ray tracer with very basic direct illumination code. It was good
but we need go further for more fun. So it's time to implement some other material types.

## Diffuse, Lambertian, Dielectric
For the new materials, I added new variables to the material struct:
```c++
struct Material {
    float refractiveIndex; // Refractive index of material. 0 means no refraction.
    float reflection; // 0 is pure diffuse, 1 is mirror.
    Vector3 color;
};
```

Core `RaytraceWord` [function](https://github.com/imgeself/raytracer/blob/master/main.cpp#L72){:target="_blank"}
is changed for the new material types. And it's now work like this:
```c++
// Main ray trace function.
// I use a loop-based tracing instead of recursion-based trace function.
// You can write clean code by using recursion but I find recursion hard to understand.
// This way is more straightforward and understandable for me.
Vector3 RaytraceWorld(World* world, Ray* ray) {
  for (bounce count) {    
    check for intersection
    if (isIntersect) {
      attenuation *= mat.color;

      isRefract = check for refraction

      if (material is dielectric && isRefract) {
        set refractedRay
        calculate the fresnelCoefficient
      }

      mirrorBounce = perfect bounce along surface normal
      randomBounce = random ray along surface normal
      reflectedRay = lerp between random and mirror bounce based on material's reflection coefficient

      picking the next ray direction between reflected and refracted ray based on fresnelCoefficient
      // We use the Russian Roulette method for determining which way to go
      ...

    } else {
       // Hit nothing (sky)
       result = attenuation;
    }
  }

  return color
}
```

When trying to implement new materials, I end up getting weird rendering artifacts
along the way. Just looking for them, tracing the ray transport for why the
artifacts happen, is amazingly fun and informative. Deep diving the math
equations and looking derivations of them, also fun and informative.

When we run this code we get this interesting image.

![Render1spp](/assets/img/render-1spp.bmp)

## Sampling

This rendering algorithm based on probability. The more we sample, the better the image gets.
So I put `RaytraceWorld` function into a sample loop. Then we calculate the average color and write into the image.

```c++
Vector3 color(0.0f, 0.0f, 0.0f);
for (sampleSize) {

    ...

    Ray ray = {};
    ray.origin = cameraPosition;
    ray.direction = Normalize(filmPosition - cameraPosition);

    color += RaytraceWorld(&world, &ray);
}

 pixelColor = (color / sampleSize);
```

> Note: Sampling is actually an advanced topic in rendering. And there is a tradeoff between sampling and performance. Increasing sample size make the image look cool but it slows the program's execution time. What I did here is a very basic way of sampling. Maybe we implement more clear sampling techniques like [importance sampling](https://en.wikipedia.org/wiki/Importance_sampling){:target="_blank"} in the future.

When we run our code in 512 sample per pixel, we get this nice, smooth image.

![Render](/assets/img/render-post1.bmp)

## What's Next

I want to implement more features to this ray tracer such as texture mapping,
light emitters, 3D model rendering, etc. But before all of that, I will try to
optimize the program and try to add a custom random function, implement multithreading,
 simd, etc.
