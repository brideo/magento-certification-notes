---
layout: post
title: Magento Catalog Certification Other Skills
archive: magento
---

#Other Skills

##Choose optimal catalog structure (EAV vs. Flat) for a given implementation

The main goal is to keep Magento as extendable as possible while minimising the amount of operations to display a entity to the user. With this in mind enabling flat storage for products is recommended as then Magento doesn't have to perform as many joins.

##Implement, troubleshoot, and modify Magento tax rules

UNANSWERED

##Modify, extend, and troubleshoot the Magento layered (“filter”) navigation

Filters  all extend from `Mage_Catalog_Model_Layer_Filter_Abstract`' meaning if a filter is broken its best off looking in here.

To create your own filter 

##Troubleshoot and customize Magento indexes

http://www.slideshare.net/ivanchepurnyi/magento-indexes

- Index Model 
	- `Mage_Index_Model_Indexer_Abstract`
- Catalog Indexers
	- `Mage_Catalog_Model_Product_indexer_Eav`
	- `Mage_Catalog_Model_Product_Indexer_Flat`
	- `Mage_Catalog_Model_Product_indexer_Flat`
- Inventory Index
	- `Mage_CatalogIndex_Model_Indexer_Stock`

To add your own indexer you will need to add a node the configuration tree in `etc/config.xml` within your module.
{% highlight xml %}
	<config>
		<global>
			<index>
				<indexer>
					<{namespace_module}>
						<module>{namespace_module/indexer}</module>
					</{namespace_module}>
				</indexer>
			</index>
		</global>
	</config>
{% endhighlight %}
You can call your indexer class what you would like, however the convention is to use the `indexer` suffix.

There are 3 required methods in your class to get your indexer to work:

- `getName` 
- `_registerEvent`
- `_processEvent` 

##Describe custom product options in Magento

Custom options are similar to configurable products however they aren't their own entity. You can see the custom option module in `Mage_Catalog_Model_Product_Option`.

The Magento catalog module doesn’t only consist of categories and products: a lot of additional catalog functionality is implemented, partly within the Mage_Catalog module, partly in other modules.

Custom options are saved in tables with a `catalog_product_option` prefix.

###When and how are the catalog flat tables updated when a product is modified?

When the "Product Flat Data" index is run or `Mage_Catalog_Model_Product_Indexer_Flat`.

###Which factors are used by the Mage_Tax module to apply the correct tax rate (or rates) to a product price?

- Customer Group
- Destination
- Catalog Prices Configuration (Including/Excluding VAT)
- Product Tax Class
- Store Scope Settings

###How can attributes with custom source models be integrated into layered navigation filtering?

To add an attribute with a custom source model you will need to create your own filter class which extends `Mage_Catalog_Model_Layer_Filter_Abstract`.

###Which classes are responsible for rendering the layered navigation?

See above

###Which indexes are used for the layered navigation?

The "Product Attributes" index which can be found 

###Which steps are needed to integrate a custom indexer into the framework offered by the Mage_Index module?

###How are custom product options stored on quote and order items?

###How can you specify custom product options on-the-fly on quote items?

###How are custom product options copied from quote to order items?

###How are custom product options processed when a product is added to the cart?
