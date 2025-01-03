# Loader class
_extends \CeremonyCrmApp\Core\Extension_

## Introduction

A class that defines routing, permissions of models and installation of models.

## Routing

The Loader class will allow you to set URLs to your Controllers through you desired routes.
In the `init()` method of a Loader class, the Router class' `httpGet()` method adds URLs to existing URLs from all Loader classes within Ceremony CRM.
For safe and correct routing please check out other Loader classes within Ceremony CRM and their routes.

Creating routes follows this pattern:

```php
  '/new-route' => Controllers\ControllerName::class
```

The string on the left is the URL for the route. The string on the right is the Controller that the route should point to.

### Example of routing with `httpGet()`

```php
public function init(): void
{
  $this->app->router->httpGet([
    '/new-route' => Controllers\ControllerName::class
  ]);
}
```

## Adding links to the sidebar navigation

The Loader class allows you to easily modify the sidebar navigation to add new links to your Views.
You can specify a new link through the sidebar object method `addLink()` in the `init()` method of the Loader class.

### Example of a new sidebar navigation link

```php
$this->app->sidebar->addLink($level, $index, $url, $title, $fontAwesomeIconCssClass, $highlighted);
```

`level` - specifies which of the sidebars will the link appear in

`index` - specifies the order in the array of all the links in the sidebar in the specified level

`url` - URL that was specified in the router

`title` - specifies the text that will be on the button of the link

`fontAwesomeIconCssClass` - specifies the FontAwesome CSS icon class that the button will have.

`highlighted` - specifies, if the button should change it's color.

Note: You need to carefully specify the index of the link. Creating a link on the same index but with different level will result in the new link to overwrite the first link. You need to manage the indexes with structured numbering, for example the level 1 links can use two-digit numbers and the level 2 links can use only three-digit numbers.

When creating the second level of the sidebar navigation, you need to corretly specify for which URL will the second level links be shown to. In the example below, we specify that two links will be availible in the Customer Views.

### Example of adding level 1 and level 2 links

```php
$this->app->sidebar->addLink(1, 40, 'customers/companies', $this->translate('Customers'), 'fas fa-address-card', str_starts_with($this->app->requestedUri, 'customers'));

if (str_starts_with($this->app->requestedUri, 'customers')) { //specifying the url that the second level links can be shown in
  $this->app->sidebar->addHeading1(2, 310, $this->translate('Customers'));
  $this->app->sidebar->addLink(2, 320, 'customers/companies', $this->translate('Companies'), 'fas fa-building');
  $this->app->sidebar->addLink(2, 330, 'customers/persons', $this->translate('Contact Persons'), 'fas fa-users');
}
```

## Installing tables of models

During installation of Ceremony CRM the `installTables()` method will be called for every Loader class in every module. You need to initialize the models of the module and use the `install()` method of the models to properly create the tabels and columns of the models.

### Example of model installation

```php
public function installTables() {
  $mExampleModel = new Models\ExampleModel($this->app);
  $mExampleModel->dropTableIfExists()->install();
}
```

## Creating permissions

Within the Loader class you also can create permissions for your Models and Controllers in the `installDefaultPermissions()` method.
Note that, **if you don't create permissions** only the **Admin** user will be able to use the modules, models or controllers.
Creating permissions for Models follows CRUD methods.
Permissions for both Models and Controllers need to be created as a full path to the Models or Controllers.
You also need to add the full path to the sub-module as a whole.

### Exmaple of permissions creation

```php
public function installDefaultPermissions()
{
  $mPermission = new \CeremonyCrmMod\Settings\Models\Permission($this->app);
  $permissions = [
    "CeremonyCrmApp/Modules/Core/Customers/Models/Activity:Create", //CRUD
    "CeremonyCrmApp/Modules/Core/Customers/Models/Activity:Read", //CRUD
    "CeremonyCrmApp/Modules/Core/Customers/Models/Activity:Update", //CRUD
    "CeremonyCrmApp/Modules/Core/Customers/Models/Activity:Delete", //CRUD
    "CeremonyCrmApp/Modules/Core/Customers/Controllers/Activity" //Controller
    "CeremonyCrmApp/Modules/Core/Customers/Activity" //sub-module as a whole
  ]

  foreach ($permissions as $key => $permission) {
    $mPermission->eloquent->create([
      "permission" => $permission
    ]);
  }
}
```

## Regiter the module

After creating a Loader.php in your module and having created the correct file structure you need to register your module in the app to initialize it during installation. In the App.php use the `addModule()` method to register your module in the app.

```php
  /*rest of the code*/

  $this->addModule(\CeremonyCrmMod\Help\Loader::class);
  $this->addModule(\CeremonyCrmMod\Deals\Loader::class);
  $this->addModule(\CeremonyCrmMod\Leads\Loader::class);

  $this->addModule(\CeremonyCrmMod\YourNewModule\Loader::class);

  /*rest of the code*/
```

## Next Up

- Check out the [Model](model) class
