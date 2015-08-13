---
layout: post
title: EAV Model Concepts Magento Certification Exam
archive: magento
---
#EAV model concepts

##Define basic EAV concepts and class hierarchy

EAV stands for Entity, Attribute and Value. Instead of storing an entity on a per row basis Magento splits each one up into different tables. This makes it far more flexible when adding new attributes to entities or changing an attribute type. 

Attribute's have there own model structure:

- `Backend Model` - This is used for C.R.U.D.
- `Source Model` - This is used for rendering options (labels)
- `Frontend Model` - Used for rendering in the front-end

##Describe the database schema for EAV entities

- `eav_entity` - (E) The **E**ntity table.
- `eav_entity_attribute` (A) The **A**ttrubute table
- `eav_entity_{type}` (V) - The **V**alue table.

The different types are mysql types: datetime, decimals, int, text and varchar.

##Describe the EAV entity structure and its difference from the standard core resource model

All EAV models are stored in `Mage/Eav/Model/*`.

##Describe the EAV load-and-save process and its differences from the regular load and-save process

Magento uses the attributes `source_model` for rendering attributes and the `backend_model` for it's main C.R.U.D actions.

This objective covers understanding how EAV entity values are stored in the database, how the involved tables relate, how the EAV resource models differ from the flat table resource models and how the EAV models process CRUD operations.

###Which classes in `Mage_Eav` are used as resource models and which are used as regular models?

###How do flat and EAV resource models differ?

Flat tables are simple and would store one entity per row, this is much like you'd see in a spreadsheet. EAV separates it's attributes from it's entities, this means multiple entity types can use the same attribute. It makes for good flexibility and easy translations. 

###Which entities in a native Magento installation use EAV resource models and why?

You can check this in the `eav_entity_type` table:

	- customer
	- customer_address
	- catalog_category
	- catalog_product
	- order
	- invoice
	- creditmemo
	- shipment

###What are the advantages and disadvantages of EAV over flat table resource models?

- Advantages
	- Flexibility
	- Translatable
	- Quick implenatation
	- Database schema doesn't change with each model

- Disadvantages
	- Inefficient due to joins
	- No grouping of entity subtypes

###How are store and website scope attribute values implemented in the Magneto EAV system?

Each entity will have a `store_id` field which links back to the `core_store` table. When on a particular store the system will check for an entity associated with the current store then fallback to the global store if nothing is found.

###How does the model distinguish between insert and update operations?

Magento will check if the object already has an id assigned to it; If it does, then Magento will perform an update else and insert.  You can see this code in `Mage_Eav_Model_Entity_Abstract::_processSaveData()`

###How do load and save processes for EAV entities differ from those for flat table entities?

Loads are a bit more complicating in EAV as table joins must be performed, this is why `Mage_Eav_Model_Entity_Abstract` perform our main load actions.

###What parts are identical?

- Both EAV and Flat resource models extend `Mage_Core_Model_Resource_Abstract
`

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Eav_Model_Config`
- `Mage_Eav_Model_Entity_Abstract`
- `Mage_Eav_Model_Entity_Collection_Abstract`
- `Mage_Eav_Model_Entity_Abstract::load()` and `save()`
- `Mage_Core_Model_Abstract::load()` and `save()`
- `Mage_Eav_Model_Entity_Collection_Abstract::load()`
- `Mage_Core_Model_Resource_Db_Collection_Abstract:: load()`
