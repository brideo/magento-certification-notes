---
layout: post
title: Checkout Components Magento Certification Exam
archive: magento
---
#Checkout components

##Describe how to modify and effectively customize the quote object, the quote item object, and the address object:

###What is the quote model used for in Magento?

To store information about the customers quote.

###What is the shopping cart model used for in Magento?

It is used to add/remove/edit/manipulate items in the quote table.

###How does Magento store information about the shopping cart?

In tables with a `sales_flat_quote` prefix.

###How are different product types processed when added to the cart?

Simple products are stored in `sales_flat_quote_item` and configurable/custom options are stored in `sales_flat_quote_item_options`.

###What is the difference between shipping and billing address objects in Magento?

There is a row in the `sales_flat_quote_address` table which is called `address_type` which is set to either `shipping` or `billing`.

###How is each used in the quote object?

The shipping address is used to calculate the totals, however if the product type is virtual or downloadable then Magento will use the billing address as the shipping address is redundant with these product types.

###What is the difference in processing quote items for onepage and multishipping checkout in Magento?

Each multi-shipping item is stored to it's own shipping address, this means they are stored in the `sales_flat_quote_address_item` table unlike the standard onepage's `sales_flat_quote_item`.

The multi-shipping checkout also uses a different controller which is `Mage_Checkout_MultishippingController` and a custom model `Mage_Checkout_Model_Type_Mulitishipping`. Apart from that the multi-shipping checkout uses much of the same code as onepage. 

###How does Magento process additional information about products being added to the shopping cart (custom options, components of configurable products, etc.)?

In the `sales_flat_quote_options` table.

###How do products in the shopping cart affect the checkout process?

They can manipulate the totals in certain ways, for instance a promotion could be run on a product which would cause the checkout to add some sort of discount.

###How can the billing and shipping addresses affect the checkout process?

Yet again this can effect the totals, for instance different locations can cost different amount for shipping or tax.

###When exactly does inventory decrementing occur?

When a user adds an item to their cart.

###When exactly does card authorization and capturing occur?

Authorization happens when the order is placed `$order->place()` and if the `$this->capture(null)` method is called.

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Sales_Model_Quote`
- `Mage_Sales_Model_Quote_Address`
- `Mage_Sales_Model_Quote_Item`
- `Mage_Sales_Model_Quote_Address_Item`
- `Mage/CatalogInventory/etc/config.xml`
- `Mage_CatalogInventory_Model_Observer`
 
##Explain the database schema for total models:

###What are total models responsible for in Magento?

Adding nominal fee's on such as shipping, tax, discounts ect.

###How you can customize total models?

You can create your own total by extending the `Mage_Sales_Quote_Address_Total_Abstract` class and registering it in `config.xml`.
{% highlight xml %}
	<config>
	<global>
		<sales>
			<quote>
				<totals>
					<{code}>
						<class>{grouped_class_name}</class>
						<before>{csv_other_totals}</before>
						<after>{csv_other_totals}</after>
						</{code}>
				</totals>
			</quote>
		</sales>
	</global>
</config>
{% endhighlight %}
###How can the individual total models be identified for a given checkout process?

You can see the total models under `Mage_Sales_Quote_Address_Total_{total_name}`. Each total is given a label and and is called by the `collectTotals` method.

###How can the priority of total model execution be customized?

If you look at the XML above you can see the `before` and `after` tag. This doesn't change the sort order of the totals, just the execution order. You can change the sort order in system configuration.

###To which objects do total models have access in Magento, and which objects do they usually manipulate?

- Quote
- Order
- Invoice
- Creditmemo 

###Which class runs total models processing?

`Mage_Sales_Model_Quote_Address_Total_Collector`

###What is the flow of total model execution?

- Nominal
- Subtotal
- Shipping
- Tax
- Grand Total

###At which moment(s) are total models executed?

UNANSWERED.

These code references can be used as an entry point to find answers to the questions above:

- `Mage/Sales/etc/config.xml`
- `Mage/Tax/etc/config.xml`
- `Mage_Sales_Model_Quote_Address`
- `Mage_Sales_Model_Quote_Address_Item`
- `Mage_Sales_Model_Quote_Address_Total_Abstract`
- `Mage_Sales_Model_Quote_Address_Total_Collector`
- `Mage/Sales/Model/Quote/Address/Total/*`
- `Mage/SalesRule/etc/config.xml`
- `Mage_SalesRule_Model_Validator`