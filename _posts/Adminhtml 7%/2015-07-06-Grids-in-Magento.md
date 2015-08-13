---
layout: post
title: Grids in Magento, Certification Notes
archive: magento
---
#Grids in Magento

##Create a simple form and grid for a custom entity

Describe how to implement advanced Adminhtml Grids and Forms, including editable cells, mass actions, totals, reports, custom filters and renderers, multiple grids on one page, combining grids with forms, and adding custom JavaScript to an admin form:

- Filters
	- Used at the top of the grid for filtering
	- `Mage_Adminhtml_Block_Widget_Grid_Column_Filter_Range` - class used
	- Also handles the sorters
	- Uses the `addFieldToFilter()` method on the collection
- Totals
	- Used for adding up data

###Which block class do Magento grid classes typically extend?

	Mage_Adminhtml_Block_Widget_Grid

###What is the default template for Magento grid instances?

	app/design/adminhtml/default/default/template/widget/grid.phtml

###How can grid filters be customized?

###How does Magento actually perform sorting/paging/filtering operations?

Magento adds the filters and sorting within `_prepareCollection`.

###What protected methods are specific to adminhtml grids, and how are they used?

http://magentools.com/blog/magento-certification-preparation-study-guide-answers/

###What is the standard column class in a grid, and what is its role?

	Mage_Adminhtml_Block_Widget_Grid_Column

This class contains the information regarding the columns styling via it's type by storing a bunch of classes.

###What are column renderers used for in Magento?

The renderers control how the column is rendered on a box, for instance a checkbox would render a checkbox
using the `adminhtml/widget_grid_column_renderer_checkbox` class. All this class does is render a checkbox with the appropriate values and stuff, but it means this is done automatically for you without you having to think about it.

###How can JavaScript that is used for a Magento grid be customized?

	setJsObjectName()

###What is the role of the grid container class and its template?

	app/design/adminhtml/default/default/template/widget/grid/container.phtml

This contains the grid and adds the content header with the default buttons such as 'Add new {object}'.

###What is the programmatic structure of mass actions? 

The mass action block is added in using the `_prepareMassaction` method.

These code references can be used as an entry point to find answers to the questions above:

- `Mage_Adminhtml_Block_Widget_Grid`
- `Mage_Adminhtml_Block_Widget_Grid_Column`
- `Mage_Adminhtml_Block_Widget_Grid_Column_Renderer/ *`
- `Mage_Adminhtml_Block_Widget_Grid_Column_Filter/* `
- `Mage_Adminhtml_Block_Widget_Grid_Container`
- `app/design/adminhtml/default/default/template/widget/grid.phtml`
- `app/design/adminhtml/default/default/template/widget/grid/container.phtml`
