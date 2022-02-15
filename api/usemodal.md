# useModal

{% hint style="info" %}
Hooks that exposes Modalfy's API. The object returned by `useModal()`is covered in [, you can get some nice autocomplete by utilizing **`ModalProp`**](types/modalprop.md).
{% endhint %}

{% hint style="danger" %}
**Note:** Prefer [**`withModal()`**](withmodal.md) HOC if you're using a Class component.
{% endhint %}

{% tabs %}
{% tab title="TypeScript" %}
```javascript
const useModal = <P extends ModalfyParams>(): UsableModalProp<P> => {
  const context: UsableModalProp<P> = React.useContext(ModalContext)
  const { closeModal, closeModals, closeAllModals } = modalfy<P>()
  return {
    closeAllModals: closeAllModals: closeAllModals as UsableModalProp<P>['closeAllModals'],

    closeModals: closeModals as UsableModalProp<P>['closeModals'],

    closeModal: closeModal as UsableModalProp<P>['closeModal'],

    currentModal: context.currentModal,

    openModal: context.openModal,
  }
}
```
{% endtab %}
{% endtabs %}

{% embed url="https://github.com/colorfy-software/react-native-modalfy/blob/main/src/lib/useModal.ts" %}

{% hint style="info" %}
If you're using TypeScript and have [your params types](../guides/typing.md#modalprop), you can get some nice autocomplete by utilizing`useModal()`like this:

```javascript
import { ModalStackParamsList } from 'App'
// ...
const { openModal } = useModal<ModalStackParamsList>()
```
{% endhint %}
