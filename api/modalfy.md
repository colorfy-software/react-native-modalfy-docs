# modalfy

{% hint style="info" %}
Function that exposes Modalfy's API outside of React's context. The object returned by `modalfy()`is covered in [**`ModalProp`**](types/modalprop.md).
{% endhint %}

{% hint style="danger" %}
**Note: Do not use if you're inside a React component.** Please consider [**`useModal()`**](usemodal.md) or [**`withModal()`**](withmodal.md) instead.
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
    closeAllModals: (callback?: () => void) => {
      ModalState.queueClosingAction({ action: 'closeAllModals', callback })
    },

    closeModals: (modalName: M, callback?: () => void) =>
      ModalState.queueClosingAction({
        action: 'closeModals',
        modalName,
        callback,
      }),
      
    closeModal: (modalName?: M, callback?: () => void) => {
      ModalState.queueClosingAction({
        action: 'closeModal',
        modalName,
        callback,
      })
    },

    currentModal: ModalState.getState<P>()?.currentModal,

    openModal: (modalName: M, params?: P[M], callback?: () => void) =>
      ModalState.openModal(modalName, params, true, callback),
  }
}
```
{% endtab %}
{% endtabs %}

{% embed url="https://github.com/colorfy-software/react-native-modalfy/blob/main/src/lib/ModalState.ts#L314-L398" %}

{% hint style="info" %}
If you're using TypeScript and have [your params types](../guides/typing.md#modalprop), you can get some nice autocomplete by utilizing `modalfy()`like this:

```javascript
import { ModalStackParamsList } from 'App'
// ...
const { openModal } = modalfy<ModalStackParamsList>()
```
{% endhint %}
