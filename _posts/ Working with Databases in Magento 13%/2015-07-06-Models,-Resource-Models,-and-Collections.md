---
layout: post
title: Magento Models, resource models, and collections
archive: magento
---

#Models, resource models, and collections

##Describe the basic concepts of models, resource models, and collections, and the relationship they have to one another

Models are where your main business logic should be handled and is a single instance of an object. The model will use the resource model to talk to the database and get/set data for it on `save()` and `load()`.

A resource model is where your main C.R.U.D happens (Create, Read, Update and delete). The resource model shouldn't contain business logic however it will talk to the adapters and basically talk to the database.

Collections are also a resource, however instead of being a single instance of an object collections store an object of objects, you can use a `foreach` to loop through a collection which can be handy. For instance outputting a list of products to a category page.

##Configure a database connection

Your database connection is handled in both <etc>code/etc/local.xml` and <etc>code/etc/config.xml`.

##Describe how Magento works with database tables

Tables are define under the `models  > entities` node in configuration xml, anytime a table is called through your resource you can use `$resource->getTableName('somemodule/sometable');`. This method is in the `Mage_Core_Model_Resource` class. You should use this function incase there are any table prefixes.

##Describe the load-and-save process for a regular entity

###Loading Data.

When working with your `model` you can call a specific entity by calling `Mage::getModel('module/model')->load($id);`. The method which has just been called can be found at: `Mage_Core_Model_Abstract::load()`. This function will clearly not connect and load the object from the database as models don't handle and C.R.U.D:
{% highlight ruby %}
	public function load($id, $field=null) 	{
    $this->_beforeLoad($id, $field); 
    $this->_getResource()->load($this, $id, $field); 
    $this->_afterLoad();
    $this->setOrigData(); 
    $this->_hasDataChanges = false; 
    return $this;
}
{% endhighlight %} 

`$this->_beforeLoad($id, $field);` dispatches 2 events:

- `Mage::dispatchEvent('model_load_before', $params);` - For generic utility
- `Mage::dispatchEvent($this->_eventPrefix.'_load_before', $params);` - For model specific utility.

Next Magento uses the models resource model, defined in the `_init()` method which is set in the `_construct` function to take care of the C.R.U.D actions for us `$this->_getResource()->load($this, $id, $field);`.

Next Magento runs the function `$this->_afterLoad()`. This fires 2 events just like before:

- `Mage::dispatchEvent('model_load_after', array('object'=>$this));` - For generic utility
- `Mage::dispatchEvent($this->_eventPrefix.'_load_after', $this->_getEventData());` For Model specific utility.

Finally Magento sets two more variables on the object which are both used for referencing changes:

- `$this->setOrigData();`
- `$this->_hasDataChanges = false;`

###Saving Data

This method is almost the same as the `$object->load($id)` method with a few additions.  The first thing Magento does is checks to see if `$this->isDeleted()</code is set to true, if it is, it simple deletes the entity.

Next Magento checks `$this->_hasModelChanged()`, if this is false, the module will return. In short this just checks the `$this->_hasDataChanges` mentioned earlier.

Now Magento starts the transaction with the write adapter and sets the `$dataCommited` to false.

Now Magento will enter a `try` method to begin saving the data, like with `load` Magento now calls `$this->_beforeSave();` Which does the following:
{% highlight ruby %}
	if (!$this->getId()) {
		$this->isObjectNew(true);
	}
Mage::dispatchEvent('model_save_before', array('object'=>$this));
Mage::dispatchEvent($this->_eventPrefix.'_save_before', $this->_getEventData());
{% endhighlight %}
Now if the `$this->_dataSaveAllowed` is set to true Magento will save the data using the resource model, directly after this Magento will call `$this->_afterSave();` which does the following:

{% highlight ruby %}
    $this->cleanModelCache();
Mage::dispatchEvent('model_save_after', array('object'=>$this));
Mage::dispatchEvent($this->_eventPrefix.'_save_after', $this->_getEventData());
{% endhighlight ruby %}

##Describe group save operations

Magento uses database transactions when handling group saves to the database to keep the database consistent, it does this by using an atomic operation. 

https://www.youtube.com/watch?v=Y7ulFqYjaT4

##Describe the role of `Zend_Db_Select` in Magento

The `Zend_Db_Select` is the class for SQL SELECT generation and results.

##Describe the collection interface (filtering/sorting/grouping)
{% highlight ruby %}
    $collection->addFieldToFilter('field_name', array('eq', 'some_value'))
$collection->getSelect()->where()
$collection- >setOrder()
$collection- >getSelect()->order()
{% endhighlight %}
##Describe the hierarchy of database-related classes in Magento

Most of Magento's flat resource models extend `Mage_Core_Model_Resource_Db_Abstract` which handles the main C.R.U.D, this model then extends the `Mage_Core_Model_Resource_Abstract` class.

http://excellencemagentoblog.com/question/describe-the-hierarchy-of-database-related-classes-in-magento/

##Describe the role and hierarchy of setup objects in Magento

Setup objects provide ways of easily updating database tables using DDL and adapters. 

These objectives fall into two broad areas. The first is about how models work with resource models and collections in order to access the database storage layer; the second is about how to work with the database directly by using the adapter classes and the `Zend_Db_Select` object to create queries.

###Which methods exist to access the table of a resource model?
{% highlight ruby %}
	$resource->getTable()
$resource- getMainTable()
Mage::getModel('core/resource')->getTableName($modelEntity)
Mage::getModel('core/resource')- >getMainTable($modelEntity)
{% endhighlight %}
###Which methods exist to create joins between tables on collections and on select instances? 
{% highlight ruby %}
	$resource->getSelect()->join()
Zend_Db_Select::join()
Zend_Db_Select::joinInner()
Zend_Db_Select::joinLeft()
Zend_Db_Select::joinRight()
Zend_Db_Select::joinFull()
Zend_Db_Select::joinCross()
Zend_Db_Select::joinNatural()
Mage_Core_Model_Resource_Db_Collection_Abstract::join()
{% endhighlight %}

###How does Magento support different RDBMSs?

Magento uses adapters which means you should never really be writing in a particular database syntax, however this does get overlooked somewhat.

###How do table name lookups work, and what is the purpose of making table names configurable?

Table names are defined in config XML under the `entities` node, you should always use the `Mage::getModel('core/resource')->getTableName($modelEntity)` method to work with tables encase there are table prefixes or some other modifications.

###Which events are fired automatically during CRUD operations? 

	model_load_before
	model_load_after
	mode_save_before
	model_save_after
	model_delete_before
	model_delete_after
	model_delete_commit_after

###How does Magento figure out if a save() call needs to create an INSERT or an UPDATE query?

Magento checks if the object has an id set `$object->getId()`. If it does then Magento updates, otherwise it inserts.

###How many ways exist to specify filters on a flat table collection?
{% highlight ruby %}
	addFieldToFilter()
Zend_Db_Select::where()
{% endhighlight %}
###Which methods exist to influence the ordering of the result set for flat table collections?
{% highlight ruby %}
	setOrder()
Zend_Db_Select::order()
{% endhighlight %}
###How do the methods differ?

`setOrder` appears to loop through the object setting the order where `Zend_Db_Select::order()` adds it to the select statement.

###Why and how does Magento differentiate between setup, read, and write database resources?
{% highlight ruby %}
	Mage::getSingleton('core/resource')->getConnection('core_read')
Mage::getSingleton('core/resource')- >getConnection('core_write')
{% endhighlight %}
This is because Magento supports master/slave configurations for databases.

http://magento.stackexchange.com/questions/7323/why-and-how-does-magento-differentiate-between-setup-read-and-write-database-r
