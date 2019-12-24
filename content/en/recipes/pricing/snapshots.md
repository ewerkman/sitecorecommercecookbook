---
title: "Snapshots"
linkTitle: "Snapshots"
weight: 30
---

### Creating a price book

var priceBook = await this.Command<AddPriceBookCommand>().Process(commerceContext, "SamplePriceBook", "Sample Price Book").ConfigureAwait(false);

### Associating a price book with a catalog


