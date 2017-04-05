---
layout: post
title: "Resize Problem"
date: 2017-04-06
subtitle: "CS50 Pset4: Resize, my struggle with it."
categories: [2017]
permalink: /resize-problem/
---

As I mentioned in my Facebook post, this problem deserves a blog post of its
own, so here it is.

### Note: This post will not help you in anyway to solve the [Problem](https://goo.gl/67djis). It is just a summary of my experience with this [problem](https://goo.gl/67djis).

So, It all started after I finished the previous problem, that is,
[Whodunit](https://goo.gl/M91fgo). So, I quickly went to check the
[docs](https://goo.gl/67djis) for next problem, that is,
[Resize](https://goo.gl/67djis). There was some
[distribution code](https://goo.gl/UemIf2), so I quickly downloaded it with
[Wget](https://www.gnu.org/software/wget/) on both my Local Machine and
[CS50 IDE](https://cs50.io) by [Cloud9](https://c9.io).
[Background](https://goo.gl/HxFbGH) of it was just to read the
[docs](https://goo.gl/M91fgo) of previous problem and understand it, which I
already did. BTW, those docs where about how a
[Bitmap file](https://goo.gl/FvUS4m) is stored, its
[Headers](https://goo.gl/Pv8Wm6), and RGB triples.

[Specifications](https://goo.gl/jb0vNK) were to resize the image with a factor
of "n". I thought, that's easy to do. And then a bunch of
[bullet points](https://goo.gl/y3XHfT) for
[error codes](https://goo.gl/cXqCDS) for specific errors.

Then came the 11 minutes long [Walkthrough](https://goo.gl/qWBbhR), as always by
[Zamyla Chan](https://goo.gl/fv4eXc). A very good one, in which she quite
accurately explained how to break down the problem into easily solvable parts.

I just checked the starting of the [Walkthrough](https://goo.gl/qWBbhR) till the
point where she breaks down problem. Then I checked the
[distribution code](https://goo.gl/UemIf2). It was the exact same Source code
files, copy.c, bmp.h from the previous problem. Images provided were also
almost same just clue.bmp file was missing, which was not required in this
problem. I was already familiar with the source code. Basically, copy.c was a
very simple program which first reads Headers and writes them into new file,
then do the same with RGB triples and bmp.h was header file with all the
required structs defined in it.

I actually started working on it a day later. So, when I actually
started working, I first went through the complete
[Walkthrough](https://goo.gl/qWBbhR) quickly. Then I opened the docs of previos
problem, [Whodunit](https://goo.gl/M91fgo) in a separate Tab, for reference.

So, basically, Header files in a Bitmap image contains meta data about images,
such as Heigh, Width, Size and other stuff, that you generally see in properties
of the image. So, in order to resize the image as whole, we first need to change
the meta data, specifically, Height, Width, Bitmap image size and Bitmap file
size. So, I did that in like 3-4 tries. Finally the output file's meta data were
resized. But it was no good without the actual image right?

Then came the hard part, to actually resize the image. So, in order to resize
image, we first need to resize it horizontally, that is, write each pixel "n"
times. Then we have to calculate New padding for new Bitmap file and add it to
Outfile. Then we need to write this whole row "n" times in order to resize it
vertically.

![Horizontally Resized](/assets/images/resize/1.png)

I did the horizontal part very easily in 2-3 tries and padding was very simple
to add as well. But the vertical resizing was the part that really pissed me
off.

So, there were two ways of doing it, first to iterate over and over the row, "n"
times and write it into the outfile, second was to store the entire row in an
array or something like that and then write that array, "n" times.

I went ahead with first method first. I tried implementing in a lot of ways.
And got strange outputs, sometimes even the color changed, as I wrote some bytes
which were not RGB triples.

![Weird output 1](/assets/images/resize/2.png)
![Weird output 2](/assets/images/resize/3.png)
![Weird output 3](/assets/images/resize/4.png)

I kept chaning the code and logic, but it was no good. Once I even tried
rewriting code, but that was no good either. The problem was that I was not sure
what's the problem in logic or code. This was probably the problem in CS50,
which took longest to finish. I kept trying for over a Week or more. At one
point I even switched my Editor. But nothing worked.

Then, after getting frustrated, I asked about my problem in [CS50's Gitter
channel](https://gitter.im/cs50/x). Thankfully, a guy, [Charles Rector](https://goo.gl/0OlhqQ) a.k.a
[@chuckrector](https://github.com/chuckrector) helped me went through the code.
Then finally he kinda gave me a hint for trying the other method. During this
one Week time, I kinda tried it but I though it would be very hard to iterate
and increament value, I even for a short while though of using a 2-D array, but
that would have been very complicated. But then I went ahead and actually try
and implement it. I though about it differently this time and I realised it was
indeed not that hard to use and implement it. Also, it was more efficient code,
as we don't have to iterate over the same row again and again and again.

So, at first, for testing purposes, I used array with static size and a little
complicated way to implement a for loop to write. It gave me something like this
as output.

![almost there](/assets/images/resize/5.png)

Then while going through the code, I realised there was some fundamental problem
which can be resolved quite easily. And so finally after all this struggle,
finally, I got this as output.

![Final](/assets/images/resize/6.png)

And that's how I did it. Code that I wrote is [here](https://goo.gl/37xwHD).
