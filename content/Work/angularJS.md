+++
title = "Angular JS"
weight = 2
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Angular JS

#### Data Binding

`Data-binding is an automatic way of updating` the view whenever the model changes, as well as updating the model whenever the view changes. This is awesome because it eliminates DOM manipulation from the list of things you have to worry about.

ng-if
ng-switch
ng-model
ng-show
ng-hide

$scope
$parent.name

#### Controller

Controllers are the behavior behind the DOM elements. AngularJS lets you express the behavior in a clean readable form without the usual boilerplate of updating the DOM, registering callbacks or watching model changes.

#### Plain JavaScript

Unlike other frameworks, there is no need to inherit from proprietary types in order to wrap the model in accessors methods. AngularJS models are plain old JavaScript objects. This makes your code easy to test, maintain, reuse, and again free from boilerplate.

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

#### componentName.html

```html
<div>{{$ctrl.varaible1}}</div>
```

#### When you use it

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

#### Sending single item

```js
...
data: variable
...

```

```c#
public dynamic UpdateFullfilledStatus([FromBody] int salesOrderId)
```

## Modal

https://angular-ui.github.io/bootstrap/#!#modal

### 1. Markup

```html
<script type="text/ng-template" id="angularModal.html">
  <div class="modal-header">
      <h3 class="modal-title" id="modal-title">Modal Title</h3>
  </div>

  <div class="modal-body" id="modal-body">
    ...
  </div>

  <div class="modal-footer">
      <button class="btn btn-primary" type="button" ng-click="ok()">OK</button>
      <button class="btn btn-warning" type="button" ng-click="cancel()">Cancel</button>
  </div>
</script>

<button type="button" class="btn btn-default" ng-click="testOpen()">
  Open me!
</button>
```

### 2. Javascript

> #### Import scripts

```js

```

> #### Modal click event function

```js
// ng-click=openModal() from HTML
$scope.openModal = function () {
  var modalInstance = $uibModal.open({
    templateUrl: "addDiscountContent.html",
    controller: "AddDiscountModalCtrl",
    resolve: {
      // Properties inside resolve goes to Modal Controller.
      items: function () {
        return "haha";
      },
    },
  });

  // Return event when closing modal
  modalInstance.result.then(
    function (selectedItem) {
      // 'Ok' clicked
      console.log("modal click ok : " + selectedItem);
    },
    function () {
      // 'Cancel' clicked
      console.log("modal에서 dismissed at: " + new Date());
    }
  );
};
```

> #### Controller

```js
angular
  .module("synicApp")
  .controller(
    "AddDiscountModalCtrl",
    function ($scope, $uibModalInstance, items) {
      console.log("ITEM: ", items);

      // When click Ok button
      $scope.ok = function () {
        $uibModalInstance.close("RETURN HOHO");
      };

      // When click Cancel button
      $scope.cancel = function () {
        $uibModalInstance.dismiss("cancel");
      };
    }
  );
```

### 3. CSS

### Angular structure

Service - complex logics (do not get confused with IServices)
Controller - set of functions/ services

## RESTful

// needs validation of unique options
POST (add)
/api/cart/{cartid}/items

// no need to validate unique options
PUT (edit item)
/api/cart/{cartid}/items/{itemId}

-- just updating the qty
