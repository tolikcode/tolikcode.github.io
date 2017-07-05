+++
date = "2017-06-28"
title = "mergeMap vs concatMap vs switchMap"

+++

Today we're going to look at the difference between these three [RxJS](http://reactivex.io/rxjs/) operators. This might be an obvious thing for someone doing reactive programming in their daily job. But for me, it took a few hours to play with these operators to thoroughly understand them. So I've decided to spend a few more hours in writing a blog post about it :)

<!--more-->

## TL;DR
Here's an [example at JsFiddle](https://jsfiddle.net/tolikcode/2uqrghy0/) showing the difference between the operators.

## When do you need them?

All these operators are used with so-called higher order Observables. This is when items of an Observable are Observables themselves (or they are mapped to Observables) and we need to flatten all of them into one final Observable. In my examples, initial Observable is called **outer** Observable. And items of the outer Observable are called **inner** Observables. You can easily identify this situation when you subscribe to an Observable inside subscription to another Observable (Note: this is **not** a recommended approach): 

~~~ javascript
outerObservable.subscribe(outerItem => {
    outerItem.subsribe(innerItem => {
            foo(innerItem);
        })});
~~~

A usage example for such operators can be a search box (search box text changes as outer Observable) with a request being sent to a server for each search text change (HTTP responses as inner Observables). Another example is mouse button clicks (outer Observable) that trigger an interval timer for each mouse click (timer events as inner Observables).


## Learning plan

To tackle these operators you need to understand more basic ones first. In this article I won't give any definitions or explanations per se, but rather just a small learning plan with links:

1. [Map](http://reactivex.io/documentation/operators/map.html), [Merge](http://reactivex.io/documentation/operators/merge.html), [Concat](http://reactivex.io/documentation/operators/concat). Basic Observable operators.
2. [MergeAll](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#instance-method-mergeAll), [ConcatAll](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#instance-method-concatAll), [Switch](http://reactivex.io/documentation/operators/switch.html). These are for higher order Observables already.
3. After that [MergeMap](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#instance-method-mergeMap), [ConcatMap](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#instance-method-concatMap), and [SwitchMap](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#instance-method-switchMap) should be easy for you.

Operators from the third group are two step operators. First, they map outer Observable items to inner Observables. The second step is to merge a result set of inner Observables in some way. The way they are merged depends on the operator you use. 

- mergeMap = map + mergeAll
- concatMap = map + concatAll
- switchMap = map + switch

Here are some pictures from [reactivex.io](http://reactivex.io) that I think are quite descriptive:

<div class="standardBorder verticalMargins" markdown="1">
	<img src="/images/mergeMap.png" alt="MergeMap">
</div>

<div class="standardBorder verticalMargins" markdown="1">
	<img src="/images/concatMap.png" alt="ConcatMap">
</div>

<div class="standardBorder verticalMargins" markdown="1">
	<img src="/images/switchMap.png" alt="SwitchMap">
</div>

## "Talk is cheap. Show me the code."

I still wasn't sure I had cracked the difference between the discussed operators even after reading all the results from Google's first page :)
I've set up a small example on [JsFiddle](https://jsfiddle.net/tolikcode/2uqrghy0/) to see the difference in practice.

In the example, outer Observable emits three items with a one-second interval. Each item is then mapped to an inner Observable, which in its turn emits another three items with a one-second interval.
The final result depends on the operator you use. Hope this small example will help you understand these operators without too much googling.
