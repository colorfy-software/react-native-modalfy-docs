# Upgrading from v1.x

We tried to keep the breaking changes as minimal as possible. As of now, here is the list of the changes we know you'll have to make if you're coming from Modalfy v1:

## API

### modal.getParams() ➜ modal.getParam()

We drop the support for the generic `getParams(defaultValue)` in favour of `getParam(paramName, defaultValue)` which gives us a much more fine-grained control. &#x20;

`getParams()` was returning the whole `params` variable or the `defaultValue` if we didn't provide any params to the modal. `getParam()` in the other hand allows us to access every single `params` key independently (granting `params` is an object), and `defaultValue` now applies to that specific key, instead of the whole `params` object.&#x20;

### backButtonBehavior ➜ backBehavior

This change applies wherever we were defining this key inside `modalOptions` object. We dropped the `Button` part as pressing the backdrop now supports `backBehavior`.

## Types

### Flow ➜ TypeScript

The library has been completely rewritten from Flow to TypeScript. We deciced to drop support for the former to only pursue with the latter. If you feel like Flow support is essential to you, feel free to submit a PR with updated [Flow definitions](https://github.com/flow-typed/flow-typed)!

### ModalStackOptions ➜ ModalOptions

Given that this type isn't only used when creating the stack, it didn't make sense to include it in the name.

### ModalStackItemProp ➜ ModalComponentProp

If you were already using the TypeScript definition files, `ModalStackItemProp` used in the modal components files has been replaced by `ModalComponentProp`. Check out its updated section in the [**TypeScript guide**](typing.md#modalcomponentprop) to see how it's used.

### ModalProp ➜ ModalProp (with props & params now)

If you were already using the TypeScript definition files, `ModalProp` used in regular components has been updated by a more complete/simpler to use `ModalProp`. Check out its updated section in the [**TypeScript guide**](typing.md#modalprop) to see how it's used.

{% hint style="success" %}
This guide is a work in progress! As more people upgrade their apps we can continue to improve it. Please send pull requests to add any suggestions that you have from your upgrade experience.
{% endhint %}

