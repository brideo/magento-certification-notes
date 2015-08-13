---
layout: post
title: Common structure/architecture in Magento, Certification Notes
archive: magento
---
#Common structure/architecture

##Describe the similarities and differences between adminhtml and frontend interface and routing

###Which areas in configuration are only loaded for the admin area?

All admin routers are under the `admin` node in config, a great example of this is in local XML where you set your frontName for the admin login page:
{% highlight xml %}
	<config>
    <routers>
        <adminhtml>
            <args>
                <modules>
                    <foo_bar before="Mage_Adminhtml">Foo_Bar_AdminHtml</foo_bar>
                </modules>
            </args>	
        </adminhtml>
    </routers>
</admin>
{% endhighlight %}
###What is the difference between admin and frontend controllers?

Admin controllers extend a different class:

- `Mage_Adminhtml_Controller_Action` - Admin controller
- `Mage_Core_Controller_Front_Action` - Frontend Controller

Adminhtml is it's own magento module because Magento used to handle it's shit like that. It's best practise to contain yours within your module though. The convention is to have the class type then an Adminhtml folder.

###When does Magento figure out which area to use on the current page?

In the method `Mage_Core_Controller_Varien_Front::dispatch()`, this method loops through each Magento module checking to see if any of them have registered the current front name, because we define our front name in `app/etc/local.xml` like mentioned earlier Magento knows to only use controllers in the adminhtml module.

###How you can make your controller work under the /admin route?

See code example above.

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Adminhtml_Controller_Action`
- `Mage_Core_Model_Layout`
- `Mage_Core_Model_Layout_Update`
- `Mage_Core_Controller_Varien_Router_Standard`
 

##Describe the components and types of cache clearing using the adminhtml interface:

###At which moment does Magento check if a user is logged in or not?

The `Mage_Admin_Model_Observer` listens for the `predispatch` event fired by the admin controller action. Here you can see the logic to see if the user is logged in.

If there is not a user logged in `(!$user || !$user->getId())`, if theres not it will check to see if theres post data, if so it will log it will attempt to log the user in, otherwise it will redirect you to the login page.

Next it clears the ACL permissions.

###Which class do most Magento adminhtml blocks extend?

	Mage_Adminthml_Block_Template

###What are the roles of adminhtml config?

`adminhtml.xml` is used for setting up your ACL and menu permissions.
{% highlight xml %}
	<config>
    <menu>
        <{some_module}>
            <children>
                <{some_node} translate="title" module="{some_module}">
                    <title>Some Title</title>
                    <sort_order>60</sort_order>
                    <children></children>
                </{some_node}
            </children>
        </{some_module}>
    </menu>
</config>
{% endhighlight %}
###What are the differences between the different cache types on the admin cache cleaning page?

- config
- blocks
- api

###What is the difference between “flush storage” and “flush Magento cache”?

Flush Storage calls the `Zend_Cache_Core::clean()` with no params, this means the whole cache will be flushed.

###How you can clear the cache without using the UI?
{% highlight ruby %}
	n98-magerun.phar cache:flush (trolling)
Mage_Core_Model_Cache::flush
Mage_Core_Model_Cache::clean
Zend_Cache_Core::clean()
{% endhighlight %}
These code references can be used as an entry point to find answers to the questions above:

###Mage_Adminhtml_Model_Config

###Mage_Adminhtml_Model_Config_Data

###Mage_Admin_Model_Observer

###Mage_Core_Model_Cache
