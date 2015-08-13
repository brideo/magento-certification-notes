---
layout: post
title: Forms in Magento, Certification Notes
archive: magento
---
#Forms in Magento

##Define form structure, form templates, grids in Magento, and grid containers and elements:

Admin pages are all containers, and then containers for containers.

You can see all these containers use `Mage/Adminhtml/Block/Widget/*`.

###Which block does a standard Magento form extend?

	Mage_Adminhtml_Block_Widget_Form

###What is the default template for a Magento form?

	app/design/adminhtml/default/default/template/widget/form.phtml

###Describe the role of a form container and its template.

	app/design/adminhtml/default/default/template/widget/form/container.phtml

This form template container wraps the form.

Adminhtml has a lot of wrappers which is good for quickly implementing new features without any design changes. 

###Describe the concept of Form elements, and list system elements implemented in Magento.

Form elements are used to add specific field types to your forms, all these can be found in:

	lib/Varien/Data/Form/Element/*

###Describe the concept of fieldsets. 

Fieldsets are containers for fields, you need to define some element types to add fields to your fieldset. 

How can you render an element with a custom template? These code references can be used as an entry point to find answers to the questions above: 

- `lib/Varien/Data/Form/*`
- `Mage_Adminhtml_Block_Widget_Form`
- `Mage_Adminhtml_Block_Widget_Form_Container`
