---
title: Styling
keywords: styling
tags: [drawobject, track]
sidebar: core_sidebar
permalink: core_styling.html
summary: The Maria GDK features powerful mechanisms for controlling the appearance of tracks and draw objects. Several aspects of track and draw object visualisation can be controlled using styling. Conditional styling allows styling based on track attributes or draw object fields, map attributes and external settings. 
---

## Overview

Styles are defined using XML-data. The common style features will be explained by using Track styling as an example. For styling specific to tracks and draw objects, see [styling draw objects](core_styling_drawobject.html) and [styling tracks](core_styling_track.html).

A very simple track style can be defined as follows:

```xml
<styleset name="Default">
	<stylecategory name="TrackSymbol">
		<style>
			<valueitem name="Visible"value="true"/>
			<compositeitem name="CoreSymbol">
				<valueitem name="Opacity"value="1.0"/>
				<valueitem name="Scale"value="1.5"/>
				<valueitem name="DynamicScale"value="false"/>
				<valueitem name="SymbolKeyField"value="symbol.2525code"/>
				<valueitem name="Symbology"value="MilStd2525"/>
			</compositeitem>
		</style>
	</stylecategory>
</styleset>
```  

By using this style definition, all tracks are rendered using Mil standard 2525 symbology (`<valueitem name="Symbology" value="MilStd2525"/>`) and symbols are fixed size at all map scales (`<valueitem name="DynamicScale" value="false"/>`). In order to change symbology to NTDS, simply change the Symbology item to `<valueitem name="Symbology" value="NTDS"/>`

When drawing the actual track, the values in “valueitem”-elements are used as input. All styling ultimately boils down to generating the appropriate valueitems for a given track. In the example above, this is very straightforward; the same valueitems are used for every track. Real life styling definitions will typically include conditional styling, allowing tracks to change visualisation based on system settings, map properties and track properties.

## Style sets and style categories

The top-level item of the style definition is the “styleset”. A styleset contains a collection of style categories covering different aspects of track visualisation.

The style set and categories are represented by interfaces as follows:
{% include image.html file="core/maria_2102_track_styling_html_m573ba0f5.png" caption="IStyleSheet" %}


### Style evaluation

Understanding how the styles are evaluated is important in order to create complex styles.

Each track or draw object contains a collection of resolved style items. The final style item values are based on:

*  The styleset 

*  Track or draw object data 

*  External input represented by *ItemContext*. External input includes object states such as selection, user input and named variables 

Each style category in the styleset contains one or more styles. When these are resolved, only concrete style items will remain. For a given track or draw object, each *style* entry in the definition is either included or not included based on the style condition.

```xml
<styleset name="Default">
	<stylecategory name="TrackSymbol">
		<style>
AB    <valueitem name="Visible"value="true"/>
AB    <compositeitem name="CoreSymbol">
				<valueitem name="Opacity"value="1.0"/>
				<valueitem name="Scale"value="[track.symbolscale]"/>
				<valueitem name="DynamicScale"value="false"/>
				<valueitem name="SymbolKeyField"value="symbol.2525code"/>
				<valueitem name="Symbology"value="MilStd2525"/>
			</compositeitem>
			<compositeitem name="SpeedVector">
A,B1    <valueitem name="Thickness"value="2.0"/>
A,B1    <valueitem name="Len"value="30.0"/>
			</compositeitem>
		</style>
		<style>
>>>   <speedcondition value="20kts"op="Gt"/>
			<compositeitem name="SpeedVector">

**B     <valueitem name="Color" value="255,0,0,128"/>
**B     <valueitem name="Thickness" value="3.0"/>
**B     <valueitem name="Len" value="40.0"/>
			</compositeitem>
		</style>
	</stylecategory>
</styleset>
```

Consider track A and B. Track A has speed 10 kts, B 25 kts. Using the style definition above, track A is assigned the “Visible”, “CoreSymbol” and “SpeedVector” items contained in the first “style” element. The resulting XML representation of the style elements are:

```xml
<compositeitem>
	<valueitem name="Visible"value="true"/>
	<compositeitem name="CoreSymbol">
		<valueitem name="Opacity"value="1.0"/>
		<valueitem name="Scale"value="1.1"/>
		<valueitem name="DynamicScale"value="false"/>
		<valueitem name="SymbolKeyField"value="symbol.2525code"/>
		<valueitem name="Symbology"value="MilStd2525"/>
	</compositeitem>
	<compositeitem name="SpeedVector">
		<valueitem name="Thickness"value="2.0"/>
		<valueitem name="Len"value="30.0"/>
	</compositeitem>
</compositeitem>
```

Note that `<valueitem name="Scale"value="[track.symbolscale]"/>`{xml} is resolved to: `<valueitem name="Scale"value="1.1"/>`{xml} Brackets are used to indicate external variables. These are substituted with actual values during resolution.

Track B satisfies the condition contained in the second style element: `<speedconditionvalue="20kts"op="Gt"/>`{xml} As a result, all style items in the first style are substituted with style items from the second style with the same name. Note that compositeitems are merged, so that all valueitems in the first style will either be replaced by items in the second style element or included.

#### Style item states

Style items (`<valueitem>` and `<compositeitem>`) are by default “active”. To specify other states, include the attribute “state”:

```xml
<style>
	<speedcondition value="1kts"op="Lt"/>
	<compositeitem name="SpeedVector"state="suppress"/>
</style>
```

Legal states are:

 | State        | Description                                                                                     | 
 | -----        | -----------                                                                                     | 
 | **Active**   | Default. If style leaf node, will replace any current style value with the same key.            | 
 | **Inactive** | Skipped during resolution. If set on a composite, children are not applied to the current style | 
 | **Suppress** | Removes style item. If composite, all children are removed. If leaf, that value is removed      | 

#### Conditions

Conditions are used by “style” elements to determine if a given style element shall be applied to a given track or draw object. In the example above, “speedcondition” is used to separate slow tracks from fast tracks, and to assign different speed vector coloring for fast tracks.

Condition types will be added by Teleplan Globe as needed. Currently, the following conditions are implemented:

**Conditions:**

 | Name               | Description                                                                                                                                                                                                                                                                                                                                                                                                               | 
 | ----               | -----------                                                                                                                                                                                                                                                                                                                                                                                                               | 
 | FieldCondition     | Used to compare fields with a specific value or variable. “field” attribute contains the field name, “value” contains the value to compare with and “op” contains the operator (see Table 2 Field operators).                                                                                                                                                                                                 | 
 | CompositeCondition | Used to group other conditions. Child conditions are defined as child elements. “op” attribute contains the logical operator used to combine children and can be “And”, “Or” or “None”.                                                                                                                                                                                                                   | 
 | StateCondition     | Can be used to test for named system states or item states in the itemcontext, for instance selection state: `<statecondition key="Selected" scope="PerItem" state="Active"/>` Note that the states are currently controlled by Teleplan Globe.                                                                                                                                                                             | 
 | VariableCondition  | Test variable in itemcontext. This allows for instance a continuous slider value in the host app to control stepwise styling. An instance is “detail level”. The styling can define at what level for instance labels and speed vectors should be dropped based on a single value. “varname” contains the variable name, “value” contains the value to compare with and “op” contains the field operator. | 
 | MapScaleCondition  | Used to test against the systems map scale. “value” contains the map scale value to compare with and “op” contains the operator.                                                                                                                                                                                                                                                                                  | 
 | MapStateCondition  | Used to test against the systems map state. “value” contains the map state value to compare with and can be Stationary or Changing, “op” contains the operator.                                                                                                                                                                                                                                                   | 

**Field operators:**

 | Name      | Description                                                                                                                                                                                                                                                                                                                                                                                                   | 
 | ----      | -----------                                                                                                                                                                                                                                                                                                                                                                                                   | 
 | Eq        | Equals, should be used for string and integer values only                                                                                                                                                                                                                                                                                                                                                     | 
 | NEq       | Not equal, should be used for string and integer values only                                                                                                                                                                                                                                                                                                                                                  | 
 | Gt, GtEq  | Greater, greater or equal                                                                                                                                                                                                                                                                                                                                                                                     | 
 | Lt, LtEq  | Less, less or equal                                                                                                                                                                                                                                                                                                                                                                                           | 
 | In        | Condition value should be a comma separated list of string or integer values. The “In” operator provides fast matching for a list of values. Example `<fieldcondition field="cargocode" op="in" value="1,7,13,55,64"/>`                                                                                                                                                                                     | 
 | Contains  | True if condition value is contained in the target                                                                                                                                                                                                                                                                                                                                                            | 
 | Defined   | True if field or value is defined                                                                                                                                                                                                                                                                                                                                                                             | 
 | Undefined | True if field or value is undefined                                                                                                                                                                                                                                                                                                                                                                           | 
 | AnyIn     | Condition value should be a comma separated list of string or integer values. The “AnyIn” operator matches multiple values in condition against multiple values in object and returns true if any of the values overlap. Example `<fieldcondition field="cargocodes" op="AnyIn" value="1,7,13,55,64"/>`. True if object has cargocodes set to "13,14", false if object has cargocodes set to "15,16,apples" | 

#### Variables

Variables are defined in an IItemContext. A variable is a simple key-value pair where the value can be any string. Any “value” attribute in a style item can contain a variable reference. This is done by enclosing the value in square brackets: `<valueitem name="Scale" value="[track.symbolscale]"/>` `{xml}

#### Item context

The item context contains predefined variables and item state.

## Style item details

Teleplan Globe will expand the list of legal style items as needed. Updated examples will be provided as new elements are added.

Note that `<valueitem …>` is used to contain actual styling. The actual data type of an item is not determined until the item is used. If parsing fails, the item is ignored.

Some general guidelines:


*  Colors are encoded as “r,g,b,a”, each component is in the range 0-255. 50% transparent red would be written as `<valueitem name=”Color” value=”255,0,0,128”/>`, opaque red as `<valueitem name=”Color” value=”255,0,0,255”/>`

*  Scaling, explicit opacity and other relative values are usually in the range [0,1] 

*  Length and thickness values are in pixels on raster display. These may be scaled depending on DPI 

*  FontSize is in font points 

*  Time spans are in decimal seconds 

*  Boolean values are “true” or “false” 

*  Floating point values use “.” As decimal point 

{% include links.html %}
