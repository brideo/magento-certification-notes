---
layout: post
title: Request Routing, Magento Certification Exam
archive: magento
---
#Request Routing

##Describe request routing/request flow in Magento

https://up.kibakoapp.com/QXvgswmXcs

Once the Magento application has instantiated front controller and matched a router in the database it should be able to match the URL to a module controller and action name. This action will instantiate a layout object which is defined in layout XML. The layout XML will define which blocks to use and start rendering content, Blocks refer directly to models for their data and sometimes helpers. 

##Describe how Magento determines which controller to use and how to customize route-to-controller resolution

Magento checks the `core_url_rewrite` database to find a match for the route or a router may be added during the events fired. Magento then references the configuration XML to find a match or the route. The naming convention for this is `{base_url}/{front_name}/{controller_name}/{action_name}/{other_params}`.

Starting with the front controller delegating the process of mapping a request to a controller action, study the steps that occur until a controller action is dispatched.

###Which routers exist in a native Magento implementation?

- admin
    - Process Admin Scope Requests
    - `Mage_Core_Controller_Varien_Router_Admin`
- standard
    - Process Front-end Scope Requests
    - `Mage_Core_Controller_Varien_Router_Standard`
- custom
    - CMS Pages for example
- default
    - Process no-route action, 404 Page
    - `Mage_Core_Controller_Varien_Router_Default`

###How does the standard router map a request to a controller class?

If there is no module specified in the url (in the case of the base URL only) it will use default from config `Mage::getStoreConfig('web/default/front');` which is mapped to a cms page, usually the homepage. You could also access this page by accessing `{base_url}/cms/index/index`.

`{base_url}/{front_name}/{controller_name}/{action_name}/{other_params}`.

###How does the standard router build the filesystem path to a file that might contain a matching action controller?

- `{base_url}/{front_name}/{controller_name}/{action_name}/{other_params}`

###How does Magento process requests that cannot be mapped?

Magento will fall back to the default router class `Mage_Core_Controller_Varien_Router_Default` and throw a 404 error, this is defined in configuration.

###After a matching action controller is found, what steps occur before the action method is executed?

- Module, Controller and Action names are reset to the found module ones. Also SetControllerModule is set to the found module name
- URL params are added to the request
- the request is set to dispatch to true
 
Mage_Core_Controller_Varien_Router_Standard->match(()

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Core_Controller_Varien_Front::init()`
- `Mage_Core_Controller_Varien_Router_Standard::collectRoutes()` and `match()`
