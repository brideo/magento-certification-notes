---
layout: post
title: Shipping and payment methods in Magento
archive: magento
---
#Shipping and payment methods in Magento

##Describe the programmatic structure of shipping methods, how to customize existing methods, and how to implement new methods

Shipping methods are known as carriers in Magento and the shipping module is called `Mage_Shipping`. This being said it shouldn't be to hard to remember that shipping methods extend `Mage_Shipping_Model_Carrier_Abstract`. You will also need to specify a new carrier in `config.xml`.

##Describe the shipping rates calculation process:

###How does Magento calculate shipping rates?

The abstract class `Mage_Shipping_Model_Carrier_Abstract` provides a method `collectRates($request)` which is used by a concrete carrier to calculate the shipping rate of all items. The request is built up by the quote model based on the shipping location and specified carrier. 

###What is the hierarchy of shipping information in Magento?

UNANSWERED

###How does the TableRate shipping method work?

The TableRate shipping method is a CSV spreadsheet with different rules. These rules can be based on:

- Weight
- Price
- Number of Items

###How do US shipping methods (FedEX, UPS, USPS) work?

These methods are a bit shit, however what they should do it offer third party integration with these companies through API'

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Shipping_Model_Carrier_Abstract`
- `Mage_Shipping_Model_Rate_Abstract`
- `Mage_Shipping_Model_Rate_Request`
- `Mage_Shipping_Model_Rate_Result`
- `Mage/Shipping/Model/Rate/Result/*`
- `Mage_Shipping_Model_Info`
- `Mage/Shipping/Model/Carrier/*`
- `Mage/Shipping/Model/Resource/Carrier/*`
 
##Describe the programmatic structure of payment methods and how to implement new methods:

###How can payment method behavior be customized (for example: whether to charge or authorize a credit card; controlling URL redirects; etc.)?

The module for Payment Methods is `Mage_Payment`. So to create a new payment method you will need to extend `Mage_Payment_Model_Method_Abstract`. Also you will need to add some information into config.xml.

###Which class is the basic class in the payment method hierarchy?

UNANSWERED

###How can the stored data of payment methods be customized (credit card numbers, for example)?

Payment information is imported in the `Mage_Sales_Model_Quote_Payment::importData($data)` method. This method fires an event you can listen to `$this->_eventPrefix . '_import_data_before'` so you are best off modifying/logging data in here.

###What is the difference between payment method and payment classes (such as order_payment, quote_payment, etc.)?

Payment methods process the transaction/card authorisation while payment classes manage the payment methods themselves. 

###What is the typical structure of the payment method module?

Payment method modules usually have a few blocks for displaying the forms and a controller to handle request to third party gateways. 

###How do payment modules support billing agreements? 

Billing agreements are out of the box with Magento, you can set a billing agreement by calling `$order->getPayment()->setBillingAgreementData($data);`.

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Payment_Model_Method_Abstract`
- `Mage_Payment_Model_Method_Cc`
- `Mage_Payment_Model_Info`
- `Mage/Paypal/*`
- `Mage/PaypalUk/*`
