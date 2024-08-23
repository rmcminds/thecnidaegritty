---
layout: post
title:  "Viva Shiny!"
date:   2024-08-23
categories: blog-meta
author: rmcminds
excerpt_separator: <!--more-->
---

I've finally returned to the Cnidae Gritty because I found a groove with Shiny app development again!

Since my postdoc in France, I've been wanting to improve a barebones photo annotation script that I created there.<!--more--> At the time, I needed a quick solution, and a colleague mentioned how easy it was to make such a thing with MATLAB. I had never worked with MATLAB before, but I was able to hack something together in just a couple days by piecing together a few code snippets that I found via Googling. It served its purpose, but it was uuug-ly. I also just never liked that I had to do it in MATLAB - I was familiar with R, and R is free! So I always imagined it would be nice to translate the program.

The obvious solution in R is to use the Shiny ecosystem. Shiny is an interface between R, HTML, and Javascript, which enables you to use all the data management, stats, and plotting functions in R, and then wrap them up in a user interface within a web browser. It's a little more complicated than the direct interactivity baked into MATLAB, but there are benefits to putting up with the learning curve. Long story short - not only was I able to translate and improve my old MATLAB workflow, it occurred to me during the process that it wouldn't be hard to add my new app to this website, making it far more accessible for potential users.

So that's how the [Photo Annotator](/photo_annotator/) came to be. 

The key thing that I wanted when developing this workflow was a very strict and controlled process of annotation that would ensure consistency across photos. When I first made it, the options that existed had a number of things that I didn't like. Though I try to automate as many of my analyses with code as possible, photo annotation inherently lends itself to some kind of graphical user inferface, or GUI. And the GUIs that I found were very flexible. But there were a number of problems that I wanted to address:

1. I wanted a simple data matrix as an output - each photo has a row, each annotation has a column.
2. Each annotation should use controlled vocabulary - if the question is 'Are there corals?', I don't want a messy column of data that contains `yes`, `Yes`, `yes `, `TRUE`, `true`, `Porites`, `maybe`, etc. The possibilities should be restricted upfront - it's much easier to have the annotator choose directly among 'yes', 'no', 'maybe', and 'not_applicable' than it is to let them type arbitrary text and then later struggle to interpret how edge cases should fit into those categories.
3. For a given project stage, each photo should be _fully_ annotated before you move on to the next. A GUI that has 30 annotation buttons on the side has the potential to lose a user. When annotating 3,000 photos over the course of months, we don't want to start by choosing some annotations, accidentally skip some for some photos, and get into a different groove by the end. And the output dataset shouldn't have any ambiguous missing values - a cell either has an annotation, or is explicitly marked as not applicable or impossible to determine. 
4. Annotation order should be randomized. Despite the strict workflow, annotators will have biases associated with the time of annotation. If you annotate photos from one reef one day, you might learn that the reef has a lot of sponges, and by the end of the day you will be more confidently annotating pixelated smears in the corner as definitely sponges. But another day annotating a different reef, you could be faced with an identical pixelated smear, but because you haven't seen many sponges in the photos that day, you're not so sure, and don't mark it as such. Don't get me going on how important randomization is for science.

So for my original script, I had a workflow hard-coded such that a single annotation was asked for at a time, and the whole set had to be completed before you moved onto the next photo. If there were questions like 'draw a box around the coral', the workflow was smart enough to skip it if the user had previously specified that there were no corals. The annotation would be automatically marked as `not applicable`, greatly speeding up the process of annotation without missing values.

During my R/Shiny translation, the greatest improvement is that I now have implemented some logic that generalizes the script for _any_ workflow and set of questions. You simply create a tab-delimited text file that has one row for each desired annotation. This row contains:

1. The annotation key (e.g. `has_corals`)
2. The controlled vocabulary or annotation type (e.g. '`yes`, `no`, `indiscernable`', or '`bounding_box_coordinates`')
3. The annotation prompt presented to the user (e.g. `Are there any corals?`)
4. A list of dependencies (see below)

I'm not gonna lie, I used ChatGPT to help me figure out the logic for implementing the dependencies. And it just, gave me something that worked! With a few tweaks, of course. Without going into details, the idea is that a given annotation shouldn't be prompted unless it is relevant according to previous answers. So the prompt asking for bounding box annotations for individual corals has a value of `has_corals:yes` in the dependencies column. If the previous annotation doesn't have a matching value, the corresponding prompt is skipped. I was actually **really** excited about this feature.

The other neat discovery I made this time was `shinylive`. Since this website is hosted on GitHub Pages, it is a 'static' site. That means, when you visit it, your browser is accessing files on the server, and the server is not going to run any code. To have any interactivity on such a site, there are two options: host an app on a different server, and embed it in a static page (this is what I did with [iNatle](/iNatle/)), or write the app in Javascript, which your browser essentially downloads and runs on *your* computer. The weird scrolling and scaling issues that are currently a little frustrating with iNatle are complications resulting from hosting it at shinyapps.io and embedding it here. And having to separately maintain the app with an account at shinyapps.io is also a little annoying. shinylive basically just packages a Shiny app for a static site. Very convenient!!

Of course, there are many highly experienced photo annotators out there who could probably tell me that making my own app was unnecessary. I had put in quite a bit of effort during my postdoc to find such a thing, and ultimately gave up. In the intervening years, a quick Google search does suggest that there are new tools out there that might work better. But I can't help being a wheel inventor. It was easier, more fun, and more of a learning experience for me to work on this code than to find a pre-baked workflow that worked for me.

And it reminded me of the Shiny app that I already had on this site, [iNatle](/iNatle/). That, too, was a fun project that I did last year during paternity leave. It broke shortly after I added it, and honestly wasn't as fun as I was hoping, at first. But I was inspired to work on that, too, this last week...

(Tune in next time!)
