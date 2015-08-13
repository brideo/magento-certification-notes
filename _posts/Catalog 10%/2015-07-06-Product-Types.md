---
layout: post
title: Identify and describe standard product types (simple, configurable, bundled, etc.)
archive: magento
---
#Product Types

##Identify and describe standard product types (simple, configurable, bundled, etc.). 

There are 6 Product Types

- Simple
	- A single entity.
- Configurable
	- Composite product, group of simple products which have a varying attribute.
	- Example: T-shirt with multiple sizes, each size will be a simple product.
- Grouped
	- Composite Product This is a product where the user can choose multiple options and order multiple products
- Bundled
	- A product which has simple products assigned to it
- Downloadable
	- A downloadable product like a module or PDF
- Virtual
	- No physical product, service charge.

##Create custom product types from scratch or modify existing product types.

http://code.tutsplus.com/tutorials/adding-a-custom-product-type-in-magento--cms-21014

To modify an existing product type you will need to rewrite/extend an existing class:

	Mage_Catalog_Model_Product_Type_{product_type}


To create a new product type, you can either base your new type off a type above or use the abstract class:

	Mage_Catalog_Model_Product_Type_Abstract

You will also need to add you new product type to an XML node, standard procedure with Magento:
{% highlight xml %}
	<config>
	<global>
		<catalog>
			<product>
				<type>
					<{custom_type_name} translate="label" module="{some_helper}">
						<label>Custom Type Label</label>
						<model>instantiation/prefix</model>
						<composite></composite>
						<index_priority></index_priority>
					</{custom_type_name}>
				</type>
			</product>
		</catalog>
	</global>
</config>
{% endhighlight %}
You may need to implement a price module to receive the price, you can do this by adding a `price_model` node.

Identify how custom product types interact with indexing, SQL, and underlying data structures. In addition to allowing customization of existing product types, the framework provided by the Magento catalog module lets you create completely new ones.

###Which product types exist in Magento?

6 product types: simple, bundled, configurable, grouped, downloadable and virtual.

###Which product types are implemented as part of the Mage_Catalog module, and which are not?

The two that are not are:

- `mage_bundled`
- `mage_downloadable`

###What steps need to be taken in order to implement a custom product type?

- Extend a `Mage_Catalog_Model_Product_Type_{type}`
- Define custom XML markup in node

###How do the different product types handle calculation?

UNANSWERED.

###Which indexing processes does the product type influence?

UNANSWERED.

###Which product types implement a parent-child relationship between product entities?

- Bundled
- Configurable
- Grouped

###Which database tables are shared between product types, and which ones are specific to one product type?

The Bundled and Downloadable products have their own tables in the database, easy to remember as they also have their own modules. 

Configurable is specific to one type `catalog_product_super_link` which is the most god fucking awful table none to mankind, FACT.

These code references can be used as an entry point to find answers to the questions above: 

- `Mage_Catalog_Model_Product_Type`
- `Mage_Catalog_Model_Product_Type_Abstract`
- `Mage_Catalog_Model_Product_Type_Simple`
- `Mage_Catalog_Model_Resource_Product_Type_Configurable`
- `Mage_Bundle_Model_Product_Type`
