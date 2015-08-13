---
layout: post
title: Module Initialization, Magento Certification Exam
archive: magento
---
#Module Initialization

http://blog.belvg.com/magento-certified-developer-exam-module-initialization.html

##Describe the steps needed to create and register a new module

- create `app/etc/modules/Namespace_Module.xml`
{% highlight xml %}
    <config>
    <modules>
        <Namespace_Module>
            <active>true</active>
            <codePool>local</codePool>
            <depends>
                <Mage_Catalog />
            </depends>
        </Namespace_Module>
    </modules>
</config>
{% endhighlight %}
The Magento autoloader globs `app/etc/modules/*.xml` and merges the config into one tree. Once this is done the namepace of the module tells Magento where to look for the configuration XML.

Now you need to create your `config.xml`. This will happen in: `app/code/{codepool}/{namespace}/{module}/etc/config.xml`. This is where you can register your Magento blocks, models, resource models, routers, helpers, layout update XML, crons and setup scripts. 

##Describe the effect of module dependencies

Making one module depend on another means Magento will not try loading its configuration XML before the dependency. This is useful for when your module is adding to another module or altering it.  

##Describe different types of configuration files and the priorities of their loading

###What does "Magento loads modules" mean?

This means to load all the modules declared in `app/etc/modules/*.xml`, then load in the configuration which is defined in these module declarations. 

###In which order are Magento modules loaded?

The first module which is loaded is the `Mage_All` gets called first, next Magento will load all modules with a `Mage_` prefix. 

Magento custom modules are loaded in alphabetical order, however if one module depends on another it will not be loaded until it's dependency has been loaded.

###Which core class loads modules?

The method which loads the modules is `Mage_Core_Model_Config::loadModules()`.

###What are the consequences of one module depending on another module?

Making one module depend on another means Magento will not try loading its configuration XML before the dependency. This is useful for when your module is adding to another module or altering it.

###During the initialization of Magento, when are modules loaded in?

As with most of the initialization in Magento the `_initModules()` is called within the `Mage_Core_Model_App` class. 

###Why is the load order important?

- Core functionality
- Dependencies

The order in which the modules are loaded is important because some configuration may depend on other nodes already being added, for example if you are adding a tab under a section of a pre existing module in `system.xml`. 

###What is the difference regarding module loading between `Mage::run()` and `Mage::app()`?

These code references can be used as an entry point to find answers to the questions above:

- `Mage::run()` and `Mage::app()`
- `Mage_Core_Model_App::run() and init()`
