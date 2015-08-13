---
layout: post
title: Explain how Magento loads and manipulates configuration information
archive: magento
---
#Magento configuration

##Explain how Magento loads and manipulates configuration information

Magento configuration files are a bunch of XML files which are merged together during app initialization into one tree, when not using cache.

Apart from core files such as `app/etc/local.xml` Magento first looks in `app/etc/modules` to find all `*.xml` files, it does this with a simple `glob` command. Because a `glob` command is used you can't just rename your `module.xml` file and expect it to be ignored, you can however change the file extension. The files in `app/etc/modules` are just module initialization files, not configuration files.

Now Magento will search for all module configuration files that are set to active. These configuration files are found in `app/code/{namespace}/{module_name}/etc/config.xml`. Magento configuration files are loaded strictly in alphabetical order, unless you declare a module dependency.

The `config.xml` file will be wrapped in a config node, then typically you have your 4 type config areas.

- admin
- adminhtml
- frontend
- global
{% highlight xml %}
    <config>
    <modules>   
        <!-- Contains module declarations (names, statuses, dependencies) -->
    </modules>
    <global>
        <!-- Contains definitions that should be declared between all scopes -->
    </global>
    <default>
        <!-- Contains definitions that should be in all stores configuration -->
    </default>
    <stores>
        <{store_code}>
            <!-- Contains definitions that should be in the {store_code}'s configuration -->
        </{store_code}>
    </stores>
    </store>
    <frontend>
        <!-- Contains definitions that should be in the frontend -->
    </frontend>
    <adminhtml>
        <!-- Contains definitions that should be in the adminhtml -->
    </adminhtml>
</config>
 {% endhighlight %}
##Describe class group configuration and use in factory methods 

There are 3 types of class groups within a module configuration, these are:

- helpers
- blocks
- models

This is how to define a class group in the `config.xml`.
{% highlight xml %}
<config>
    <{class_group}>
        <{class_name}>
            <class>{class_name}_<{class_group}></class>
        <{class_name}>
    </{class_group}>
</config>
{% endhighlight %}
Because Magento is annoying, when retrieving our class group we don't quite use the same convention.

To get a model:
{% highlight ruby %}
   $model = Mage::getModel('{module_name}/{class_name}');
{% endhighlight %}
This will look for:

    Mage_{module_name}_Model_{class_name}
    Mage/{module_name}/Model/{class_name}.php
    
You can also append things to the end of this class

To get a block:
{% highlight ruby %}
    $block = Mage::app()->getLayout()->createBlock('{module_name}/{class_name}');
{% endhighlight %}
This will look for:

    Mage_{module_name}_Block_{class_name}
    Mage/{module_name}/Block/{class_name}.php

To get a helper:
{% highlight ruby %}
    $helper = Mage::helper('{module_name}');
{% endhighlight %}
This will look for:

    Mage_{module_name}_Helper_Data
    Mage/{module_name}/Helpers/Data.php
    
Or:

To get a helper:
{% highlight ruby %}
    $helper = Mage::helper('{module_name}/{class_name}');
{% endhighlight %}

This will look for:

    Mage_{module_name}_Helper_{class_name}
    Mage/{module_name}/Helpers/{class_name}.php
    
##Describe the process and configuration of class overrides in Magento

You should never change files in the core codepool in Magento, if you do need to rewrite functionality Magento has the capability to do so using configuration XML. The things you can override in Magento are:

- Helpers
- Blocks
- Models
- Resource Models

Here is a module rewrite example:
{% highlight xml %}
    <global>
    <models>
        <{module_name}>
            <rewrite>
                <{class_name}>
                    Some_Module_Model_Some_Class
                </{class_name>
            </rewrite>
        <{module_name}>
    </models>
</global>
{% endhighlight %}
Remember all the configuration XML gets merged into one file, meaning the `{module_name}` reference will become a node. All we are doing in adding our own node into the `rewrite` node within the `{module_name}` node.

When Magento initializes the class it will check to see if the `{class_name}` exists under the `rewrite` node and prioritise that module over the one in the `class` node.

Noding hell.

##Register an Observer
{% highlight xml %}
    <{area}>
        <event>
            <{event_name}>
                <observer>
                    <{observer_name}>
                        <type>{type}</type>
                        <class>{class}</class>
                        <method>{method}</method>
                    <{observer_name}>
                </observer>
            <{event_name}>
        </event>
    </area>
{% endhighlight %}
There are 3 types available:

- Singleton
- Model
- Disabled

The class is the string that is passed into the defined type and method is the method used.
{% highlight ruby %}
    Mage::getModel({class})->{method}
Mage::getSingleton({class})->{method}
{% endhighlight %}
Observers are models and the default namespace for them is Model/Observer. All of your observer methods should go in this one file. Observer methods have to be public get passed the `Varien_Event_Observer` as a parameter. 

##Identify the function and proper use of automatically available events, including *_load_after, etc

Some events automatically dispatched by magento are:
{% highlight ruby %}
    if($this->_eventPrefix && $this->_eventObject) {
    Mage::dispatchEvent($this->_eventPrefix . '_load_before', array(
        $this->_eventObject => $this
    ));
 }

Mage::dispatchEvent($this->_eventPrefix . '_delete_before', 
$this->_getEventData());
{% endhighlight %}    
As you can see the prefix changed on those two event dispatches, here is a full list of the events that will get fired on an object:

- *_load_before
- *_load_after
- *_save_before
- *_save_after
- *_save_after_commit_after
- *_delete_before
- *_delete_after
- *_delete_after_commit_after

This allows models to be intercepted at the define prefix point, IE as the model is being saved, and be modified or used.

##Set up a cron job

The cron should be setup on your server to hit the cron file on Magento which in turn schedules jobs within Magento. Here is how to register a cron job.
{% highlight xml %}
    <crontab>
    <jobs>
        <{cronjob identifier}>
            <schedule>{schedule}</schedule>
            <run>
                <model>{grouped class name}::{method name}</model>
            </run>
        </{cronjob identifier}>
    </jobs>
</crontab>
{% endhighlight %}
Since Magento is an XML configuration-based framework, a thorough understanding of how Magento loads and assembles its configuration XML is a core competency.

##How does the framework discover active modules and their configuration?

Magento glob's the `app/etc/modules/*.xml` folder which tells Magento the module name, codepool, dependencies and if the modle is active.

##What are the common methods with which the framework accesses its configuration values and areas?
{% highlight ruby %}
    Mage::app()->getConfig()->getNode();
Mage::getStoreConfig();
Mage::getStoreConfigFlag();
{% endhighlight %}
##How are per-store configuration values established in the XML DOM?

Within the stores node you use your store code:
{% highlight xml %}
    <stores>
    <{storecode}>
        <!-- default config in here -->
    </{storecode}>
</stores>
{% endhighlight %}
##By what process do the factory methods and autoloader enable class instantiation?

Magento uses factory classes to instantiate other classes using the `Mage::getModel('')` and `Mage::helper('')` methods. Following the namespace conventions means the autoloader knows what directories to look in. For instance `Mage::getModel('sales/order')</code will instantiate the `Mage_Sales_Model_Order`, then the autoloader knows to look in `Mage/Sales/Model/Order.php`.

##Which class types have configured prefixes, and how does this relate to class overrides?

- Helpers
- Models
- Resource Models
- Blocks

Class overrides happen in the configuration XML tree, because the configuration is merged into one XML tree this means you can add your own node to the configured prefix telling Magento to override the original class with your own.

##Which class types and files have explicit paths?

- Helpers
- Models
- Resource Models
- Blocks

##What configuration parameters are available for event observers?

- `<type>disabled|singleton|model</type>`
- `<class></class>`
- `<method></method>`


##What are the interface and configuration options for automatically fired events?

When a model is being initialized Magento fires events during the process which allows us to edit the model at a certain point or add some other additional functionality.

##What is the structure of event observers, and how are properties accessed there in?

Event observers must always be a public function and the paremeter which is passed in will be an instaceof `Varien_Event_Observer`.

The class doesn't need to extend any other classes and the file should `Model/Observer.php` to follow Magento's naming conventions.

##What configuration parameters are available for cron jobs?

- Schedule
- run > module
