---
title: marker-start
slug: Web/SVG/Reference/Attribute/marker-start
page-type: svg-attribute
browser-compat: svg.global_attributes.marker-start
sidebar: svgref
---

The **`marker-start`** attribute defines the arrowhead or polymarker that will be drawn at the first vertex of the given [shape](/en-US/docs/Web/SVG/Reference/Element#shape_elements).

For all shape elements, except {{SVGElement("polyline")}} and {{SVGElement("path")}}, the last vertex is the same as the first vertex. In this case, if the value of `marker-start` and {{SVGAttr("marker-end")}} are both not `none`, then two markers will be rendered on that final vertex. For `<path>` elements, for each closed subpath, the last vertex is the same as the first vertex. `marker-start` is only rendered on the first vertex of the [path data](/en-US/docs/Web/SVG/Reference/Attribute/d#path_commands).

> [!NOTE]
> As a presentation attribute, `marker-start` also has a CSS property counterpart: {{cssxref("marker-start")}}. When both are specified, the CSS property takes priority.

You can use this attribute with the following SVG elements:

- {{SVGElement("circle")}}
- {{SVGElement("ellipse")}}
- {{SVGElement("line")}}
- {{SVGElement("path")}}
- {{SVGElement("polygon")}}
- {{SVGElement("polyline")}}
- {{SVGElement("rect")}}

## Example

```css hidden
html,
body,
svg {
  height: 100%;
}
```

```html
<svg viewBox="0 0 120 120" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <marker
      id="triangle"
      viewBox="0 0 10 10"
      refX="1"
      refY="5"
      markerUnits="strokeWidth"
      markerWidth="10"
      markerHeight="10"
      orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#f00" />
    </marker>
  </defs>
  <polyline
    fill="none"
    stroke="black"
    points="20,100 40,60 70,80 100,20"
    marker-start="url(#triangle)" />
</svg>
```

{{EmbedLiveSample("Example", "200", "200")}}

## Usage notes

<table class="properties">
  <tbody>
    <tr>
      <th scope="row">Value</th>
      <td><code>none</code> | <code>&#x3C;marker-ref></code></td>
    </tr>
    <tr>
      <th scope="row">Default value</th>
      <td><code>none</code></td>
    </tr>
    <tr>
      <th scope="row">Animatable</th>
      <td>discrete</td>
    </tr>
  </tbody>
</table>

- `none`
  - : Indicates that no marker symbol shall be drawn at the first vertex.
- `<marker-ref>`
  - : This value is a reference to a {{SVGElement("marker")}} element, which will be drawn at the first vertex. If the reference is not valid, then no marker will be drawn.

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- {{SVGElement("marker")}}
- {{SVGAttr("marker-end")}}
- {{SVGAttr("marker-mid")}}
- CSS {{cssxref('marker-start')}} property
