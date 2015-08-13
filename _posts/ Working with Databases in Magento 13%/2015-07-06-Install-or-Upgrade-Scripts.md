---
layout: post
title: Magento Certification Notes Install/upgrade scripts 
archive: magento
---

#Install/upgrade scripts

I feel fairly confident with this section so aren't going to waste time on it.

Feel free to submit a pull request to my GitHub if you have some notes you'd like to add here. 

##Describe the install/upgrade workflow

Magento compares the version number in `config.xml` against the version number stored in the `core_resource` table. If the versions don't match Magento will run the install scripts with the correct prefixes.

##Write install and upgrade scripts using set-up resources

##Identify how to use the DDL class in setup scripts

Each module can encapsulate the preparation and upgrade of the database table it requires via setup scripts.

###Under which circumstances are setup scripts executed? 
###What is the difference between the different classes used to execute setup scripts?
###Which is the base setup class for flat table entities, and which one the base for EAV entities?
###Which methods are generally available in setup scripts to manipulate database tables and indexes?
###What is the difference between addAttribute() and updateAttribute() in EAV setup scripts?
###How can you implement a rollback in Magento?

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Core_Model_App::run()` and `_initModules()`
- `Mage_Core_Model_Resource_Setup::applyAllUpdates()` and `applyAllDataUpdates()`
- `Mage_Eav_Model_Entity_Setup::addAttribute()` and `updateAttribute()`
