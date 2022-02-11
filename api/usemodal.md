# useModal

{% hint style="info" %}
Hooks that exposes Modalfy's API.
{% endhint %}

{% hint style="danger" %}
**Note:** Prefer [**withModal()**](withmodal.md) HOC if you're using a Class component.
{% endhint %}

{% hint style="success" %}
The object returned by `useModal()`is covered in [**ModalProp**](types/modalprop.md).
{% endhint %}

{% tabs %}
{% tab title="TypeScript" %}
```javascript
const useModal = <P extends ModalfyParams>(): UsableModalProp<P> => {
  const context: UsableModalProp<P> = React.useContext(ModalContext)
  return {
    closeAllModals: context.closeAllModals,

    closeModal: context.closeModal,

    closeModals: context.closeModals,

    currentModal: context.currentModal,

    openModal: context.openModal,
  }
}
```
{% endtab %}
{% endtabs %}

{% embed url="https://github.com/colorfy-software/react-native-modalfy/blob/master/lib/useModal.ts" %}

{% hint style="info" %}
If you're using TypeScript and have [your params types](../guides/typing.md#modalprop), you can get some nice autocomplete by utilising `useModal()`like this:

```javascript
import { ModalStackParamsList } from 'App'
// ...
const { openModal } = useModal<ModalStackParamsList>()
```
{% endhint %}
