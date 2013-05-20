# The `src/app` Directory

## Overview

```
src/
  |- app/
  |  |- home/
  |  |- about/
  |  |- app.js
  |  |- app.spec.js
```

The `src/app` directory contains all code specific to this application. Apart
from `app.js` and its accompanying tests (discussed below), this directory is
filled with subdirectories corresponding to high-level sections of the
application, often corresponding to top-level routes. Each directory can have as
many subdirectories as it needs, and the build system will understand what to
do. For example, a top-level route might be "products", which would be a folder
within the `src/app` directory that conceptually corresponds to the top-level
route `/products`, though this is in no way enforced. Products may then have
subdirectories for "create", "view", "search", etc. The "view" submodule may
then define a route of `/products/:id`, ad infinitum.

As `ngCompassBoilerplate` is quite minimal, take a look at the two provided submodules
to gain a better understanding of how these are used as well as to get a
glimpse of how powerful this simple construct can be.

## `app.js`

This is our main app configuration file. It kickstarts the whole process by
requiring all the modules from `src/app` that we need. We must load these now to
ensure the routes are loaded. If as in our "products" example there are
subroutes, we only require the top-level module, and allow the submodules to
require their own submodules.

As a matter of course, we also require the template modules that are generated
during the build.

However, the modules from `src/components` should be required by the app
submodules that need them to ensure proper dependency handling. These are
app-wide dependencies that are required to assemble your app.

```js
angular.module( 'ngCompassBoilerplate', [
  'app-templates',
  'component-templates',
  'ngCompassBoilerplate.home',
  'ngCompassBoilerplate.about'
])
```

With app modules broken down in this way, all routing is performed by the
submodules we include, as that is where our app's functionality is really
defined.  So all we need to do in `app.js` is specify a default route to follow,
which route of course is defined in a submodule. In this case, our `home` module
is where we want to start, which has a defined route for `/home` in
`src/app/home/home.js`.

```js
.config( function ngCompassBoilerplateConfig ( $routeProvider ) {
  $routeProvider.otherwise({ redirectTo: '/home' });
})
```

One of the components included by default is a basic `titleService` that simply
allows you to set the page title from any of your controllers. The service accepts
an optional suffix to be appended to the end of any title set later on, so we set
this now to ensure it runs before our controllers set titles.

```js
.run([ 'titleService', function run ( titleService ) {
  titleService.setSuffix( ' | ngCompassBoilerplate' );
}])
```

And then we define our main application controller. It need not have any logic, 
but this is a good place for logic not specific to the template or route, such as
menu logic or page title wiring.

```js
.controller( 'AppCtrl', [ '$scope', function AppCtrl ( $scope ) {
}])
```

### Testing

One of the design philosophies of `ngCompassBoilerplate` is that tests should exist
alongside the code they test and that the build system should be smart enough to
know the difference and react accordingly. As such, the unit test for `app.js`
is `app.spec.js`, though it is quite minimal.
