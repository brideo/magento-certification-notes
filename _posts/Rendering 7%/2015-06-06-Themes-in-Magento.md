---
layout: post
title: Themes in Magento, Certification Exam Notes
archive: magento
---
#Themes in Magento

##Define and describe the use of themes in Magento:

###How you can use themes to customize core functionality?

In Magento you have two areas for themes:

- `skin/{area}/{package}/{theme}`
- `app/design/{area}/{package/{theme}`

By setting your theme/package correctly you can override the CSS, Skin JS, template files and layout XML.

Magento has a fallback system which means you can override the core files without editing them which will make your install far easier to upgrade. Although copying layout files will override the fallback files, it's not advisable; you should instead do all of your layout updates in `local.xml`, this will help prevent conflicts with other developers.

When editing `skin`, Magento actually say not to copy/name and CSS files with the same name as the fallback files. However this isn't a convention which is followed, even Ben Marks says on the Magento sold technical track that he would not advise sticking to this standard.

In layout XML you can update the block type without updating the template, this means you could override functions without rewriting blocks in config XML.

###How can you implement different designs for different stores using Magento themes?

You can set a theme on a store and store view basis in the configuration. However you cannot create Blocks on a website basis as they are global, however this is irrelevant really as you can chose to use a block within your layout XML.

###In which two ways you can register custom theme?

- The main way to register a theme/package is in ` System > Configuration > Design.`
- You can register a temporary theme in `System > Design`
- You can make theme/layout updates on a product/category level.

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Core_Model_Layout`
- `Mage_Core_Model_Layout_Update`
- `Mage_Core_Model_Design`
- `Mage_Core_Model_Design_Package`

##Mage_Core_Model_Design_Package:

###What is the difference between package and theme?

A package contains a set of themes, once you set a package that will be your fallback scope. So for example if your main theme was `foo` and your fallback was `bar` then this would be the fall back scope:

- `{area}/{package}/foo/* `
- ` {area}/{package}/bar/*`
- ` {area}/{package}/default/*`
- ` {area}/base/default/*`

To see/extend the functionality of the fallback system I would recommend looking at {% highlight ruby %}`Mage_Core_Model_Design_Package::getFilename`{% endhighlight %}.

###What happens if the requested file is missed in your theme/package?

It will use the fallbacks mentioned above.

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Core_Model_Layout`
- `Mage_Core_Model_Layout_Update`
- `Mage_Core_Model_Design`
- `Mage_Core_Model_Design_Package`
- `Mage_Core_Block_Template`

##Describe the process of defining template file paths:

I'm not sure if this question means defining the theme path or the actual template `.phtml` file. Because I've already covered how to set a theme and fallbacks I will assume its a

###Which kind of paths (absolute or relative) does Magento use for templates and layout files?

- `app/design/{area}/{package}/{theme}/template` - template files.
- `app/design/{area}/{package}/{theme}/layout` - layout files.

###How exactly can Magento define which physical file correspond to certain template/layout to use?

Layout files get defined in configuration XML, then Magento will rendered the blocks defined within the layout handles of the page being rendered. The blocks defined in here tell Magento what template to use and which type to use.

###Which classes and methods need to be rewritten in order to add additional directories to the fallback list?
{% highlight ruby %}
`Mage_Core_Model_Design_Package::getFilename`
{% endhighlight %}
These code references can be used as an entry point to find answers to the questions above:

- `Mage_Core_Model_Design`
- `Mage_Core_Model_Design_Package`
- `Mage_Core_Block_Template`
