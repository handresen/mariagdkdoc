---
title: Drawobject default values xml
keywords: drawobjects
tags: [drawobject]
sidebar: core_styling_sidebar
permalink: core_styling_drawobject_defaultvalues_xml.html
summary: Complete style set targeting draw object fields.
---

# DrawObjectDefaultValueStyle.xml

```xml
<?xml version="1.0" encoding="utf-8" ?>
<styleset name="Default">
  <stylepropertymap>
    <mapitem name="Line.Width" target="LineWidth"/>
    <mapitem name="Line.Color" target="LineColor"/>
    <mapitem name="Line.DashStyle" target="LineDashStyle"/>		
    <mapitem name="Line.ShowLinePointText" target="ShowLinePointText"/>    
    <mapitem name="DefaultFont.Bold" target="BoldFont"/>
    <mapitem name="DefaultFont.Italic" target="ItalicFont"/>
    <mapitem name="DefaultFont.Underline" target="UnderlineFont"/>
    <mapitem name="DefaultFont.Strikethrough" target="StrikethroughFont"/>
    <mapitem name="DefaultFont.Font" target="DefaultFontName"/>
    <mapitem name="DefaultFont.Size" target="DefaultFontSize"/>
    <mapitem name="DefaultFont.Color" target="DefaultFontForegroundColor"/>
    <mapitem name="DefaultFont.BackgroundColor" target="DefaultFontBackgroundColor"/>    
    <mapitem name="Fill.ForegroundColor" target="FillForegroundColor"/>
    <mapitem name="Fill.BackgroundColor" target="FillBackgroundColor"/>
    <mapitem name="Fill.Style" target="FillStyle"/>    
    <mapitem name="Buffer.Width" target="BufferLineWidth"/>
    <mapitem name="Buffer.Color" target="BufferLineColor"/>
    <mapitem name="Buffer.DashStyle" target="BufferLineDashStyle"/>    
    <mapitem name="Label.Draw" target="DrawLabel"/>    
    <mapitem name="Unrelated.DrawOutline" target="DrawOutline"/>
    <mapitem name="Unrelated.Smooth" target="Smooth"/>
  </stylepropertymap>
  <stylecategory name="DrawObjects">
    <style>
      <compositeitem name="Line">
        <valueitem name="Width" value="[LineWidth]"/>
        <valueitem name="Color" value="[LineColor]"/>
        <valueitem name="DashStyle" value="[LineDashStyle]"/>
      </compositeitem>
      
      <compositeitem name="Fill">			
        <valueitem name="ForegroundColor" value="[FillForegroundColor]"/>
        <valueitem name="BackgroundColor" value="[FillBackgroundColor]"/>
        <valueitem name="Style" value="[FillStyle]"/>
      </compositeitem>
      
      <compositeitem name="Buffer">			
        <valueitem name="Width" value="[BufferLineWidth]"/>
        <valueitem name="Color" value="[BufferLineColor]"/>
        <valueitem name="DashStyle" value="[BufferLineDashStyle]"/>
      </compositeitem>
      
      <compositeitem name="DefaultFont">
        <valueitem name="Bold" value="[BoldFont]"/>
        <valueitem name="Italic" value="[ItalicFont]"/>
        <valueitem name="Underline" value="[UnderlineFont]"/>
        <valueitem name="Strikethrough" value="[StrikethroughFont]"/>
        <valueitem name="Font" value="[DefaultFontName]"/>
        <valueitem name="Size" value="[DefaultFontSize]"/>
        <valueitem name="Color" value="[DefaultFontForegroundColor]"/>
        <valueitem name="BackgroundColor" value="[DefaultFontBackgroundColor]"/>
        <valueitem name="MinFontSize" value="[MinFontSize]"/>
        <valueitem name="MinFontMapScale" value="[MinFontMapScale]"/>
        <valueitem name="MaxFontMapScale" value="[MaxFontMapScale]"/>
        <valueitem name="CustomFontScale" value="[CustomFontScale]"/>
      </compositeitem>
      
      <compositeitem name="Label">
        <valueitem name="Draw" value="[DrawLabel]" />
      </compositeitem>
      
      <compositeitem name="Unrelated">
        <valueitem name="DrawOutline" value="[DrawOutline]" />
        <valueitem name="Smooth" value="[Smooth]" />
      </compositeitem>			
    </style>
  </stylecategory>
</styleset>
```
