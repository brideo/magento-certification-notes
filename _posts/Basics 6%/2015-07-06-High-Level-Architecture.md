---
layout: post
title: Describe Magento codepools, Magento Certification Notes
archive: magento
---
#High-level Magento architecture

##Describe Magento codepools

Magento has 3 sperate code pools

- core
	- Magento's Modules Developed by Magento
- Community
	- Third Party Modules / Extensions
 - local
 	- Internal Modules, Installations & Custom Overrides

##Describe typical Magento module structure

- app/etc/{namespace}_{module_name}.xml
- app/code/{code_pool}/{namespace}/{module_name}
	- etc (Configuration Files)
	- Block
	- Controller 
	- Model
	- Model/Resource
	- Helper
	- data (Data Scripts) (seeding)
	- sql (Installation Scripts) (migrations)

###{Variables}
{% highlight ruby %}
	$codepool = array(
    'local',
    'community',
    'core'
);

$namespace = 'Meanbee'
$module_name = 'Certification'
{% endhighlight %}
##Describe Magento templates and layout files location && Describe Magento skin and JavaScript files location

###View files
- app/design/{area}/{package}/{theme}/template
- app/design/{area}/{package}/{theme}/layout

###Skin Files
- skin/{area}/{package}/{theme}/images
- skin/{area}/{package}/{theme}/js
- skin/{area}/{package}/{theme}/css
- skin/{area}/{package}/{theme}/scss (Magento 1.9+)

###Other js
js/

In here you will find js libraries such as prototype, as a rule of thumb these are usually assets for modules.

###{Variables}
{% highlight ruby %}
	$area = array (
    'frontend',
    'adminhtml'
)

$package = 'meanbee';
$theme = 'default'
{% endhighlight %}

##Identify and explain the main Magento design areas (adminhtml and frontend)

Frontend is the stuff the user see's and adminhtml is for the /admin section of the website.

##Explain class naming conventions and their relationship with the autoloader

Magento's include path looks in app/code and lib. The autoloader uses the PSR-0 standard, it replaces the '_' with a ' ' then it replaces the ' ' with a DS or a Directory Seperator (In other terms a /). After that it sticks a .php extension on the end.
{% highlight ruby %}
	public function autoload($class)
{
    $classFile = str_replace(' ', DIRECTORY_SEPARATOR, ucwords(str_replace('_', ' ', $class)));
    $classFile.= '.php';
    @include $classFile;
}
{% endhighlight %}
##Describe methods for resolving module conflicts

There are 3 types of module conflicts:

###Configuration conflicts
To fix this you can open up your module file we refered to earlier and add a <depends> tag in.

- app/etc/Module_B.xml
{% highlight xml %}
	<depends>
    <Module_A />
</depends>
{% endhighlight %}
Adding the depends tag in this scenario will mean the configuration from Module_B will be loaded after the configuration from Module_A, because it depends on it to load... obviously.

An instance in which this could be used is say in the system configuration Module_A added a section called Module and now you go to write Module_b and you want to add a tab to that section, instead of writing all the system configuration again you can just reference the section created in Module_A.

###Rewrite conflicts

Okay so now Module_A and Module_B are rewriting the same Core_Class. Module_B code should take priority over Module_A as it's newer. To resolve this in the confiuration of Module_B just extend Module_A instead of Core_Class, simples.

###Theme conflicts

Maybe blocks share a name, maybe you removed a block, maybe you changed a blocks template, maybe you changed the blocks type, maybe you moved a block, maybe you duplicated a block. Basically theme conflicts happen in Layout xml. 

##To verify your understanding of these objectives, ask yourself the following questions:

###How does the framework interact with the various codepools?

Magento prioritieses codepools like this (1 being the most important and the first looked for):

1 local
2 Community
3 Core

###What constitutes a namespace and a module?

- Namespace
	- A namespace is whatever you want it to be, usually a company name such as Meanbee
- Module
	- The module name, descriptive of what its purpose is

###What does the structure of a complete theme look like?

###View files
- app/design/{area}/{package}/{theme}/template
- app/design/{area}/{package}/{theme}/layout

###Skin Files
- skin/{area}/{package}/{theme}/images
- skin/{area}/{package}/{theme}/js
- skin/{area}/{package}/{theme}/css
- skin/{area}/{package}/{theme}/scss (Magento 1.9+)
