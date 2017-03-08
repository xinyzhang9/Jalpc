---
layout: post
title:  "Movie recommender using Rxjs"
date:   2016-11-18
desc: "Movie recommender using Rxjs"
keywords: "Rxjs, movie, tutorial"
categories: [Javascript,Frontend]
tags: [Rxjs,ReactiveX]
icon: icon-javascript
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
* When user open the page, it renders 3 movie suggestions via calling from API endpoints.  
* When user click 'refresh' button, it re-render 3 new movie suggestions.  
* When user click 'x' button on one of the movie suggestion, it replaces with a new suggestion.  
* When user click movie title, a new tab of movie homepage will show up.  
* When user click 'add to cart' on one of the movie suggestions, the movie will be added to user's cart(collections) which is located on the right up corner of page.  
* User's cart is saved in local storage(offline) so user can still access that when revisiting page.
The screenshot is shown below.  
<img src="https://raw.githubusercontent.com/xinyzhang9/movie_recommender/master/movie2.png" alt="Drawing" style="width: 100%;"/>

## Key steps
Create observables from click event on the 3 close buttons and 1 refresh button.  
```
var closeButton1 = document.querySelector('.close1');
var closeButton2 = document.querySelector('.close2');
var closeButton3 = document.querySelector('.close3');


var refreshClickStream = Rx.Observable.fromEvent(refreshButton, 'click');
var close1ClickStream = Rx.Observable.fromEvent(closeButton1, 'click');
var close2ClickStream = Rx.Observable.fromEvent(closeButton2, 'click');
var close3ClickStream = Rx.Observable.fromEvent(closeButton3, 'click');
```

Define request stream and respond stream.  When the page is loaded at the first time or user click refresh button, it will fire a request stream, which contains a API GET request. For the response stream, it maps each request stream to a promise object.  
```
var requestStream = refreshClickStream.startWith('startup click')
    .map(function(){
        var page = Math.floor(Math.random()*100)+1
        var randomOffset = Math.floor(Math.random()*20);
        return 'https://api.themoviedb.org/3/discover/movie?
                api_key='+API_KEY+'&page='+page;
    })

var responseStream = requestStream
    .flatMap(function (requestUrl) {
        return Rx.Observable.fromPromise($.getJSON(requestUrl));
    });
```

To understand how **flatMap** works, please see the following diagram.  
![alt](https://segmentfault.com/image?src=http://i.imgur.com/Hi3zNzJ.png&objectId=1190000004293922&token=e2d14a0bb39789fbea2aecf7abc4fcd0)  

Create suggestion stream. The idea is we want to render 3 suggestion streams, which is a random chice from the returned movies list with length 20. We can combine the closeclick stream(user click 'x') with the latest response stream. So when user click 'x', that suggestion is replaced by another item from the latest returned movie list without calling API again. merge() is used to merge the multiple streams into one stream.  
```
function createSuggestionStream(closeClickStream) {
    return closeClickStream.startWith('startup click')
        .combineLatest(responseStream,             
            function(click, listMovies) {
                return listMovies['results'][Math.floor(Math.random()*20)];
            }
        )
        .merge(
            refreshClickStream.map(function(){ 
                return null;
            })
        )
        .startWith(null);
}
```

Final step is to render the stream. It is basically mapping JSON key-values.  
```
// Rendering ---------------------------------------------------
function renderSuggestion(suggestedMovie, selector) {
    var suggestionEl = document.querySelector(selector);
    if (suggestedMovie === null) {
        suggestionEl.style.visibility = 'hidden';
    } else {
        suggestionEl.style.visibility = 'visible';
        var movienameEl = suggestionEl.querySelector('.moviename');
        movienameEl.href = 'https://www.themoviedb.org/movie/'+suggestedMovie.id;
        movienameEl.textContent = suggestedMovie.title;
        var imgEl = suggestionEl.querySelector('img');
        imgEl.src = "";
        imgEl.src = 'https://image.tmdb.org/t/p/w500'+ suggestedMovie.poster_path;

        var overviewEl = suggestionEl.querySelector('.overview');
        overviewEl.textContent = suggestedMovie.overview;

        var releaseEL = suggestionEl.querySelector('.release_date');
        releaseEL.textContent = suggestedMovie.release_date;

        var voteEL = suggestionEl.querySelector('.vote_average');
        voteEL.textContent = suggestedMovie.vote_average+'/10';

        var addToCartEl = suggestionEl.querySelector('.addToCart');

        // dirty way to solve the 'overlap-element' bug
        addToCartEl.id = suggestedMovie.id+'#'+suggestedMovie.poster_path;
        addToCartEl.addEventListener("click", function(){
            // console.log(suggestedMovie.id);
            if(cart.indexOf(this.id) < 0){
                cart.push(this.id);
                localStorage.setObj('movie_cart',cart);
                //re-render dropdown
                initDropdown();
            }
        });
    }
}
```

There are some tricks in the rendering function. If the suggestion is empty, we want to hide it. Otherwise we set it as visible. And we want to add a event listener for each 'add to cart' button. We maintain a variable 'cart' to record user's collections. For each item, we push the movie id + movie image src to the cart. Make sure 'this' is used correctly.

For the full source code, please visit my [Github Page](https://github.com/xinyzhang9/movie_recommender)  

Thanks for reading.
