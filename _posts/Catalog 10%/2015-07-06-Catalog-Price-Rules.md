---
layout: post
title: Magento Certification Catalog Price Rules 
archive: magento
---
#Catalog Price Rules

##Identify how catalog price rules are implemented in Magento:

There are 2 price rules in Magento Product & Cart price rules. 

There is a `Mage_CatalogRule` module which handles the product price rules. Product rules are based on date, catalog and website scope.

###How are the catalog price rules related to the product prices?

When calculating the final price Magento fires an event `catalog_product_get_final_price`. Catalog rule listens to this event and adjusts the price here.

###How are the catalog price rules stored in the database?

with a catalogrule prefix

These code references can be used as an entry point to find answers to the questions above:

- `Mage_CatalogRule_Model_Rule`
- `Mage_CatalogRule_Model_Observer`
- `Mage_CatalogRule_Model_Rule_Product_Price`
