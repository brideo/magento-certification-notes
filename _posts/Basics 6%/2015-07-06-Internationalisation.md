---
layout: post
title: Describe how to plan for internationalization of a Magento site
archive: magento
---

#Internationalization

##Describe how to plan for internationalization of a Magento site

Magento is made up of websites, stores and store views. A store view belongs to a store, a store belongs to a website and a website is the top level.

To internationalize a Magento store you will need to have multiple store views, one for each language you would like to have the store translated to.

To add a new store view you can go to `System -> Manage Stores` and click 'Create store view'. The options in here are fairly self explanatory so I will skip over them.
   
Once that is set up you will need to choose the correct *locale* in your system configuration for the appropriate store. For instance if you have set-up a French store, you will want to set it to french.
 
 Now you will need to make sure you have the Magento translation packages, head over to Magento connect where you can download these for free. http://www.magentocommerce.com/magento-connect/customer-experience/internationalization-localization.html?utm_source=mc&utm_medium=web&utm_campaign=translations

##Describe the use of Magento translate classes and translate files

When you are in any template files you can make sure text is translated it by parsing it through `<?php echo $this->__('some text here') ?>`. This actually is using a core helper function `Mage::helper(‘core’)->__('some text here')`.

This helper function then uses the `Mage_Core_Model_Translate` to translate.

###Magento supports 3 types of translations:

####Module Translations

Module translations are stored within `app/code/locale/{languagecode}_{country_code}/{namespace}_{module}.csv. This is where all of your module tranlations using the `Mage::helper('module')->__('some text here');` is tranlasted

####Theme Translations

app/design/{area}/{package}/{theme}/locale/{languagecode}_{country_code}/translate.csv.

####Inline Translations

Inline translations are stored in the `core_translate` table.

##Describe the advantages and disadvantages of using subdomains and subdirectories in internationalization

###Subfolders

####Advanteges

- The page rank of the Top Level Domain (TLD) will be better (better SEO)
- Easier linking to content

####Disadvantages

- SERP limitations
- Local brand recognition
- Hosting on multiple servers can be made difficult

###Subdomains

####Advantages

- Easily host servers in native countries
- Easier to link to pages

####Disadvantages

- Double the SEO efforts

##To verify your understanding of these objectives, ask yourself the following questions:

###Which method is used for translating strings, and on which types of objects is it generally available?

When translating strings within template (`.phtml`) files you should use the method `<?php echo $this->__('Some text here') ?>` to translate which is an alias for `<?php Mage::helper('core')->__('Some text here') ?>`. Within your modules you should use your own custom helper, if you have one, to echo translations.

###In what way does the developer mode influence how Magento handles translations?

Developer mode means only translations related to your module will be used. This is to help test your module translations... surprisingly. 

###How many options exist to add a custom translation for any given string?

3 options exist to translate a given string, these are:

- Inline Translations (database translations)
- Theme Translations (CSV files)
- Module Translations (CSV files)

- What is the priority of translation options?

First Magento will check Inline translations, then theme then it will default to module translations. 

- How are translation conflicts (when two modules translate the same string) processed by Magento?

Magento stores the value in a `key => value` array. To help resolve conflicts Magento will store a module prefix to the array, if it's the first translation for a certain key it will also add the translation without the prefix.
{% highlight ruby %}
    array(
    "AAA" => "BBB",
    "Namespace_Module:AAA" => "BBB"
)
{% endhighlight %}

Now if magento finds another module translation with the same key (AAA) it will not update the main key of "AAA" but it will add an additional item to the array with the new prefix, for example:
 {% highlight ruby %}
    array(
    "AAA" => "BBB",
    "Namespace_Module:AAA" => "BBB",
    "Example_Module:AAA" => "CCC"
)
{% endhighlight %}
Magento will override the main translation of "AAA" with theme and inline translations. 

http://magento.stackexchange.com/a/6725/24087

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Core_Model_Translate::init()`
- `Mage_Core_Model_Locale::emulate()`
