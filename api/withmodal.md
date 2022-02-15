# withModal

{% hint style="info" %}
HOC that provides the `modal` prop to a wrapped Class component. The `modal` prop injected by`withodal()`is covered in [**`ModalProp`**](types/modalprop.md).
{% endhint %}

{% hint style="danger" %}
**Note:** Prefer[**`useModal()`**](usemodal.md)Hooks if you're using a functional component.
{% endhint %}

{% tabs %}
{% tab title="TypeScript" %}
```javascript
const withModal = <P extends ModalfyParams, Props extends object>(
  Component: React.ComponentClass<Props>,
) => {
  const { closeModal, closeModals, closeAllModals } = modalfy<P>()
  class WithModalComponent extends React.Component<ModalProp<P, Props>> {
    render() {
      return (
        <ModalContext.Consumer>
          {(context) => (
            <Component
              modal={{
                closeModal,
                closeModals,
                closeAllModals,
                openModal: context.openModal,
                currentModal: context.currentModal,
              }}
            />
          )}
        </ModalContext.Consumer>
      )
    }
  }

  return WithModalComponent
}
```
{% endtab %}
{% endtabs %}

{% embed url="https://github.com/colorfy-software/react-native-modalfy/blob/main/src/lib/withModal.tsx" %}

## API reference

```javascript
import React, { Component } from 'react'
import { Button, Text } from 'react-native'
import { withModal } from 'react-native-modalfy'

class ProfileScreen extends Component {
  render() {
    return (
      <View>
        <Text>Welcome!</Text>
        <Button
          title="Edit"
          onPress={() => this.props.modal.openModal('EditProfile')}
        />
      </View>
    )
  }
}

export default withModal(ProfileScreen)
```

Using **`withModal()`** will give you access to:

* `this.props.modal`
  * `currentModal`: name of the currently displayed modal if there's one
  * `openModal`: open a specific modal
  * `closeModal`: close a modal
  * `closeModals`: close every instance of a given modal
  * `closeAllModals`: close all open modals

as seen in [**`ModalProp`**](types/modalprop.md).
