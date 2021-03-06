---
layout: post
title: How to make general shellbar for every view?
categories: [SAP UI5 & Open UI5]
author_github: negativename
author: Vladislav Kobenko
---

In this article we show you how to add general shellbar for every view in your SAP UI5 app.

## manifest.json file

In your `manifest.json` file you need to find `rootView`:

```json
     "rootView": {
            "viewName": "replenishment.view.MainView",
            "type": "XML",
            "async": true,
            "id": "MainView"
        }
```

Then you need to open a `.xml` file of this view.

## MainView.view.xml file

Insert this code before the `Shell` tag.

```xml
    <f:ShellBar
        id="sapFShellBarSample"
        title="Example App"
        secondTitle="General ShellBar"
        homeIcon="./resources/logo.png"
        showSearch="true"
        showNavButton="true"
        navButtonPressed="onNavBack"
		>
        <f:profile>
			<Avatar initials="UI"/>
		</f:profile>
	</f:ShellBar>
```

There are some parameters, like title or navigation button and etc., which you will see on every view in your app.
