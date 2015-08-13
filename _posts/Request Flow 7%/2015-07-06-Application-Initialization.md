---
layout: post
title: Application initialization, Magento Certification Exam
archive: magento
---
#Application initialization

##Describe the steps for application initialization

- `index.php` - Calls `run('', 'store');`
- `app/Mage.php` - Calls `Mage_Core_Model_App::run('','store,array())`
- `Mage_Core_Model_App::run('','store,array())` is the heart of the Magento app.

￼￼https://up.kibakoapp.com/mDzNy3out8

First Magento includes the config file which can be found in `includes/config.php`. If compiler is enabled in `System > Tools > Compilation`, this will change loading paths.

Next Magento checks to see if `app/Mage.php` exists, if it doesn't it will redirect you to the downloader page. If it doesn't exist it will tell you the file doesn't exist.

Next Magento will check if the `maintenance.flag` exists, if it will display to Magento's 503 page kept in `errors/503.php`.
 
Should the last 2 steps not take place, Magento will now include `app/Mage.php`. This is where the Magento shabang goodness is stored.

###Within Mage.php

At the top of `app/Mage.php`, remember Magento included it earlier a few functions are run. If Compilation mode is not enabled Magento will include default paths, load all code pools, include `app/code/{code_pool}/Mage/functions.php` and include `Varien/Autoload.php`. It will then call the method `Varien_Autoload::register();`.

Finally Magento will run the autoloader which includes all classes in `index.php`.

Next Magento checks if the developer mode is set, if it is it sets `Mage::setIsDeveloperMode(true)`.

Now 2 variables are set, `$mageRunCode` which should be set to a store code and `$mageRunType` which specifies whether you run a store or a website. These variables are for multi store setup so will change dependant on what store your running. If you are running a sub domain these can be set in your .conf or htaccess files. However if you're using a subfolder setup you will need to create a `{store_code}/index.php` folder and set the variables in there.

##Describe the role of the system entrypoint, index.php

The reason Magento sets these variables mention above is because it passes them into the next function ran, which in `Mage::run($mageRunCode, $mageRunType);`. This right here is the main system entry point.

The first thing `run()` does is instantiate the `Mage_Core_Model_App` class and set it to the protected variable `$_app`. This is what is returned with the `Mage::app();` function.

Now that is instantiated Magento runs `Mage_Core_Model_App::run()`. This is the following functions are run:

- `baseInit($options)` - Load the base config and initialise cache.
- `$this->_initModules();` - Load module configuration.
- `$this->_initCurrentStore($scopeCode, $scopeType);` - Initialise the current store using the store code passed in. 
- `Mage_Core_Model_Resource_Setup::applyAllDataUpdates();` - Run all setup scripts

###How and when is the include path set up and the auto loader registered?

When Magento is happy it can include `app/Mage.php` into `index.php` requires it. Mage.php will include the `varien/Autoload.php` file which contains the `Varien_Autoload` class. Now that is included Mage.php runs the `Varien_Autoload::register();` method which registers the autoloader. 

###How and when does Magento load the base configuration, the module configuration, and the database configuration?

`index.php` Will run `Mage::run()` which in turn will instantiate the `Mage_Core_Model_App` class and run the `Mage_Core_Model_App::run()` method. This is where the main config is loaded.

The function `$this->baseInit()` function loads the `Mage_Core_Model_Config::loadBase()`

###How and when are the two main types of setup script executed?

The two kind of setup scripts are:

- Sql
- Data


The setup scripts are also run in `Mage_Core_Model_App::run()`, this calls `Mage_Core_Model_Resource_Setup::applyAllDataUpdates();` which references all modules config version number, then checks the version number against the one in the `core_resource` table to see if the script has already been run... if it hasn't it will be executed.

###When does Magento decide which store view to use, and when is the current locale set?

`Mage_Core_Model_App::run` - `$this->_initCurrentStore($scopeCode, $scopeType);`

###Which ways exist in Magento to specify the current store view?

You can set the `$_SERVER` variables either in `index.php` or your environment variables in `.conf` or `.htaccess`, these then get passed to `Mage::run()` or `Mage::app()`.

###When are the request and response objects initialized?

`Mage_Core_Model_App::run()` - `$this->_initRequest();`

These code references can be used as an entry point to find answers to the questions above:

- `index.php`
- `Mage_Core_Model_App::run()`
- `Mage_Core_Model_Config::loadBase() and init()`
