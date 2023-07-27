## Create Anuglar Component

Folder : [wwwroot/js/components]

#### componentName.component.js

```js
(function () {
  "use strict";

  angular.module("synicApp").component("componentName", {
    templateUrl: "/js/components/componentName.html",
    bindings: {
      variable1: "<", // one way binding
      variable2: "=", // two way binidng
    },
    controller: componentNameController,
  });

  componentNameController.$inject = [];
  function componentNameController(
    SocialMediaLinksService,
    constants,
    UtilsService,
    $scope
  ) {
    var $ctrl = this;
  }
})();
```

## componentName.html

```html
<div>{{$ctrl.varaible1}}</div>
```

## When you use it

```html
<component-name variable1="test" variable2="test2"></component-name>
```

## POST Request

```js
$scope.submitFunction = function () {
  $http({
    method: "POST",
    url: "/api/route",
    headers: {
      "Content-Type": "application/json",
    },
    data,
  })
    .then((res) => {
      console.log("RESULT: ", res.data);
    })
    .catch((err) => {
      console.log("ERROR: ", err);
    });
};
```
