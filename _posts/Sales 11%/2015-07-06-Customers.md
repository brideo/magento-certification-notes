---
layout: post
title: Customers Magento Certification Exam
archive: magento
---
#Customer

##Describe the architecture of the customer module

The `Mage_Customer` model is split up into different sections for handling customer entity data. 

- Address
- Customer
- Group
- Attribute

##Describe the role of customer addresses

Customer addresses mean customers can save their billing addresses so they don't have to enter them everytime they checkout. The customer addresses entities are EAV just like the customer entity themselves meaning you can easily add attributes to them.

##Describe how to add, modify, and display customer attributes:

To add attributes you should to use and install script. Like with catalog entities you should use the `Mage::getModel('customer/entity_setup')->addAttribute()` method to add a new attribute.

Great tutorial here:

http://excellencemagentoblog.com/blog/2011/10/02/customer-registration-fields-magento1-6/

###What is the structure of tables in which customer information is stored?

The customers entiteis are stored in `customer_entity_{attribute_type}`.

###What is the customer resource model?

	Mage_Customer_Model_Resource_Customer

###How is customer information validated?

	Mage_Customer_Model_Resource_Customer::validate()

###How can customer-related email templates be manipulated?

You can creating customer emails in the admin section of magento: `System` > `Transactional Emails`.  You can then assign the email template to a certain transaction type in `System` > `Configuration` > `Sales Emails`.

###What is the difference between shipping and billing addresses for a customer?

There is no difference until they select one on the checkout.

###How does the number of shipping and billing address entities affect the frontend interface for customers? 

There is a select box on checkout which displays these addresses. 

###How does customer information affect prices and taxes?

Customer groups can set different promotions and tax rules.

###How can attributes be added to a customer address?

The same as they are added to customers, see above.

###How are custom address attributes you added converted to an order address?

Using the `sales_convert_quote_item` node under `fieldsets` in config XML.

http://www.atwix.com/magento/custom-product-attribute-quote-order-item

###Can a customer be added to two customer groups at the same time?

No.

These code references can be used as an entry point to find answers to the questions above: 

- `Mage/Customer/etc/config.xml`
- `Mage_Customer_Model_Customer`
- `Mage_Customer_Model_Resource_Customer`
- `Mage_Customer_Model_Customer_Address`
