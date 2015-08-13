---
layout: post
title: Access Control Lists (ACL) and permissions in Magento 
archive: magento
---
#Access Control Lists (ACL) and permissions in Magento 

##Define/identify basic terms and elements of ACL

ACL is used to restrict users access pages in the admin, like menu items this is defined in <code>adminhtml.xml</code>. This means the user isn't just forbidden from accessing the controller but the menu item won't display. 

##Use ACL to:

– Set up a menu item 
– Create appropriate permissions for users 
{% highlight xml %}
	<config>
    <!-- This is how to setup a menu in a category -->
    <menu>
        <{top_menu} translate="title" module="{some_helper}">
            <title>Top Menu</title>
            <sort_order>100</sort_order>
            <children>
                <{child_menu} translate="title">
                        <action>adminhtml/some_module/some_action</action>
                        <title>Child Menu</title>
                        <sort_order>1</sort_order>
                </{child_menu>
            </children>
        </{top_menu}>
    </menu>
    <!-- This is how to setup permissions for users -->
    <acl>
        <resources>
            <admin> <!-- admin module -->
                <children>
                    <{top_menu}>
                        <title>Top Menu</title>
                        <sort_order>100</sort_order>
                        <children>
                            <{child_menu} translate="title">
                                <title>Child Menu</title>
                            </{child_menu}>
                        </children>
                    </{top_menu>
                </children>
            </admin>
        </resources>
    </acl>
</config>
{% endhighlight %}
##Check for permissions in permissions management tree structures To verify your understanding, ask yourself these questions: 

###For what purpose is the _isAllowed() method used and which class types implement it?

<code>_isAllowed()</code> is called by the admin controller, it checks whether the user is allowed on the current page.

###What is the XML syntax for adding new menu element?

See above.

###What is adminhtml.xml used for? Which class parses it, and which class applies it?

Adminhtml is used for defining Menu items and user permissions. 

###Where is the code located that processes the ACL XML and where is the code that applies it?

Mage_Admin_Model_Config::loadAclResources() uses the config model to get the aACL

###What is the relationship between Magento and Zend_Acl?

<code>Mage_Admin_Model_Acl</code> extends <code>Zend_Acl</code>. 

###How is ACL information stored in the database?

These code references can be used as an entry point to find answers to the questions above:

- <code>Mage_Admin_Model_Acl</code>
- <code>Mage_Admin_Model_Acl_Resource</code>
- <code>Mage_Admin_Model_Acl_Role</code>
- <code>Mage_Admin_Model_Resource_Acl</code>
- <code>Mage_Admin_Model_Resource_Role</code>
- <code>Mage_Admin_Model_Resource_Roles</code>
- <code>Mage_Admin_Model_Resource_Rules</code>
