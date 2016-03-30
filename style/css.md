# BEM. Block, Element, Modifier

https://en.bem.info/method/naming-convention/

# Blocks

Blocks are namespaced to `b-` to differentiate BEM CSS from legacy CSS.

```css
.b-mylist { }
```

# Elements

A block can only have 1 element, which is indicated with a double underscore:

```css
.b-mylist__item { }
```

# Modifier

Modifiers can be boolean or key/value, and are indicated with a single underscore.

```css
/* boolean */
.b-mylist__item_error {}

/* key/value */
.b-mylist__item_state_failed {}
```
# Common misconceptions

BEM methodology involves the use of flat structure inside a block. It means that BEM entity can not be represented as an element of the other element and the following string representation will be invalid:

```css
'block__some-elem__sub-elem'
```

Also there is no such BEM entity as a modifier and an element modifier simultaneously so the following string representation will be invalid:

```css
'block_block-mod-name_block-mod-val__elem-name_elem-mod-name_elem-mod-val'
```

For clarity, the only allowed patterns in BEM are:

```
.b-block {}
.b-block_modifier {}
.b-block_modifier_value {}
.b-block__element {}
.b-block__element_modifier {}
.b-block__element_modifier_value {}
```
