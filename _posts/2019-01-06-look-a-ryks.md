---
title: 'Look-a-Ryks: find your doppelganger in the Rijksmuseum collection (2017)'
date: 2019/01/06
layout: post
---

[Look-a-Ryks](http://look-a-ryks.herokuapp.com) matches your face against portraits in the [Rijksmuseum art collection](https://www.rijksmuseum.nl/en/rijksstudio). It uses [AWS Rekognition](https://aws.amazon.com/rekognition)

![](/media/2019-01-06-look-a-ryks-1.png)
_Amazon thinks I look like the [creepy older brother of painter Frederik Weissenbruch](https://www.rijksmuseum.nl/en/collection/RP-P-1889-A-14501A)._

A couple of years ago I ran into a picture of a [crashed facial recognition system](https://boingboing.net/2017/05/11/public-private-surveillance.html) in a Oslo pizza place.

I thought it foreshadowed a pretty bleak world where you'll be filmed, scanned,  recognised, analysed and remembered in every store you'll ever visit. This will be to provide a "superior customer service" of course, but we all [know what happens next](https://www.bloomberg.com/news/articles/2017-05-19/uber-s-future-may-rely-on-predicting-how-much-you-re-willing-to-pay).

![](/media/2019-01-06-look-a-ryks-2.png)

I wondered if we could come up with use cases for facial recognition that weren't completely evil.

## Idea

First, we'll need the facial recognition technology. This can be provided entirely by [Amazon Rekognition](https://aws.amazon.com/rekognition). This is the service [under fire for selling to US law enforcement agencies](https://techcrunch.com/2019/01/17/amazon-shareholders-want-the-company-to-stop-selling-facial-recognition-to-law-enforcement).

Secondly, we'll need a massive dataset of faces to play with. Luckily, the Rijksmuseum in Amsterdam has an amazing API that [exposes almost 600.000 works of art](https://www.rijksmuseum.nl/en/api).

I named it [Look-a-Ryks](http://look-a-ryks.herokuapp.com).

![](/media/2019-01-06-look-a-ryks-3.jpeg)
_The actual Rijksmuseum_

## How it works

To start us off we'll need all relevant objects from the collection. I decided to go for the easy route by downloading all data for the search term "portret" ("portrait"), of which there are 37,000.

Using some one-off scripts on an EC2 instance, I downloaded all of the images, put them in an S3 bucket and then forwarded them to Rekognition. The [code for the download and upload is here](https://github.com/tijmenb/rijksmuseum-aws-experiment), but it's not super reproducible.

After this, I created a [small application](https://github.com/tijmenb/look-a-ryks) where users can upload a photo and see their doppelganger from the Rijksmuseum collections.

![](/media/2019-01-06-look-a-ryks-4.png)

## The result

For a lot of people, the application manages to find a portrait that at least has a passing resemblence. It was enough for a delivery manager in GDS to print out the doppelgangers of team members for on the wall.

![](/media/2019-01-06-look-a-ryks-5.jpeg)
