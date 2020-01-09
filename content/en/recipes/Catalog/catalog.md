---
title: "Catalog"
linkTitle: "Catalog"
weight: 10
description: >
  Working with catalogs.
tags: 
  - catalog
---

### Creating a new catalog

Create a new catalog by executing the following command:

{{< highlight csharp >}}

     var catalog = await this.Commander.Command<CreateCatalogCommand>().Process(commerceContext, "SampleCatalog", "Sample Catalog");

{{< / highlight >}}

### Associating a catalog with a price book


See: [Associating a catalog with a price book]({{< ref "pricebook.md" >}})

### Deleting a catalog
