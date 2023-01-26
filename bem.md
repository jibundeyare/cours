# BEM

BEM (Block, Element, Modifier) is a component-based approach to web development. The idea behind it is to divide the user interface into independent blocks. This makes interface development easy and fast even with a complex UI, and it allows reuse of existing code without copying and pasting.

## Key concepts

[Key concepts / Methodology / BEM](https://en.bem.info/methodology/key-concepts/)

- block
- element
- modifier

![Nested structure](img/bem-nested-structure.png)

![Block](img/bem-block.png)

![Element](img/bem-element.png)

![Modifier](img/bem-modifier.png)

## Naming convention

[Naming convention / Methodology / BEM](https://en.bem.info/methodology/naming-convention/)

### Naming rules

```
block-name__elem-name_mod-name_mod-val
```

- Names are written in lowercase Latin letters.
- Words are separated by a hyphen (`-`).
- The block name defines the [namespace](https://en.wikipedia.org/wiki/Namespace) for its elements and modifiers.
- The element name is separated from the block name by a double underscore (`__`).
- The modifier name is separated from the block or element name by a single underscore (`_`).
- The modifier value is separated from the modifier name by a single underscore (`_`).
- For boolean modifiers, the value is not included in the name.

Important: Elements of elements [do not exist in the BEM methodology](https://en.bem.info/methodology/faq/#why-not-create-elements-of-elements-block__elem1__elem2). The naming rules do not allow creating elements of elements, but you can nest elements inside each other in the DOM tree.

### Two Dashes style

```
block-name__elem-name--mod-name--mod-val
```

- Names are written in lowercase Latin letters.
- Words within the names of BEM entities are separated by a hyphen (`-`).
- The element name is separated from the block name by a double underscore (`__`).
- Boolean modifiers are separated from the name of the block or element by a double hyphen (`--`).
- The value of a modifier is separated from its name by a double hyphen (`--`).

Important: A double hyphen inside a comment (`--`) may cause an error during [validation of an HTML document](http://www.w3.org/TR/html5/syntax.html#comments).

## Ref

- [BEM](https://en.bem.info/)
- [Quick start / Methodology / BEM](https://en.bem.info/methodology/quick-start/)

