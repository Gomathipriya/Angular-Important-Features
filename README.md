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
* Event Binidng - () - response to the user input
* Property Binding - [] {{}} - Interpolate values into HTML
* Before displaying view - Angular "evalutates Directive" and resolves "binding syntax" in template to modify the DOM
* Angular supports two way data binding

#### Pipes

*



