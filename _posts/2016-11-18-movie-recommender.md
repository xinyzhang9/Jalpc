---
layout: post
title:  "Movie recommender using Rxjs"
date:   2016-11-18
desc: "Movie recommender using Rxjs"
keywords: "Rxjs, movie, tutorial"
categories: [Javascript]
tags: [Rxjs]
icon: icon-html
---
## Introduction
Recently I am very interested in Reactive Programming, so I did a mini project "movie recommender" to generate a movie recommending list for users using ReactiveX asynchronous data streaming concept.

## Prior Knowledge
What is ReactiveX?
ReactiveX is a library for composing asynchronous and event-based programs by using observable sequence. The concept is shown in the following diagrams.  
![alt](https://segmentfault.com/image?src=http://i.imgur.com/cL4MOsS.png&objectId=1190000004293922&token=7a0e3538a8320e9e2ecdbc7cb8e8c096)  
Stream is an event time sequence. You can emit certain type of values, errors, and completed signals. We capture emitted events in asynchronous way. Sometimes we omit error signals and just focus on handling values. The monitoring of stream is called subscribe. The function to handle stream is called observer. In this way, the streams in ReactiveX is also called Observables.  

## Project Objectives
The objectives of this movie recommender is as follows.
- [x] When user open the page, it renders 3 movie suggestions via calling from API endpoints.  
- [x] When user click 'refresh' button, it re-render 3 new movie suggestions.  
- [x] When user click 'x' button on one of the movie suggestion, it replaces with a new suggestion.  
- [x] When user click movie title, a new tab of movie homepage will show up.  
- [x] When user click 'add to cart' on one of the movie suggestions, the movie will be added to user's cart(collections) which is located on the right up corner of page.  
- [x] User's cart is saved in local storage(offline) so user can still access that when revisiting page.
The screenshot is shown below.  
![alt](https://raw.githubusercontent.com/xinyzhang9/movie_recommender/master/movie2.png)

## Key steps
1. Create observables from click event on the 3 close buttons and 1 refresh button.  
```
var closeButton1 = document.querySelector('.close1');
var closeButton2 = document.querySelector('.close2');
var closeButton3 = document.querySelector('.close3');


var refreshClickStream = Rx.Observable.fromEvent(refreshButton, 'click');
var close1ClickStream = Rx.Observable.fromEvent(closeButton1, 'click');
var close2ClickStream = Rx.Observable.fromEvent(closeButton2, 'click');
var close3ClickStream = Rx.Observable.fromEvent(closeButton3, 'click');
```
