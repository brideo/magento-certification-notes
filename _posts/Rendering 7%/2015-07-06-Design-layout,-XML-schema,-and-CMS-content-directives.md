---
layout: post
title: Design layout, XML schema, and CMS content directives
archive: magento
---
#Design layout, XML schema, and CMS content directives

##Describe the elements of Magento's layout XML schema, including the major layout directives:

###How are <update />, <block />, and <action /> used in Magento layout?

- update
	- Include a nodes from another handle
- block
	- Define a block
- action
	- Can call a method within a block

###Which classes and methods determine which nodes from layout XML correspond to certain URLs?

When the {% highlight ruby %}`$this->loadLayout()`{% endhighlight %} is called in Magento, this in turn calls {% highlight ruby %}`Mage_Core_Model_Layout::generateXml()`{% endhighlight %}.

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Core_Model_Layout`
- `Mage_Core_Model_Layout_Update`
- `Mage_Core_Block_Abstract`

##Register layout XML files:

See the question below.

###How can layout XML files be registered for the frontend and adminhtml areas?

This is done in configuration XML `etc/config.xml`
{% highlight xml %}
	<config>
    <{area}>
        <layout>
            <{unique_string}>
                <update>
                    <file>{filename}.xml</file>
                </update>
            </{unique_string}>
        </layout>
    </{area}> 
</config>
{% endhighlight %}
These code references can be used as an entry point to find answers to the questions above:

- `Mage_Core_Model_Layout_Update`

##Create and add code to pages:

###How can code be modified or added to Magento pages using the following methods?

- Template customizations
- Layout customizations
- Overriding block classes
- Registering observers on general block events

##In which circumstances are each of the above methods more or less appropriate than others?

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Core_Controller_Varien_Action`
- `Mage/Core/Block/*`
- `Mage_Core_Block_Abstract`
- `Mage_Core_Block_Template`
- `Mage_Core_Block_Text`
- `Mage_Core_Block_Text_List`
- `Mage_Page_Block_Html_Head`
 
##Explain how variables can be passed to block instances via layout XML:

You can pass variables in layout XML by using Varien Objects magic methods combined with the `action` XML tag:
{% highlight xml %}
	<action method="setSomeVariable">
    <args>
        SomeValue
    <args>
</action>
{% endhighlight %}
###How can variables be passed to the block using the following methods?

- From layout xml file
	- If you see above you can see the Varien's Magic Method allows you to set variables. 
- From controller
	- {% highlight ruby %}`Mage::Regester({key}, {value})`{% endhighlight %}
- From one block to another
- From an arbitrary location (for example, install/upgrade scripts, models)
	- Configuration Value
 
###In which circumstances are each of the above methods more or less appropriate than others? 

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Core_Model_Layout`
- `Mage_Core_Controller_Varien_Action`
- `Mage/Core/Block/*`
- `Mage_Core_Block_Abstract`
- `Mage_Core_Block_Template`
- `Mage_Core_Block_Text`
- `Mage_Core_Block_Text_List`
- `Mage_Page_Block_Html_Head`
 
##Describe various ways to add and customize JavaScript to specific request scopes:

Magento has a bunch of predefined methods in which you can Add javascript to the page. JS libraries are in the root `/js` folder. Skin JS is for theme only Javascript and is usually for customised code, this is useful for making custom overrides. This is stored in `/skin/{area}/{package}/{theme}/js`.

Like when using any method within a block you can use these methods by using the following layout XML.
{% highlight xml %}
	<action method="{method_name}">
    <{descriptive_node}>
        {parameter}
    </descriptive_node>
</action>
{% endhighlight %}

###Which block is responsible for rendering JavaScript in Magento?

The `head` block or `Mage_Page_Block_Html_Head` is responsible for rendering the Javascript.

###Which modes of including JavaScript does Magento support

There are various methods for adding Javascript in Magento, these include:

- `addJs`
- `addJsIe`
- `addItem`
	- ###Params:
	- `js
	- `skin_js 


###Which classes and files should be checked if a link to a custom JavaScript file isn’t being rendered on page?

I would first check the layout XML file which the JS had been added in to check the syntax is correct,  next I would check the file exists where specified.  Now finally if everything is still correct it might be worth stepping through the code in `Mage_Page_Block_Html_Head` to see what is happening; it could be being removed by another module or something.

These code references can be used as an entry point to find answers to the questions above:

- `Mage/Core/Block/*`
- `Mage_Core_Block_Abstract`
- `Mage_Core_Block_Template`
- `Mage_Core_Block_Text`
- `Mage_Core_Block_Text_List`
- `Mage_Page_Block_Html_Head`
