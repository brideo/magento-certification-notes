---
layout: post
title: Front Controller, Magento Certification Exam
archive: magento
---
#Front Controller

##Describe the role of the front controller

The front controller has 5 areas of responsibility:

- It gathers routes via its `init()` method
- Applies Database Rewrites such as products, categories and CMS pages.
- Apply configuration URL rewrites (deprecated) 
- Matching a router 
- Sending output

https://up.kibakoapp.com/RM3ITqK8ee

https://up.kibakoapp.com/SV7eG8SnSK

The frontend controller is the area of the application which handles all the logic with request and response.

##Identify uses for events fired in the front controller

`Mage_Core_Controller_Varien_Front` is the frontend controller. 

Using the events specified in this class we can observe and do anything we like with these, such as log 404 pages, add other routers to the system before the default routers, set cookies. 

###Which ways exist in Magento to add router classes?

Magento uses two ways of adding routes, the most commonly used way is through configuration XML.
{% highlight xml %}
    <config>
    <default>
        <web>
            <routers>
                <{name}>
                    <area></area>
                    <class></class>
                </{name}>
            </routers>
        </web>
    </default>
</config>
{% endhighlight %}
The second way, which is the way Magento adds routes for CMS pages is observing an event fired within the `Mage_Core_Controller_Varien_Front::init` method.

###What are the differences between the various ways to add routers?

###Think of possible uses for each of the events fired in the front controller

- `controller_front_init_before` - This is the first event fired, meaning if you would like to override a route with a custom one.
- `controller_front_init_routers` - We can add our own or modify and existing route, this is how the CMS pages routers work.
- `controller_front_send_response_before` - First before the response is dispatched, can be used for modifying the response data. Or setting cookies.
- `controller_front_send_response_after` - Fired after the response is sent out. It is useful for performing any tear down operations after the request has been dealt with.

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Core_Controller_Varien_Front::init()` and `dispatch()`
