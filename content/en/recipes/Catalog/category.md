---
title: "Category"
linkTitle: "Category"
weight: 20
description: >
  Working with categories in a catalog.
tags:
  - catalog
  - category
---

### Creating a root category

Creating a root category consists of two steps: 

1. Create a category;
2. Associate the created category with the catalog we want it to be a root category of;

{{< highlight csharp >}}

// Create a category
var rootCategory = await this.Commander.Command<CreateCategoryCommand>()
  .Process(commerceContext, 
          catalog.Id, 
          "Departments", 
          "Departments", 
          "These are the different departments in our catalog");

// Associate the created category with the catalog
var associateRootCategoryResult = await this.Commander.Command<AssociateCategoryToParentCommand>()
    .Process(commerceContext, catalog.Id, catalog.Id, rootCategory.Id);          

{{< / highlight >}}

### Creating a sub category

To create a sub category, you:

1. Create a category;
2. Associate it with the category you want it to be a child of.

{{< highlight csharp >}}

// * Create a sub category
var subCategory = await this.Commander.Command<CreateCategoryCommand>()
  .Process(commerceContext, catalog.Id, 
      "Samples", 
      "Sample Products", 
      "This is our collection of sample products");

// * Associate a category with a catalog
var associateSubCategoryResult = await this.Commander.Command<AssociateCategoryToParentCommand>()
  .Process(commerceContext, catalog.Id, rootCategory.Id, subCategory.Id);

{{< / highlight >}}
