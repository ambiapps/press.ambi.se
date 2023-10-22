---
layout: post
title: "Style your app intents"
date: 2023-08-29 08:00:00 +0200
categories: ios17 intent appIntents shortcuts
image: /assets/img/styledIntent-1024.png
published: true
---

Starting in iOS 17 you can tint your app intents. The documentation from Apple is a bit vague on this, so here's how it's done.

![Screenshot showing what Ambre looks like with tinted intents]({{site.url}}/assets/img/styledIntent-1024.png)

The intent colors are defined in your `Info.plist`, you set them by adding the following to your `Info.plist` file:

```
<key>CFBundleIcons</key>
<dict>
    <key>CFBundlePrimaryIcon</key>
    <dict>
        <key>NSAppIconActionTintColorName</key>
        <string>ShortcutsForeground</string>
        <key>NSAppIconComplementingColorNames</key>
        <array>
            <string>ShortcutsBackground1</string>
            <string>ShortcutsBackground2</string>
        </array>
    </dict>
</dict>
```

The color assets `ShortcutsForeground`, `ShortcutsBackground1` and `ShortcutsBackground2` have to be defined in an `.assets` folder.

That's it! As you see we have defined two background colors to get a subtle gradient in our Ambre intents.
