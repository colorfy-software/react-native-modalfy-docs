# Subscribing to events

#### [**> ModalComponentProp API**](../api/types/modalcomponentprop.md)

Once in a while, we might want to perform some animations alongside the ones applied to our modals. One example could be animating the modal and something inside it at the same time. But to do so and have those animations happening in sync, we'd need to have access to the `animatedValue` Modalfy uses under the hood. Use cases like this are the reason why we decided to provide a way to hook into the library's internal events, thanks to `modal.addListener()`.

{% tabs %}
{% tab title="React JSX" %}
{% code title="./modals/EmptyModal.tsx" %}
```jsx
import React, { useEffect, useRef }  from 'react'

const EmptyModal = ({ modal: { addListener }) => {
  const modalListener = useRef()

  const handleAnimation = (value) => {
    console.log('Modal animatedValue:', value)
  }

  useEffect(() => {
    modalListener.current = addListener('onAnimate', handleAnimation)
    
    return () => {
      modalListener.current?.remove()
    }
  }, [])

  return (
    //...
  )
}

export default EmptyModal
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
`onAnimate` is the only event we support for now. If there is another one you'd like to have, please: [let us know](https://github.com/colorfy-software/react-native-modalfy/issues/new)!
{% endhint %}
