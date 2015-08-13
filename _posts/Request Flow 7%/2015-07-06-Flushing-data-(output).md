---
layout: post
title: Flushing data (output), Magento Certification Exam
archive: magento
---
#Flushing data (output)

##Describe how and when Magento renders content to the browser

https://up.kibakoapp.com/XCNPYn2Jx1

The response is set when the controller calls `$layout->renderLayout()` method. 

Magento recursively loops through its nest of blocks setting a string in the correct order, sets it to the body of the response object which is sent to the browser in the Front Controller. 

##Describe how and when Magento flushes output variables using the Front controller


This objective covers the response object as well as combining JavaScript and CSS files.

###Which events are associated with sending output?

- `controller_front_send_response_before` - This can be used for editing the response data.
- `controller_front_send_response_after` - This can be used for clean up jobs.

###Which class is responsible for sending output?

`Mage_Core_Controller_Varien_Front::dispatch()`

###What are possible issues when this output is not sent to the browser  using the typical output mechanism, but is instead sent to the browser directly?

This can prevent the correct headers being set.

###How are redirects handled?


- `$this->_redirect()` - This performs a HTTP redirect
- `$this->_forward()` - This performs an internal redirect to another controller/action. 



These code references can be used as an entry point to find answers to the questions above:

- `Mage_Core_Controller_Varien_Front::dispatch()`
- `Mage_Core_Controller_Response_Http` and `super classes`
- `Mage_Page_Block_Html_Head::getCssJsHtml()`
