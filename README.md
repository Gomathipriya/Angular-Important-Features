# Angular-Important-Features

## Angular

Platform and library for building client applications using HTML and Typescript

### Basic Building Blocks

#### Decorators

* Javascript functions to define what that particular class means and how it should work

#### Modules

* Compilation Context for the components
* Defines an angular application
* Should have a root module to bootstrap the application
* Can import functionalities from other modules and also allow their functionality to be exported
* Lazy loading - Loading modules on demand, minimize the code loaded during start up

#### Components

* Defines the View
* Has the data and login mapped to the template (HTML)
* Class with decorator @component

#### Services

* Provides specific functionalities
* Injected into components as Dependency
* Makes the code module reusable and efficient
* Class with decorator @Injectable

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
<code> <p> {{ observalbe | async }} >/p> </code > 
* async keyword resolve observable to the current value


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




