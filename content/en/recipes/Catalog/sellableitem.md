---
title: "Sellable Items"
linkTitle: "Sellable Items"
weight: 30
description: >
  Working with sellable items in a catalog.
tags:
  - catalog
  - sellable item  
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

### Setting display properties on a sellable item

{{< highlight csharp >}}

var displayProperties = sellableItem.GetComponent<DisplayPropertiesComponent>();
displayProperties.DisambiguatingDescription = "This is the description of my first product";
displayProperties.Color = "Black";
displayProperties.Size = "Large";
displayProperties.Style = "Modern";
displayProperties.DisplayOnSite = true;

{{< /highlight >}}

### Adding variants to a sellable item

{{< highlight csharp >}}

sellableItem = await this.Command<CreateSellableItemVariationCommand>()
  .Process(commerceContext, sellableItem.Id, "GreenSampleProduct", 
    "GreenSampleProduct", "Green Sample Product");
sellableItem = await this.Command<CreateSellableItemVariationCommand>()
  .Process(commerceContext, sellableItem.Id, "BlueSampleProduct", 
    "BlueSampleProduct", "Blue Sample Product");

var greenVariation = sellableItem.GetVariation("GreenSampleProduct");
var greenDisplayProperties = greenVariation.GetComponent<DisplayPropertiesComponent>();
greenDisplayProperties.Color = "Green";

var blueVariation = sellableItem.GetVariation("BlueSampleProduct");
var blueDisplayProperties = blueVariation.GetComponent<DisplayPropertiesComponent>();
blueDisplayProperties.Color = "Blue";

{{< /highlight >}}

### Localizing display properties on a sellable item

The following code adds localization 

{{< highlight csharp >}}

// getting localization entity for your sellable item. It will automatically 
// create new if there is no existing one.
var localizationEntity = await this.Command<LocalizeEntityPropertyCommand>()
        .GetLocalizationEntity(commerceContext, sellableItem);

// preparing translations
var displayNameLocalizations = new List<Parameter> {
    new Parameter{ Key = "en", Value = "My first product" },
    new Parameter{ Key = "de-DE", Value = "Mein erstes Produkt" }
};

// Adding localizations for DisplayName property
localizationEntity.AddOrUpdatePropertyValue("DisplayName", displayNameLocalizations);

// preparing translations
var colourLocalizations = new List<Parameter> {
    new Parameter { Key = "en", Value = "Black"},
    new Parameter { Key = "de-DE", Value = "Schwarz"}
};

// Adding localizations for Color property on DisplayPropertiesComponent
localizationEntity.AddOrUpdateComponentValue("DisplayPropertiesComponent", 
          displayProperties.Id, "Color", colourLocalizations);

// Save localization entity 
await Commander.PersistEntity(commerceContext, localizationEntity);

{{< /highlight >}}

### Adding localization for variants

The following code adds localization for the `DisplayName` property on the ItemVariationComponent. Note that you need to have a Localization entity first.

{{< highlight csharp >}}

// Retrieve or create a LocalizationEntity for the sellableItem
var localizationEntity = await this.Command<LocalizeEntityPropertyCommand>()
        .GetLocalizationEntity(commerceContext, sellableItem);

// Add localization for displayname on variants
var greenVariationLocalizations = new List<Parameter> {
    new Parameter { Key = "en", Value = "Green Sample Product" },
    new Parameter { Key = "de-DE", Value = "Grünes Musterprodukt" }
};

localizationEntity.AddOrUpdateComponentValue("ItemVariationsComponent.ItemVariationComponent", 
    "GreenSampleProduct", "DisplayName", greenVariationLocalizations);

var blueVariationLocalizations = new List<Parameter> {
    new Parameter { Key = "en", Value = "Blue Sample Product" },
    new Parameter { Key = "de-DE", Value = "Blaues Musterprodukt" }
};

localizationEntity.AddOrUpdateComponentValue("ItemVariationsComponent.ItemVariationComponent", 
    "BlueSampleProduct", "DisplayName", greenVariationLocalizations);

// Save localization entity 
await Commander.PersistEntity(commerceContext, localizationEntity);

{{< /highlight >}}                