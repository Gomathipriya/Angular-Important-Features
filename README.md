# Angular-Important-Features

## Angular

Platform and library for building client applications using HTML and Typescript

### Basic Building Blocks

#### Decorators

* Javascript functions to define what that particular class means and how it should work

#### Modules

* Compilation Context for the components (templates) - Consolidate Components, directives, pipes into cohesive blocks of funtions
* Defines an angular application
* Should have a root module to bootstrap the application
* Can import functionalities from other modules and also allow their functionality to be exported
* Lazy loading - Loading modules on demand, minimize the code loaded during start up
* eg., Reactive form Directive and services \ Router Directive and services
*  Uses @NgModule()
* Helps to define public API 
* Helps in dependency injection configuration
* <strong> Root module </strong> - name (AppModule) - import BrowserModule - helps in bootstrap
* Making modules more readalbe using spread operators
```
export const componentList = [];
export const directiveList = [];
export const pipeList = [];
@NgModule({
  declarations : [
    ...componentList,
    ...directiveList,
    ...pipeList
  ]
})
```
* To make a module publicly available we need to export it
* Injectables received in most of the cases are application wide singleton
* Helps in lazy loading, server side rendering and to split large application into set of feature modules

#### Components

* Defines the View
* Has the data and login mapped to the template (HTML)
* Class with decorator @component

 | Presentational Component | Container Component |
 | -------------------------|-------------------- |
 | How things look | how things work|
 | Have DOM mark up and styles | Dont have DOM markup expect for wrapping div no styles|
 | Sateless | Stateful|
 | Receives data via @Input | All business logic and no UI logic |
 


#### Services

* Provides specific functionalities
* Injected into components as Dependency
* Makes the code module reusable and efficient
* Class with decorator @Injectable
* eg., fetching data from service, validating user input
* Available for any component
* <strong> Injector </strong> - application wide injector during bootstrap to create dependency, maintian container of dependency instances that is reusable if possible
* <strong> Provider </strong> - object that tells injector to obtain or create dependancy
``` 
@Injectable({
  provideIn : 'root'
})  Above is the single shared instance - inject into any class that request
```
* Registering in NgModule  - Available to all the components in that module
* Registering in Component - providers : [] - New instance of service with each new instance of the component  

#### Templates

* HTML that can modifiy data before it display
* <strong>Event Binidng</strong> - () - response to the user input
* <strong>Property Binding</strong> - [] {{}} - Interpolate values into HTML
* Before displaying view - Angular "evalutates Directive" and resolves "binding syntax" in template to modify the DOM
* Angular supports <strong>two way data binding</strong>

#### Pipes

* Improve the user experience by <strong>transforming the value</strong> before displaying
* Can be used when we need to transform data only <strong>in the template</strong>
* eg. Currency, date, lowercase , uppecase, percent
* <code> {{ dateVal | date:'dd/mm/yy' }} </code>

<strong> <u> Async Pipes </u> </strong>

* Uses promise and Observable directly in template wihtout storing the result
* Unwraps a value from an asynchronous primitive
* Allows <strong>subscription</strong> to observables <strong>inside</strong> of the angular <strong>template</strong>
* <strong>Automatic</strong>ally takes care of <strong>unsubscribing</strong> from the observalbes
``` <p> {{ observalbe | async }} </p> ```
* async keyword resolve observable to the current value
* When new value emitted, pipe marks component to be checked for changes
* Default Without Async - subscribe and map data into a local variable manually -> unsubscribing manually (ngOnDestory) -> to avoid memory leak when the component is destroyed
* <strong> TakeUntil </strong> operator - Supports multiple observable per subscription - Takes care of unsubscribing - Doesnt work with (onPush) change detection( due to performance optimization)
```
<pre>

<p *ngIf = "(observable$ | async) > 5" >
  {{ observable$ | async }}
</p>

items$ : observable<number[]>;

<p *ngFor="let item of items | async">
  {{item}}
</p>
  
</pre>
```
* Whenever a new value is emitted, async pipe marks the component to be checked for changes. 
* Invokes transform() on every change detection cycle.
* Async pipe will be destoryed when its container is removed from the DOM or the parent component is destroyed.
* Async pipe optimizes when its parent component is using change detection strategy as onPush

Note: Not to create multiple async pipes for the same observable

<strong> <u> Custom Pipes </u> </strong>
<pre>
import { pipe } from '@angular/core';

@pipe({
   name : 'default'
})

class DefaultPipe() {
  transform(value, fallback) {
    let data = null;
    if(value) {
      data = value;
    }
    else {
      data = fallback;
    }
    return data;
  }
}
</pre>

### Life Cycle

#### ngOnit

* Called after the component is loaded and all the data properties are set
* Called only once when the component is loaded into the DOM
* Use to add something once the component is loaded.
* Subscribe to data from API
* Initialize third party library

#### ngOnChange

* Called before ngOnit
* Called everytime when @Input data changes
* Holds both current and previous value

#### ngDoCheck

* Called everytime when there is a change detection
* Used to trigger change detection manually
* Called immediately after ngOnchange and ngOnInit

#### ngAfterContentInit (after content projection in view completed)

* Called only once after ngDoCheck
* Called whenever Angular projects content into view

#### ngAfterContentChecked (after content projection in view completed)

* Called everytime after ngDoCheck

#### ngAfterViewInit (once view is loaded)

* Called once view and all the other child views are loaded

#### ngAfterViewChecked (once view is loaded)

* Called once after ngAfterViewInit
* Called everytime after ngDoCheck
* Avoid using this for performance reason

#### ngDestory

* Cleans up event (detach) handlers or subscriptions (unsubscribe) to prevent memory leak
* Called just before the component is destroyed

### JIT Vs AOT

| Just In Time Compilation                  | Ahead of Time Compilation                            |
| ----------------------------------------- | ---------------------------------------------------  |
| 1. Development using TS                   | 1. Development using TS                              |
| 2. Compilation with tsc                   | 2. Compilation with ngc                              |
|    Build cmd (ng serve)                   |    Build cmd (ng build -- prod && ng serve --prod)   |
| 3. Bundling                               | 3. Compilation of templates with angular - typescript| 
| 4. Minification                           | 4. Compilation of typescript to javascript           | 
| 5. Deployment                             | 5. Bundling                                          |
| 6. On Browser - Download all JS           | 6. Minification                                      |
| 7. Angular Bootstrap                      | 7. Deployment                                        |
| 8. JIT compilation - generation of JS for each component |  8. On Browser - (download all assets)|
| 9. Application render                     | 9. Angular bootstrap                                 |
|                                           | 10. Application render                               |


### RXJS

* Library - composing async and event based program using Observable sequence

#### Reactive Programming

* async data streams (sequence of ordered events) e.g. button click
* declarative programing paradigm - react to changes via streams
* handling events and data flows
```
|-------------- | ------------- |
| Node JS       | Streams       |
| Unix          | Pipes         |
| Angular       | async pipes   |
|-------------- | ------------- |
```

#### PUSH Vs PULL Protocol

| Protocol    |  Single    | Multiple       | Producer     | Consumer   |
| ----------- | --------   | -------------- |------------- | ---------  |
| PULL        | Function   | Iterator       | Passive (produce data when required) | Active (decide when data is required) |
| PUSH        | Promise    | Observable     | Active (produce data at its own place ) | Passive (reacts to received data ) |

* Protocol - Determines how a data producer can communicate with a data consumer

##### Pull System

* Consumer determines when it receives data from the producer
* Producer unaware of when data will be delivered to the consumer
* e.g. JavaScript Function - code which calls the function consumes by puling out single return value

#### Push System

* Producer determines when to send data to the consumer
* Consumer unaware of when it will receive the data
* e.g. Promise - delivers a resolved value to registered callbacks (Consumers) - incharge of when to push value to callbacks

#### Observable

* Push system JS
* Producer of multiple values pushing them to observers (consumers)
* Functions with zero arguments but generalize those to allow multiple values
* To invoke an observable we need to subscribe to it
* Subscribing to an observable is analogy to calling a function
* Observables able to deliver values either synchronously or async

```
import { observalbe } from 'rxjs';
const foo = new Observable( subscriber => {
  console.log("Hello");
  subscriber.next(42);
  subscriber.next(100);
  setTimeOut(() => {  //async call
    subscriber.next(300);
  },1000);  
});
console.log("before");
foo.subscribe(x => {
  console.log(x);
});
console.log("after");


output
======
"before"
"Hello"
42
100
"after"
300 //async
```

| System         |  Description   |
| ------------   | -------------- |
| Function       | Sync, return single value on invocation |
| Generator      | sync, return 0 to potentially infiniate value on iteration  |
| Promise        | May or maynot return single value |
| Observable     | sync or async return potentially infinite values   |

#### RxJS - Reactive Extension for JavaScript

* Anatomy of Observable
  1. Creating an observable
  2. Subscribing to an observable
  3. Executing an observable
  4. Disposing observable

* creating observable from promise

```
import { from } from 'rxjs';
const data = from (fetch('/api/endpoint'));
data.subscribe({
 next(response) {
   console.log(response);
 }, error (err) {
   console.log("Error "+ err);
 }, complete () {
   console.log("completed"
 }
})
```

* Creating observable form counter

```
import { interval } from 'rxjs';
const secondsCounter = interval(1000);
secondsCounter.subscribe( () => {
  console.log("completed");
});
```

* Creating observable from event

```
import { fromEvent } from 'rxjs';
const e1 = document.getElementById('my-id');
const mouseMove = fromEvent(e1, 'mouseMove');
const subscription = mouseMoves.subscribe( (event: mouseEvent) => {
  console.log("completed");
})
```

* Creating observable from ajax request

```
import { ajax } from 'rxjs/ajax';
const apiData = ajax('/api/data');
apiData.subscribe( res => {
  console.log(res)
});
```

* Router and form modules are observables - listen for and respond to the user input events

#### Operators

* functions that enable manipulation of collection
* takes config options - return fn that take source observable - on exec - operator observer (source observable emitted value) - transform them - return new observable of transformed value
* allows complex async code to be easily composed in a declarative manner.

|   Pipeable               | Creation            |
| -------------------------| --------------------|
| Can be piped to observables | Stand alone functions |
| filter, mergemap         | of                  |
| dont change the existing, returns new obs|        |

```
import { map } from 'rxjs/operators';
const nums = of(1,2,3);
const sqVal = map ((val:number) => val * val);
const sqNum = sqVal(nums);
sqNum.subscribe ( x => console.log(x));

output: 1 4 9
```
 *  Pipe() - link operators together - combine multiple function into single function - runs composed function in sequence
 
 ```
 const squareOdd = of(1,2,3,4,5).pipe(
   filter(n => n%2 != 0),
   map(n => n * n)
 ). subscribe(x => console.log(x));
 ```
 | Area       | Operations                  |
 |------------| ----------------------------|
 | Creation   | from, fromEvent, of         |
 | Combination| combineLatest, concat, merge, startWith, zip    |
 | Filtering  | debounceTime, filter, take, takeUntil           |
 | Transformation | bufferTime, concatMap, map, mergeMap, scan, switchMap |
 | Utility    | tap                         |
 | Multicasting| share                      |
 
 ```
                        |----------------------------------------------------------------------- |
                        |                          Chained   .then()  => Promise                 |
                        |                          pipes()  => Observable                        |
                        |            OBSERVABLE SENDS NOTIFICATION OBSERVER RECIEVES THEM        |
                        |------------------------------------------------------------------------|
```

* CatchError

```
import { ajax } from 'rxjs/ajax';
import { map, CatchError } from 'rxjs/operators'
const apiData = ajax('/api/data').pipe(
 retry(3),
 map(res => {
  if(!res.response) throw new Error ("No Data");
  return res.response;
 }),
 catchError(err => of ([])
);
apiData.subscribe({
  next(x) { console.log(x)},
  error(err) { console.log(err)}
})
```
#### Subscribe

* Observable doesnot maintain list of attached observers
* Each call starts execution and deliver value or events to the observer
* 3 types of values observer execution can deliver are
  1. Next - sends value
  2. Error - Sends JS Error or exception
  3. Complete - doesnot send a value
* In an observable execution, 0 to inifinate next notifications may be delivered.
* If either error or complete notification is delivered, then nothing else can be delivered afterwards
* Best practise is to use try {} catch(e) {}

```
const observable = new Observable(
 function subscribe ( subscriber => {
  subscriber.next(1);
  subscriber.next(2);
  subscriber.complete();
  subscriber.next(3); // not delivered since it violates the contract
 })
)
```

* Observer gets attached to newly created observable execution
* returns an object subscription (ongoing execution)
* subscription.unsubscribe() - cancel ongoing execution

#### Higher Order Observalbe 

* Observable of observable
* ConcatAll() - Subscribe to each inner observable that comes out of outer observable
* ConcatAll() - copies all emitted value unti the observable complete - then move to next one - all values are in that way concatenated
* MergeAll() - subscribe to each inner observable as it arives, then emits each value as it arrives
* SwitchAll() - subscribe to first inner observable then emit each value, but when new inner observable unsubscribe to previous and subscribe to that new inner observable
* exhaust() - subscribe to first inner observable emit each value discarding all new inner observable untill first completes then waits for next inner observable

|  Hot Observable                                      |  Cold Observable                    |
| -----------------------------------------------------| ------------------------------------|
| always broadcast                                     | starts when needed                  |
| doesnt bother if something receives                  | dont start unless subscribed, push value to stream when subscribed |

* To push Data to observable call Next().

##### Adv

* Safety - like observable contract
* Composability with Operators

#### Subject

* Special type of observable
* shares single execution path among observers
* subject delivered to many observers at once
* multicasting

```
--------------------------------------------------------------------------------------------------------------------------------------
|                                              TV - Observer                                                                          |
|                                          TV Station - Subject                                                                       |
|-------------------------------------------------------------------------------------------------------------------------------------|
```

```
import { subject } from 'rxjs/Subject';
const subject = new Subject<string>();
subject.subscribe(value => {})

import { observable } from 'rxjs/observables';
function fn() : observable <number> {
  const subject = new Subject<number> ();
  return subject.asObservable();
```

* <strong> Subject </strong> - No initial value or replay behaviour
* <strong> Async Subject </strong> - Emits latest value to observer upon completion
* <strong> Behaviour Subject </strong> - Req inital value and emits the current value to new subscribers
* <strong> Replay Subject </strong> - Emits specified number of latest emitted values to new subscriber
