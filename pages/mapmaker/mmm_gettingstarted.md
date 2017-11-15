---
title: Maria Map Maker documentation
keywords: Maria Map Maker overview
tags: [navigation]
sidebar: mmm_sidebar
permalink: mmm_gettingstarted.html
summary: Getting started using Maria Map Maker.
---

# Maria Map Maker

## Intro
The main purpose of Maria Map Maker is to create, edit and administer maps for MariaGDK 2.x and 3.x-based applications. A large portion of the functionality in the application is documented using tooltips. Simply hover the mouse over the names of settings and buttons, and a textbox will appear explaining its use.

## Getting started

Before using Maria Map Maker there are a few concepts you should know about:

*  **Product:** A product is a folder which contains a product metadata xml file. It will also contain map data, styling files, and/or other files that the user has found it useful to pack into a portable unit. A product can be self contained, or it can have dependencies to other products. An example is a product containing a detailed map of a small area, which is dependent on a background map in another product to appear as intended.

*  **Workspace:** A workspace is simply a folder containing one or more products. A user can have several workspaces active simultaneously, and products can have dependencies across workspaces.

When starting up Maria Map Maker for the first time, it's neccessary to define a workspace. This could be a folder where you have some existing products, or an empty folder where you intend to create new products. This is done in the "MANAGE WORKSPACES" window. Each workspace gets its own button in the vertical toolbar. 

Opening a workspace with its button in the vertical toolbar, you'll see a list of products. You can view them as a tree according to their virtual paths, or as a flat list.

You can in turn open one of the products and view its contents. The "Contents" pane lets you view existing templates. You can create new ones with the "Add Content" button. "Basemap" and "Overlay" are two variants of the template which are functionally identical in Maria Map Maker, but may be interpreted differently in other Maria GDK-based applications. 

The template is the starting point for editing maps. If you create a new template it will automatically open, and you can start adding layers to it. You can either add map data from existing products (Vector, Raster, Shading), or you can import a wide variety of common geodata formats into one of your active workspaces. 

## Importing geodata

When Maria Map Maker imports geodata, it will be processed into vector tiles (vector) or geopackage (raster, elevation). These are open source map formats, but the scheme used by Maria GDK to style them is proprietary. [These formats are supported.](./mmm_maps_supportedformats.html)

Importing geodata is done by dragging and dropping files and folders onto the map area, or by using **Add layer -> Map layer from file** in an open template. You will be prompted to enter a name and choose a workspace. You can also choose if you want to import the data into an existing product, or create a new one. In addition, there may be  other settings based on the format of the geodata.









