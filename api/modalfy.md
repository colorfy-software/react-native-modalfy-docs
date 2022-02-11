# modalfy

{% hint style="info" %}
Function that exposes Modalfy's API outside of React's context.
{% endhint %}

{% hint style="danger" %}
**Notes:**&#x20;

* **Do not use if you're inside a React component.** \
  Please consider [**useModal()**](usemodal.md) or [**withModal()**](withmodal.md) instead.
* You can't pass a `modalName` to `closeModal()` to close a modal that's not the currently displayed one. This is due to a current API limitation especially in the case where you'd have several modals with the same name opened, as Modalfy can't safely determine which one you wanted to close.
{% endhint %}

{% hint style="success" %}
The object returned by `modalfy()`is covered in [**ModalProp**](types/modalprop.md).
{% endhint %}

{% tabs %}
{% tab title="TypeScript" %}
```javascript
const modalfy = <
  P extends ModalfyParamsm,
  M extends keyof P
>(): UsableModalProp<P> => {
  const context: UsableModalProp<P> = React.useContext(ModalContext)
  return {
    closeAllModals: ModalState.closeAllModals,

    closeModal: () => ModalState.closeModal(),

    closeModals: (modalName: M) => ModalState.closeModals(modalName),

    currentModal: ModalState.getState<P>()?.currentModal,

    openModal: (modalName: M, params?: P[M]) =>
      ModalState.openModal(modalName, params, true),
  }
}
```
{% endtab %}
{% endtabs %}

{% embed url="https://github.com/colorfy-software/react-native-modalfy/blob/master/lib/ModalState.ts#L253" %}
Types have been simplified for the sake of clarity. Refer to the exact definitions here.
{% endembed %}

{% hint style="info" %}
If you're using TypeScript and have [your params types](../guides/typing.md#modalprop), you can get some nice autocomplete by utilising `modalfy()`like this:

```javascript
import { ModalStackParamsList } from 'App'
// ...
const { openModal } = modalfy<ModalStackParamsList>()
```
{% endhint %}
