---
icon: js
---

# Using outside React

#### [**> modalfy() API**](../api/modalfy.md)

Thanks to the rewrite that led to Modalfy v2, we can now control our modals from _outside_ React for the 1st time!

This means that we don't need to be inside any kind of component to open or close modals anymore. We could be in a function fetching some data or inside a Saga side effect and manage our modals without any problem.&#x20;

To do so, simply we `import { modalfy } from 'react-native-modalfy'` in any file and we'll have access to the same [**`ModalProp`**](../api/types/modalprop.md) as if we were using [**`useModal()`**](../api/usemodal.md) or [**`withModal()`**](../api/withmodal.md):

{% tabs %}
{% tab title="JavaScript" %}
```javascript
import { modalfy } from 'react-native-modalfy'

const {
  currentModal,
  openModal,
  closeModal,
  closeModals,
  closeAllModals,
} = modalfy()

openModal('MessageSentModal')
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
To always get the latest value of `currentModal,` consider using `modalfy().currentModal` when you need that information, instead of the aforementioned destructuring syntax.
{% endhint %}

