---
layout: post
title: Attributes Management in Magento
archive: magento
---
#Attributes management

##Identify the purpose of attribute frontend, source, and backend models

- `Mage_Eav_Model_Entity_Attribute_Abstract` - **Attribute Model** - Performs the C.R.U.D actions
- `Mage_Eav_Model_Entity_Attribute_Frontend_Abstract` - **Frontend Model** Used for rendering in the front-end
- `Mage_Eav_Model_Entity_Attribute_Backend_Abstract` - **Backend Model** - Used for validating/conversion before C.R.U.D
- `Mage_Eav_Model_Entity_Attribute_Soucre_Absract` - **Source Model** - Used for attribute options and getting labels.

The class names seem long but they are actually fairly easy to remember.

- `Mage_Eav` - Module name
- `Model` - Obviously we are working with Models
- `Entity_Attribute` - Same name as the table. Attributes belong to Entities so this is a fairly standard naming convention
- `{type}` - This is the only variable, just remember there are 3 types of Attribute Models
- `Abstract` - It's the abstract class

##Describe how to implement the interface of attribute frontend, source, and backend models:

You'd extend from the classes above, they have some required methods defined in them.

###How do attribute models, attribute source models, attribute backend models and attribute frontend models relate to each other?

The attribute model is it's own kind of entity. To be able to save it you will need to assign it a backend model so it can validate it's type and save it in the database. Attribute Front-end models render Magento attributes in the front-end, you can create a custom front-end model for any type and assign it to your entity to have it render differently.  

###Which methods have to be implemented in a custom source model (or frontend model or backend model)?

- Frontend Model Required Methods
	- `getValue()`
- Backend Model
	- `getTable()`
	- `isStatic()`
	- `getType()`
	- `getEntityIdField()`
	- `setValueId()`
	- `afterLoad()`
	- `afterSave()`  - also before
	- `afterDelete()` - also before
	- `getEntityValueId($entity)`
	- `getEntityValidId($entity, $valueId)`
- Source Models
	- `getAllOptions()`
	- `getOptionText($value)`


###Can adminhtml system configuration source models also be used for EAV attributes?

No because admin source models use `optionToArray()` where as source models use `getAllOptions`.

###What is the default frontend model (source and backend model) for EAV attributes?

`Mage_Eav_Model_Entity_Attribute_Frontend_Default`

###Does every attribute use a source model?

No only if it needs to be rendered in the front-end. 

###Adding an EAV attribute in an install script
{% highlight ruby %}
	Mage::getResourceModel('eav/entity_setup)->updateAttribute()
Mage::getResourceModel('eav/entity_setup)->addAttribute()
{% endhighlight %}
###Which classes and methods are related to updating the EAV attribute values in the flat catalog tables?

Catalog flat tables are handled by the indexers which can be handled in the methods, the following methods handle updating indexer methods for the catalog:
{% highlight ruby %}
	Mage:getResourceModel('catalog/product_flat_indexer')->updateAttribute();
Mage:getResourceModel('catalog/category_flat')->synchronise()
{% endhighlight %}

###What factors allow for attributes to be added to flat catalog tables?

You can see the method which handles the manageability of these attributes in `Mage::getModel('mage_catalog/product_flat_indexer')->getAttributeCodes()`. The attributes for collections are defined in your config.xml:
{% highlight xml %}
	<config>
		<frontend>
			<collection>
				<attributes>
						<{attribute />
				</attributes>
			</collection>
		</frontend>
	</config>
{% endhighlight %}
###How are store-scoped entity attribute values stored when catalog flat storage is enabled for that entity type?

There is a flat table for each store, for example `catalog_category_flat_store_{id}`

###Which frontend, source, and backend models are available in a stock Magento installation?

You can find all of these models in:
{% highlight ruby %}
	Mage/Eav/Model/Entity/Attribute/{type}
{% endhighlight %}
- `Mage_Eav_Model_Entity_Attribute_Backend_{type}` - **Backend**
	- Abstract
	- Array
	- Datetime
	- Default
	- Incremiment
	- Interface
	- Serialized
	- Store

- `Mage_Eav_Model_Entity_Attribute_Source_{type} - **Source**
	- Abstract
	- Array
	- Boolean
	- Config
	- Interface
	- Store
	- Table

- `Mage_Eav_Model_Entity_Attribute_Frontend_{type} - **Frontend**
	- Abstract
	- Datetime
	- Default
	- Interface

###How do multi-lingual options for attributes work in Magento?

Languages are handled on a store level, because there are multi flat tables for each store each flat table stores the attribute data is stored in the correct language in it's table.

###How do you get a list of all options for an attribute for a specified store view in addition to the admin scope?

You get options for an attribute through a source model. You need to make sure you have set the correct store before using this function.
{% highlight ruby %}
	Mage::getModel('eav/entity_attribute')->load({id})->getSource()->getAllOptions()
{% endhighlight %}
