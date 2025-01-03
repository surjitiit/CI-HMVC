# CodeIgniter 3.1.10 - HMVC
CodeIgniter 3.1.10 with HMVC and Whoops Error Handling Framework

HMVC stands for Hierarchical Model View Controller.

## Features:

All controllers can contain an $autoload class variable, which holds an array of items to load prior to running the constructor. This can be used together with module/config/autoload.php, however using the $autoload variable only works for that specific controller.

```
<?php     
class Xyz extends MX_Controller 
{
    $autoload = array(
        'helper'    => array('url', 'form'),
        'libraries' => array('email'),
    );
}

```

The Modules::$locations array may be set in the application/config.php file. ie:

```
<?php
$config['modules_locations'] = array(
    APPPATH.'modules/' => '../modules/',
);

```

Modules::run() output is buffered, so any data returned or output directly from the controller is caught and returned to the caller. In particular, $this->load->view() can be used as you would in a normal controller, without the need for return.

Controllers can be loaded as class variables of other controllers using $this->load->module('module/controller'); or simply $this->load->module('module'); if the controller name matches the module name.

Any loaded module controller can then be used like a library, ie: $this->controller->method(), but it has access to its own models and libraries independently from the caller.

All module controllers are accessible from the URL via module/controller/method or simply module/method if the module and controller names match. If you add the _remap() method to your controllers you can prevent unwanted access to them from the URL and redirect or flag an error as you like.

## Notes:

To use HMVC functionality, such as Modules::run(), controllers must extend the MX_Controller class.

To use Modular Separation only, without HMVC, controllers will extend the CodeIgniter Controller class.

```
<?php
class Xyz extends MX_Controller 
{
    function __construct()
    {
        parent::__construct();
    }
}

```

Constructors are not required unless you need to load or process something when the controller is first created.

All MY_ extension libraries should include (require) their equivalent MX library file and extend their equivalent MX_ class

Each module may contain a config/routes.php file where routing and a default controller can be defined for that module using:

```
<?php
$route['module_name'] = 'controller_name';

```
Controllers may be loaded from application/controllers sub-directories.

Controllers may also be loaded from module/controllers sub-directories.

Resources may be cross loaded between modules. ie: $this->load->model('module/model');

Modules::run() is designed for returning view partials, and it will return buffered output (a view) from a controller. The syntax for using modules::run is a URI style segmented string and unlimited variables.

```
<?php
/** module and controller names are different, you must include the method name also, including 'index' **/
modules::run('module/controller/method', $params, $...);

/** module and controller names are the same but the method is not 'index' **/
modules::run('module/method', $params, $...);

/** module and controller names are the same and the method is 'index' **/
modules::run('module', $params, $...);

/** Parameters are optional, You may pass any number of parameters. **/

```
To call a module controller from within a controller you can use $this->load->module() or Modules::load() and PHP5 method chaining is available for any object loaded by MX. ie: $this->load->library(‘validation’)->run().

To load languages for modules it is recommended to use the Loader method which will pass the active module name to the Lang instance; ie: $this->load->language('language_file');

The PHP5 spl_autoload feature allows you to freely extend your controllers, models and libraries from application/core or application/libraries base classes without the need to specifically include or require them.

The library loader has also been updated to accommodate some CI 1.7 features: ie Library aliases are accepted in the same fashion as model aliases, and loading config files from the module config directory as library parameters (re: form_validation.php) have beed added.

$config = $this->load->config(‘config_file’), Returns the loaded config array to your variable.

Models and libraries can also be loaded from sub-directories in their respective application directories.

When using form validation with MX you will need to extend the CI_Form_validation class as shown below,

```
<?php
/** application/libraries/MY_Form_validation **/ 
class MY_Form_validation extends CI_Form_validation 
{
    public $CI;
}

```
before assigning the current controller as the $CI variable to the form_validation library. This will allow your callback methods to function properly. (This has been discussed on the CI forums also).

```
<?php
class Xyz extends MX_Controller 
{
    function __construct()
    {
        parent::__construct();

        $this->load->library('form_validation');
        $this->form_validation->CI =& $this;
    }
}

```

## View Partials

Using a Module as a view partial from within a view is as easy as writing:

```
<?php echo Modules::run('module/controller/method', $param, $...); ?>

```
Parameters are optional, You may pass any number of parameters.

# Whoops Error Handling Framework

![Whoops!](http://i.imgur.com/0VQpe96.png)

**Whoops** is an error handler framework for PHP. Out-of-the-box, it provides a pretty
error interface that helps you debug your web projects, but at heart it's a simple yet
powerful stacked error handling system.

**Features**

- Flexible, stack-based error handling
- Stand-alone library with (currently) no required dependencies
- Simple API for dealing with exceptions, trace frames & their data
- Includes a pretty rad error page for your webapp projects
- Includes the ability to [open referenced files directly in your editor and IDE](https://github.com/filp/whoops/blob/master/docs/Open%20Files%20In%20An%20Editor.md)
- Includes handlers for different response formats (JSON, XML, SOAP)
- Easy to extend and integrate with existing libraries
- Clean, well-structured & tested code-base

You can also use pluggable handlers, such as [SOAP handler](https://github.com/whoops-php/soap).

## Server Requirements
- PHP version 5.6 or newer is recommended (We have include a backward function for each() and list() in HMVC, you can use PHP v5.6 or v7.2)

## Source HMVC Modular Extensions
* [Wiredesignz](https://bitbucket.org/wiredesignz/codeigniter-modular-extensions-hmvc)

## Source Whoops Framework
* [Whoops](https://github.com/filp/whoops)
