---
layout: post
title: Identify basic concepts of price generation in Magento
archive: magento
---
#Product Prices

##Identify basic concepts of price generation in Magento

Simple product types use the `Mage_Catalog_Model_Product_Type_Price` model to calculate their prices. However if the product is a bit more complicated Magento extends this class and implements the same methods to calculate the price; product types that do this are configurable, bundled, downloadable and grouped. 

##Modify and adjust price generation for products (for example, during integration of third-party software): 

There is two methods of adjusting the price. Firstly you can just set the price on the product once it has been set, Magento will not recalculate it.

Or the better way to do this would be to add the price using an observer. Magento first an event in `getFinalPrice()` which is `catalog_product_get_final_price`.

###Under what circumstances are product prices read from the index tables?

Magento references the flat tables for prices when using collections.

###From which modules do classes participate in price calculation?

	Mage_Catalog_Model_Product_Type_Price

###Which ways exist to specify custom prices during runtime?

Observers, setting the price on the object.

###How do custom product options influence price calculation?

Custom options add onto the price.

###How are product tier prices implemented and displayed?

`template/catalog/product/view/tierprices.phtml`

###What is the priority of the different prices that can be specified for products (price, special price, group price, tier price, etc.)?

UNANSWERED

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Catalog_Model_Product::getPrice()` and `getFinalPrice()`
- `Mage_Catalog_Model_Product_Type_Price ::getTierPrice()`
- `Mage_Catalog_Model_Product_Indexer_Price`
- `Mage_Catalog_Model_Product_Type_Configurable_Price`
