---
title: "Sellable Items"
linkTitle: "Sellable Items"
weight: 30
description: >
  Working with sellable items in a catalog.
---

### Creating a sellable item

Create a sellable item with the following code:

{{< highlight csharp >}}

var createdSellableItem = await this.Commander.Command<CreateSellableItemCommand>()
  .Process(commerceContext, 
    "SampleProduct", 
    "Sample-Product", 
    "My first sample product", 
    "This is the description of my first product", "ACME", "ACME");

{{< / highlight >}}

After executing this code, the sellable item has been created but is not part of a catalog yet. If you look in the Business Tools, you won't see the sellable item, but it is still there. Remember that you can associate a sellable item with one ore more catalogs.

### Associating a sellable item with a catalog

To make a sellable item visible in the Business Tools and in the content editor, you need to associate it with a catalog or a category.

{{< highlight csharp >}}

var associateResult = await this.Commander.Command<AssociateSellableItemToParentCommand>()
  .Process(commerceContext, catalog.Id, catalog.Id, createdSellableItem.Id);

{{< /highlight >}}

The sellable item is now part of a catalog and will be visible in the Business Tools and in the content editor, directly below the catalog.

### Associating a sellable item with a category

To associate a sellable item with a category:

{{< highlight csharp >}}

var associateResult = await this.Commander.Command<AssociateSellableItemToParentCommand>()
  .Process(commerceContext, catalog.Id, subCategory.Id, createdSellableItem.Id);

{{< /highlight >}}


### Setting list prices on a sellable item

{{< highlight csharp >}}

// Retrieve the sellable item
var sellableItem = await this.Commander.GetEntity<SellableItem>(commerceContext, 
    "SampleProduct".ToEntityId<SellableItem>());

// * Set list pricings on a sellable item
sellableItem.ListPrice = new Money() { CurrencyCode = "EUR", Amount = 9.99M };
var listPricingPolicy = sellableItem.GetPolicy<ListPricingPolicy>();
listPricingPolicy.AddPrice(new Money("EUR", 9.99M));
listPricingPolicy.AddPrice(new Money("USD", 10.99M));
listPricingPolicy.AddPrice(new Money("GBP", 8.99M));
listPricingPolicy.AddPrice(new Money("CAD", 13.99M));

{{< /highlight >}}