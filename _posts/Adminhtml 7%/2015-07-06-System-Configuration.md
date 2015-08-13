---
layout: post
title: Magento System Configuration, Certification Notes
archive: magento
---
#System Configuration

##Define the basic terms, elements, and structure of system configuration XML:

- Tabs
	- These are are just used for grouping.
-  Sections
	- Sections refer to tabs to end up under them, however they are at the same level in the XML structure
- Groups
	- Nested under sections
- Fields
	- Nested under group

###How can elements in system configuration be rendered with a custom template?

You would have to create a custom front-end type which is used for rendering and specify it in your `system.xml`.

###How does the structure of `system.xml` relate to the rendered elements in the System Configuration view?

If you look in `Mage_Core_Model_Config` you can see a function `_initSectionsAndTabs` which initialises the `system.xml`. `system.xml` is where the node structure for system XML is stored.

###How can the CSS class of system configuration elements be changed?

You can add a `frontend_class` which can be used to specify the class of a field.

###What is the syntax for specifying the options in dropdowns and multiselects?

You will define a <frontend_type>{type}</frontend_type>. You may also need to specify a source model:
{% highlight xml %}
	<list_mode tranlate="label">
    <label>List Mode</label>
    <frontend_type>select</frontend_type>
    <source_model>adminhtml/system_config_source_catalog_listMode`
    <sort_order>9</sort_order>
    <show_in_default>1</show_in_default>
    <show_in_website>1</show_in_website>
    <show_in_store>1</show_in_store>
</list_mode>
{% endhighlight %}

###Which classes are used to parse and render system configuration XML?

	Mage_Core_Model_Config
	Mage_Adminhtml_Block_System_Config_Form

###What is the syntax to specify a custom renderer for a field in system configuration?

###How does Magento store data for system configuration?

In the table `core_config_data`. You can also set default values in `config.xml`.

###What is the difference between Mage::getStoreConfig(...) and Mage::getConfig()->getNode(...)?

`Mage::getStoreConfig(...) `Handles the store scope for us and is a wrapper for `$path=null, $scope='', $scopeCode=null
`

These code references can be used as an entry point to find answers to the questions above:

- `Mage/Adminhtml/Model/System/Config/*`
- `Mage/Adminhtml/Block/System/Config/*`

##Describe system configuration scopes:

If defined in `system.xml` to display, you can save this

###How do different scopes (global, website, store) work in Magento system configuration?

Store is the most specific and highest priority setting. 

###How does Magento store information about option values and their scopes?
