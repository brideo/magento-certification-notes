---
layout: post
title: Magento Catalog Certification Category Structure 
archive: magento
---

#Category Structure

##Describe the Category Hierarchy Tree Structure implementation (the internal structure inside the database), including:

###The meaning of parent_id 0

The parent_id 0 category is the root category, IE the god father category. 

###The construction of paths

The path column can be seen in `catalog_category_entity` and are slash separated Ids.

###How is the category hierarchy reflected in the database?

The slashed separated IDS represent the nesting of the categories

###Does it differ when multiple root categories are present?

Theres more than one slash.

###How is a catalog tree read from the database tables, with and without flat catalog tables enabled?

- `Mage_Catalog_Model_Resource_Category_Tree` If flat data is disabled
- `Mage_Catalog_Model_Resource_Category_Flat`  If flat data is enabled

###How does working with categories differ if the flat catalog is enabled on a model level?

###How is the category parent id path set on new categories?

The method `Mage_Catalog_Model_Resource_Category::_beforeSave()` gets the objects path, explodes it by the `/` seperator and then sets the arrays key to the size of the array - 1.
{% highlight ruby %}
	$path  = explode('/', $object->getPath()); 
$level = count($path); 
$object->setLevel($level); 
if ($level) {
     	$object->setParentId($path[$level - 1]); 
}
{% endhighlight %}

###Which methods exist to read category children and how do they differ?

- `Mage_Catalog_Model_Resource_Category::getChildren($category)` - Returns comma separated ID's
- `Mage_Catalog_Model_Resource_Category::getAllChildren($category)` - Returns an array of ID's
- `Mage_Catalog_Model_Resource_Category::_getChildrenCategoriesBase($category)` - Returns a loaded collection

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Catalog_Model_Category`
- `Mage_Catalog_Model_Resource_Category`
- `Mage_Catalog_Model_Resource_Category_Collection`
- `Mage_Catalog_Model_Resource_Category_Tree`
