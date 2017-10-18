+++
date = "2017-06-28"
title = "mergeMap vs flatMap vs concatMap vs switchMap"

+++

Today we're going to look at the difference between these ~~four~~ three [RxJS](http://reactivex.io/rxjs/) operators. The simple part is that `flatMap` is just an alias for `mergeMap`. Other operators have a difference that might be important in some cases.

<!--more-->

## When do you need them?

All these operators are used with so-called higher order Observables. This is when items of an Observable are Observables themselves (or they are mapped to Observables) and we need to flatten all of them into one final Observable. You can easily identify this situation when you subscribe to an Observable inside subscription to another Observable (Note: this is **not** a recommended approach): 

~~~ javascript
outerObservable.subscribe(outerItem => {
    outerItem.subsribe(innerItem => {
            foo(innerItem);
        })});
~~~

In my examples, initial Observable is called **outer** Observable. And items of the outer Observable are called **inner** Observables. Technically **inner** and **outer** Observables are just plain Observables.

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

Here are some reworked [reactivex.io](http://reactivex.io) diagrams. At first some explanations for the diagrams:

<div class="standardBorder verticalMargins" markdown="1">
	<img src="/images/rxjsDiagram.png" alt="MergeMap">
</div>

In these diagrams outer (initial) Observable emits circles. Each circle is then mapped to its own inner Observable - collection of rhombuses. Collections are identified by color; each collection has its own color. All those inner Observables are then merged into one final Observable - resulting collection of rhombuses.

<br>

<div class="standardBorder verticalMargins" markdown="1">
	<img src="/images/mergeMap.png" alt="MergeMap">
</div>

- `mergeMap` emits items into the resulting Observable just as they are emitted from inner Observables. It doesn't wait for anything.
- `mergeMap` doesn't preserve the order from outer Observable. Collections of rhombuses interleave.
- `mergeMap` doesn't cancel any inner Observables. All rhombuses from inner Observables get to final collection.

<br>

<div class="standardBorder verticalMargins" markdown="1">
	<img src="/images/concatMap.png" alt="ConcatMap">
</div>

- `concatMap` waits for inner Observable to complete before taking items from the next inner Observable.

- `concatMap` does preserve the order from outer Observable. Collections of rhombuses  don't interleave.

- Just as `mergeMap`, `concatMap` doesn't cancel any inner Observables. All rhombuses  from inner Observables get to the final collection.

<br>

<div class="standardBorder verticalMargins" markdown="1">
	<img src="/images/switchMap.png" alt="SwitchMap">
</div>

- `switchMap` emits items only from the most recent inner Observable.

- `switchMap` cancels previous inner Observables when a new inner Observable appears. Items of inner Observable that were emitted after the Observable was canceled will be lost (not included in the resulting Observable).

## "Talk is cheap. Show me the code."

I still wasn't sure I had cracked the difference between the discussed operators even after reading all the results from Google's first page :)
I've set up a small example on [JsFiddle](https://jsfiddle.net/tolikcode/2uqrghy0/) to see the difference in practice.

In the example, outer Observable emits three items with a one-second interval. Each item is then mapped to an inner Observable, which in its turn emits another three items with a one-second interval.
The final result depends on the operator you use. Hope this small example will help you understand these operators without too much googling.

## Bonus

Some visualizations for Rx Observables:

[Rx Visualizer](https://rxviz.com/)

[RxJS Marbles](http://rxmarbles.com)
