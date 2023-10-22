---
layout: post
title:  "Using ShareLink to share content using NameDrop"
date:   2023-08-05 12:00:00 +0200
categories: ios17 shareLink SwiftUI NameDrop
image: /assets/img/shareSheet.png
published: false
---
At WWDC 23 a new feature named NameDrop was introduced. This feature makes it possible to share content from one device to another by simple placing them close to each other. To support this there are a few things we have to do.

First of all our struct/class has to conform to [Transferable](https://developer.apple.com/documentation/coretransferable/transferable). The Transferable protocol explains to the system how your content is shared.

One way to share recipes in [Ambre](https://ambre.ambi.se) is via URL, the conformance looks like this:

```
public class ShareableWebRecipe: Transferable {
    let recipe: Recipe

    public init(recipe: Recipe) {
        self.recipe = recipe
    }

    static public var transferRepresentation: some TransferRepresentation {
        ProxyRepresentation { shareableRecipe in
            try await Share.share(recipe: shareableRecipe.recipe, isAddedToGallery: false, container: CoreDataContainer.shared)
        }
    }
}
```

Once this is in place we can share a recipe using [ShareLink](https://developer.apple.com/documentation/SwiftUI/ShareLink).

```
ShareLink(
    item: ShareableWebRecipe(recipe: recipe),
    preview: SharePreview(recipe.safeName, image: recipe.getImage()),
    label: {
        Label("Share", systemImage: "square.and.arrow.up")
    })
```

If we tap the ShareLink button the following share sheet is presented:

![Share sheet screenshot]({{site.url}}/assets/img/shareSheet.png)

In this view we first tap `AirDrop` followed to placing our device close to another unlocked device. The NameDrop animation occurs and the content is transferred.

Ambre has support for Universal Linking, which means the received URL will open up Ambre and present the shared recipe. Here's what it looks like:

<center>
    <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/2w5ni7mOmy4?si=-ENcnuHwZpv_eoiW" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</center>
