---
layout: post
title: "Raytracer 0: Intro"
date: "2019-01-02 23:16:27 +0300"
---

I started to develop raytracer using C++ for learning computer graphics.  

You can find source code in [here.](https://github.com/imgeself/raytracer/tree/baba952f46f980c8ff1295f594b20bef437d82a2){:target="_blank"}

Main references for this raytracer are:

 - [Handmade Ray](https://hero.handmade.network/episode/ray/){:target="_blank"} (Raytracing extension of Handmade Hero)
 - [Peter Shirley's ray tracing books](https://github.com/petershirley){:target="_blank"}
 - [Aras's amazing blog posts](http://aras-p.info/blog/2018/03/28/Daily-Pathtracer-Part-0-Intro/){:target="_blank"}
 - [Scratchapixel](https://www.scratchapixel.com/index.php){:target="_blank"}

First, I wrote [BMP](https://en.wikipedia.org/wiki/BMP_file_format){:target="_blank"} image writer and math, vector utils. 
Then I wrote sphere, plane, world, light structures and basic intersection code. 
Most raytracer tutorials starts teaching using global illumination and there is a good reason for that.
The power of raytracing is creating a photorealistic image using global illumination in relatively small code. 
But I started using direct illumination from my small OpenGL-rendering experiences to understand the core concept.

![Diffuse Lighting](/assets/img/diffuse.bmp)

After writing the diffuse lighting code image is already getting started to look good and 3D-ish. Now, we need hard shadows for direct illumination.
I used a very simple solution for hard shadows. Just throw shadow ray to light and check for intersection. If there is a intersection then threat that pixel as a shadow. Super simple stuff:

![Hard Shadows](/assets/img/hardshadow.bmp)

Then I added more lights to the scene and wrote very simple and not physically accurate multi light support. Just for seeing multiple shadows. And image looks good for such basic calculations.
In the next blog post, I will try to implement little more realistic light calculations for this raytracer.

![Render](/assets/img/render-post0.bmp)
