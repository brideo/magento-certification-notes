---
layout: post
title: Design and layout initialization, Magento Certification Exam
archive: magento
---
#Design and layout initialization

Layout XML file is defined in a modules configuration, you can also define any changes in file called `local.xml` which is automatically loaded by Magento. Templates are linked to blocks and controllers via the layout XML and layout handles. This is the same as most MVC structures with the exception of the block, which holds a bunch of methods used for connecting the view to the model. The methods can be accessed using `$this`.

`Layout XML` -> `Block` -> `View`

##Identify the steps in the request flow in which:

###Design data is populated

Data is populated by defining blocks in layout XML.

###Layout configuration files are parsed

As with everything, you define your layout XML files in your `etc/config.xml` with the following code:
{% highlight xml %}
    <{area}>
    <layout>
        <update>
            <file>{file_name}</file>
        </update>
    </layout>
</{area}>
{% endhighlight %}
###Layout is compiled

When the controller calls `$this->loadLayout()` it in turn calls `parent::loadLayout($ids, $generateBlocks, $generateXml);`.

This parent method is with the `Mage_Core_Controller_Varien_Action</code class. This method takes care of adding any additional layout handles. It calls `$this->loadLayoutUpdates()`.

Unless the specified otherwise with a parameter in the original controller `$this->loadLayout()` call Magento will next begin to general the Layout XML. It does this within <code$this->generateLayoutXml`.

Finally, again if no param is specified, Magento will generate the layout blocks with the function `$this->generateLayoutBlocks()`.

###Output is rendered

You generally would render your layout in your controller as this makes sense. When in your controller you can call `$this->loadLayout()->renderLayout()`. 

###Side note:

You can use `$this` within your controller because all 3 of the `Mage_Core_Controller_Varien_Router_{type}` controllers extend from the `Mage_Core_Controller_Varien_Action` which has this method stored in. 

The design configuration is part of Magento's view implementation. This objective covers the processing of these XML instructions.

###Which ways exist to specify the layout update handles that will be processed during a request?

From anywhere (probably an Observer):
{% highlight ruby %}
    <?php Mage::getLayout()->getUpdate()->addHandle('new_handle'); ?>
{% endhighlight %}
From a controller:
{% highlight ruby %}
	<?php $this->loadLayout('new_handle'); ?>
{% endhighlight %}
###Which classes are responsible for the layout being loaded?

`Mage_Core_Model_Layout_Update`

###How are layout xml directives processed?

`Mage_Core_Model_Layout`

###Which configuration adds a file containing layout xml updates to a module?

`etc/config.xml`

###Why is the load order of layout xml files important?

The order of the layout XML files are important because you can override blocks with your own methods or set default fall backs.

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Core_Controller_Varien_Action::loadLayout()`
- `Mage_Core_Model_Layout::__construct()`
- `Mage_Core_Model_Layout_Update::load()`
