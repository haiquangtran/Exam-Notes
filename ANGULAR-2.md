# Angular 2.0
- Angular 2.0 performance upgrade (A lot faster)
- Angular 2.0 uses Typescript (or DART) and meets ECMAScript6 specification.
- Bootstrap is platform specific
- Mobile support
- local variables are defined with # in the view i.e. *ngFor = "#item of items"
- **Components**
  - Component-based programming creates less depdency (more modular) and faster entities.
  - Components are directives with a template
  - Controllers and $scope is no longer used and are replaced by components and directives
  - All components used must be known in the bootstrap process, and have to be imported on the page
- **Directives**
  - Declared with @Directive annotation
  - Can be used within components

## ECMAScript 6
- Classes can be defined
- ECMAScript 6 has full inheritance with super (parameters) to the constructor of the parent
- static variables can be declared

## TypeScript
- From Microsoft
- TypeScript is an extension of ECMAScript, TypeScript = ES6 + Types + Annotations
- Form of JavaScript which knows types and classes and can be compiled to JavaScript. 
- Open source
- Has Inheritance, interfaces, generics and lambdas.

## RxJs
- **Subject**
  - http://reactivex.io/documentation/subject.html
  - Acts as both an observer and an Observable.
  - It can subscribe to 1 or more Observables (since it's an observer)
  - It can also pass through items it observes by reemtting them, and it can also emit new items (Since it's an observable)
  - Since a Subject subscribes to an observable, it will trigger that observable to begin emitting items 
    - If an observable is "cold" (waits for a subscription before it begins to emit items)
    - This can have the effect of making the resulting subject a "hot" observable variant of the original cold observable.
  - There are 4 subjects
    - AsyncSubject
      - Emits the last value (and only the last value) emitted by the source Observable, and only after that source Observable completes 
      - If the source Observable does not emit any values, the AsyncSubject also completes without emitting any values.
    - BehaviourSubject
    - PublishSubject
    - ReplaySubject
- **Observable**
  - An observer subscribes to an Observable
  - That observer reacts to whatever item or sequence of items the Observable emits.
  - This pattern facilitates concurrent operations because it does not need to block while waiting for the Observable to emit objects, but instead it creates a sentry in the form of  an observer that stands ready to react appropriately at whatever future time the Observable does so. 