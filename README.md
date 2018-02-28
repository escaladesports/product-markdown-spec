# Product Markdown Specification

A simple standard for managing products through markdown files.

## Example

```markdown
---
# ./markdown/apple.md
name: Red Apple
id: RA5
color: red
description: An apple.
price: 0.99
variants:
  - name: Green Apple
    id: GA2
    color: green
  - name: Rotten Apple
    id: RT1
    price: 0.01
    description: A gross apple.
---

An apple is a sweet, edible fruit produced by an apple tree (*Malus pumila*). -[Wikipedia](https://en.wikipedia.org/wiki/Apple)

```

## YAML Front Matter

The majority of your product information will be set as [YAML front matter](http://assemble.io/docs/YAML-front-matter.html). There are no required properties, but a minimum of `id` is recommended.

## ID Property

There aren't any requirements for formatting your ID, but you should try to keep the business logic for your IDs case insensitive. Transformations to upper or lower case should happen before any API calls or rendering.

## Variant Products

Variant products are products that inherit the properties of the parent with just a few changes. A practical example can be seen above, or with products like clothing that have multiple sizes. The variant property is an array full of objects detailing the differences between the variant and the parent product.

If an alternate property isn't supplied in the variant object, it will assume the same property as the parent. In the apple example above, the data would look something like this when unpacked:

```json
{
	"name": "Red Apple",
	"id": "RA5",
	"color": "red",
	"description": "An apple.",
	"price": 0.99,
	"body": "<p>An apple is a sweet...",
	"variant": false
}
{
	"name": "Green Apple",
	"id": "GA2",
	"color": "green",
	"description": "An apple.",
	"price": 0.99,
	"body": "<p>An apple is a sweet...",
	"variant": true
}
{
	"name": "Rotten Apple",
	"id": "RT1",
	"color": "green",
	"description": "A gross apple.",
	"price": 0.01,
	"body": "<p>An apple is a sweet...",
	"variant": true
}
```

## File Names

File paths and names should never be important to the application logic and should only named appropriately for the convenience of the developer. This makes it easier to organize product files as the project scales.

Applications should always recursively search for product markdown files so the folder structure can change and be as deep as it needs to be.

## Reserved Properties

While these properties can be used in your YML, they will likely be overwritten when the product objects are unpacked.

- `body`: (String) This will always unpack as the markdown content transpiled to HTML.
- `variant`: (Boolean) Used for identifying whether or not a product originated as a variant.

## Why Markdown?

If a static CMS is used, this makes it easier for a content editor to add their own custom HTML. In the page layouts, markdown body should always be treated as generic information about the product. This way if an emergency were to happen where you might need a quick body copy change, you can add copy quickly without restructuring your view layer.
