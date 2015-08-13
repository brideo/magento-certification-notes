---
layout: post
title: Shopping Cart price rules in Magento
archive: magento
---
#Shopping Cart price rules

##Describe how shopping cart price rules work and how they can be customized:

###Which module is responsible for shopping cart price rules?

`Mage_SalesRule`

###What is the difference between shopping cart and catalog price rules?

Shopping cart rules apply to the quote, whereas catalog rules apply to catalog information about the product entities.

###What are the limitations of Magento shopping cart rules?

- Only one voucher code can be applied at once
- You cannot base rules off each other as they work independently of each other
- For admin area orders, shopping cart price rules can be disabled on specific items but they are always applied on all items in the frontend.


These code references can be used as an entry point to find answers to the questions above:

- `Mage/SalesRule/etc/config.xml`
- `Mage/SalesRule/Model/*`
