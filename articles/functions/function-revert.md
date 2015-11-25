<properties
	pageTitle="PowerApps: Revert function"
	description="Reference information for the Revert function in PowerApps, including syntax and examples"
	services=""
	suite="powerapps"
	documentationCenter="na"
	authors="gregli-msft"
	manager="dwrede"
	editor=""
	tags=""/>

<tags
   ms.service="powerapps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/21/2015"
   ms.author="gregli"/>

# Revert function in PowerApps #

Reloads and clears errors for the [records](working-with-tables.md#records) of a [data source](working-with-data-sources.md). 

## Description ##

The **Revert** function retrieves a fresh copy of the entire data source or a single record.  You will see changes made by other users. 

For the records reverted, **Revert** also clears any errors from the [table](working-with-tables.md) returned by the **[Errors](function-errors.md)** function.

If a conflict is reported by the **[Errors](function-errors.md)** function after a **[Patch](function-patch.md)** or other data operation, **Revert** the record to start with the conflicting version and reapply the change.

**Revert** has no return value.  It can only be used in behavior formulas. 

## Syntax ##

**Revert**( *DataSource* [, *Record* ] )

- *DataSource* – Required. The data source that you wish to revert.

- *Record* - Optional.  The record that you wish to revert.  If no record is specified, the entire data source is reverted.

## Examples ##

### Step by Step ###

In this example, you'll revert the data source named **IceCream**. The data source begins with:

![](media/function-revert/icecream.png)

A user on another device makes a modification to the Strawberry record, changing the **Quantity** to 400.  At about the same time, you change the same record's **Quantity** to 500, not knowing about the other change.

You **[Patch](function-patch.md)** your change:

- **Patch( IceCream, First( Filter( IceCream, Flavor = "Strawberry" ) ), { Quantity: 500 } )**

You check the **[Errors](function-errors.md)** table and find there is an error:

| Record | [Column](working-with-tables.md#columns) | Message | Error |
|--------|--------|---------|-------|
| **{ ID: 1, Flavor: "Strawberry", Quantity: 300 }** | *blank* | **"The record you are trying to modify has been modified by another user.  Please revert the record and try again."** | **ErrorKind!Conflict** |

Based on the **Error** column, you have a "Reload" button with an OnSubmit formula of:

- **Revert( IceCream, First( Filter( IceCream, Flavor = "Strawberry" ) ) )**

The **[Errors](function-errors.md)** table is now [empty](function-isblank-isempty.md), and the new value for Strawberry has been loaded:

![](media/function-revert/icecream-after.png)

Now you can reapply your change on top of the previous change, there is no longer a conflict, and the change succeeds. 

![](media/function-revert/icecream-success.png)
