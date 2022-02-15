# Triggering a callback

#### [> ModalProp API](../api/types/modalprop.md)

You may often want to perform an action as soon as you open or close a modal. Up until now, you had to skillfully employ `setTimeout()` in order to trigger your function after the modal is done animated. With Modalfy v3, you no longer have to think about it thanks to the built-in support of callbacks.

## 1. When opening

The [**`openModal()`**](../api/types/modalprop.md#openmodal) function now accepts a 3rd argument that designates your callback. Simply provide it and you should see it be invoked _after_ the modal has been opened (this includes animation time as well).

{% tabs %}
{% tab title="TypeScript" %}
```typescript
openModal('ErrorModal', { titleColor: 'red' }, () => {
  console.log(`✅ Opened ErrorModal`)
})
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

{% hint style="info" %}
As you can see, [**`openModal()`**](../api/types/modalprop.md#openmodal)still expects the`modalName`and`params`as the first two arguments, with the latter being optional.
{% endhint %}

## 2. When closing

As you'd expect, all the closing methods have also received a new argument that behaves in the same capacity:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
closeModal(undefined, () => console.log('✅ Closed latest opened modal'))

closeModals('ErrorModal', () => console.log('✅ Closed all ErrorModal'))

closeAllModals(() => console.log('✅ Closed all modals'))
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

{% hint style="info" %}
You'll notice that [**`closeModal()`**](../api/types/modalprop.md#closemodal) first argument is optional. That's you can either provide `undefined` (to close the latest opened modal) or specify a name to close another one.
{% endhint %}

## Things to remember

If you're using [**`animationIn`**](../api/types/modaloptions.md#animationin)/[**`animationOut`**](../api/types/modaloptions.md#animationout) to drive the modal animations yourself, remember that you have to call the new `callback` argument there in order for your callbacks to be called. You can refer to the dedicated section in the [**Upgrading from v2.x**](upgrading.md#modaloptions.animateout-new-mandatory-callback-argument) guide to learn more about it.
