---
layout: post
title: How to get selected items or properties of this item from the table in SAP UI5?
categories: [SAP UI5 & Open UI5]
author_github: negativename
author: Vladislav Kobenko
--- 

In case you need to get selected items or property of this item from the table in SAP UI5, you need to add few lines of code in your controller file.

## In SAP UI5 Controller.js
To get all selected items add these lines in `*.controller.js` file.

```js
    var oTable = this.getView().byId("Table");
    var aItems = oTable.getSelectedItems();
```

If you need to get value of specific properties for your selected items, you need to add this.
```js
    var paramsObject = {
        "param1" : [],
        "param2" : []
    };
    for (let i = 0; i < aItems.length; i++){
        paramsArray.param1.push(aItems[i].getBindingContext().getProperty("param1"));
        paramsArray.param2.push(aItems[i].getBindingContext().getProperty("param2"));
    }
```

Now you have an object with different array of params for every selected row, that you choose.
