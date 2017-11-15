---
title: Symbolpoint
keywords: symbolpoint, styling
tags: [drawobject]
sidebar: core_sidebar
permalink: core_symbolpoint.html
summary: .... 
---

### Configuring symbol point libraries

```json
{
	"id": "simplepoi",
	"name": "Basic test symbol library",
	"symbolPath": "simplepoi/symbols",
	"tags": [ "simplepoitag", "tpdev", "internal" ],
	"groups": 	[
			{
			"id": "styled",
			"name": "default",
			"symbolGrouppath": "",
			"symbolPostfix": ".svg",
			"internal": false,
						
			"styleoptions":
			{
				"template":
				[".background *{fill: $BK_C !important;}",
				".foreground *{fill: $FG_C !important;}",				
				".background *{stroke-dasharray: $OL_DA !important;}",
				".foreground *{stroke-dasharray: $OL_DA !important;}",								
				".background *{stroke: $OL_C !important;}",
				".foreground *{stroke: $OL_C !important;}",	
				".text *{fill: $TE_C !important;}"],
				"items": 
				[
					{
						"name":"Background color",
						"type":"css_color",
						"id":"BK_C"
					},
					{
						"name":"Foreground color",
						"type":"css_color",
						"id":"FG_C"
					},
					{
						"name":"Outline linestyle",
						"type":"css_dasharray",
						"id":"OL_DA"
					},
					{
						"name":"Outline color",
						"type":"css_color",
						"id":"OL_C"
					},
					{
						"name":"Text color",
						"type":"css_color",
						"id":"TE_C"
					}
				]
			},
			"mappings": 
			[
				{
					"id": "SHP",
					"symbolPath": "testStyling",
					"symbolName": "Shapes for test",
					"tags": ["tpdev"],
				},
				{
					"id": "CI",
					"symbolPath": "circle2",
					"symbolName": "Circle template",
					"tags": ["tpdev"],
					"styleoptions":
					{
						"template":[".text *{opacity: $TE_OP !important;}",
						".background *{stroke-dasharray: $BK_DASH !important;}"],
						"items": 
						[
							{
								"name":"Background color2",
								"type":"css_color",
								"id":"BK_C"
							},
							{
								"name":"Text opacity",
								"type":"css_opacity",
								"id":"TE_OP"
							}		
						]
					}							
				}
			]					
		}		
	]
}
```