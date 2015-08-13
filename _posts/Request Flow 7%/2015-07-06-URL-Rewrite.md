---
layout: post
title: URL rewrites, Magento Certification Exam
archive: magento
---
#URL rewrites

##Describe URL structure/processing in Magento

Magento allows you to specify the URL key (also known as the URL identifier) or static, content and product category page. This means that you can choose the keyword you want and add it to the particular page's URL independently from the Magento page name.

Magento generally uses the the namespace `{base_url}/{front_name}/{controller}/{action}` for its URL structure. However Magento offers a solution to rewrite these URL's to be a bit more pretty. 

##Describe the URL rewrite process
 
The URL rewrite process happens within the front controller, Magento builds an array of rewrites based from the `core_url_rewrite</code table and maps any rewrites in here. For instance, when you create a pretty URL for a Magento... such as `mobile-phones` this will map to the Magento Category controller: `catalog/category/view/{category_id}.

Next Magento checks the configuration rewrites. 

https://up.kibakoapp.com/NuFGwpAd9W

Focus on the internals of database-based URL rewrites.

###What is the purpose of each of the fields in the core_url_rewrite table?

- Type
- Store
    - This is the store view you would like your rewrite/redirect for.
- ID Path
    - This is a unique identifier, just for personal store use.
- Request Path
    - 
- Target Path
- Options
    - No Redirect, 301 or 302 redirect.
- Description
    - Description is just for you're own notes. 

###When does Magento created the rewrite records for categories and products?

###How and where does Magento find a matching record for the current request?

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Core_Controller_Varien_Front::dispatch()`
- `Mage_Core_Model_Url_Rewrite::rewrite()`
