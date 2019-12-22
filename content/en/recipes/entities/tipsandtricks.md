---
title: "Tips&Tricks"
linkTitle: "Tips&Tricks"
weight: 10
description: >
    Tips and tricks for working with Entities.
---

### Generating an entity id

You can generate an entity id in the proper format using `CommerceEntity.IdPrefix<SellableItem>()` or  the `.ToEntityId<>()` extension method this code:

{{< highlight csharp >}}

    Console.WriteLine( CommerceEntity.IdPrefix<SellableItem>()+"ACMEWIDGET01" );
    Console.WriteLine( "ACMEWIDGET01".ToEntityId<SellableItem>() );
    Console.WriteLine( "1234567".ToEntityId<Order>() );

{{< /highlight >}}

will output: 

{{< highlight csharp >}}

Entity-SellableItem-ACMEWIDGET01
Entity-SellableItem-ACMEWIDGET01
Entity-Order-1234567

{{< /highlight >}}