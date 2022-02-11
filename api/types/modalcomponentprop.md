# ModalComponentProp

{% hint style="info" %}
Interface of the `modal` prop exposed by the library specifically to modal components.
{% endhint %}

{% hint style="success" %}
**Note:** `ModalComponentProp` **** reuses the same types as [`ModalProp`](modalprop.md) and only adds 4 more on top of them.
{% endhint %}

{% tabs %}
{% tab title="TypeScript" %}
```typescript
export type ModalComponentProp<
  P extends ModalfyParams,
  Props = unknown,
  M = keyof P
> = Props & {
  modal: UsableModalComponentProp<P, M>
}

// ------------------ INTERNAL TYPES ------------------ //

type ModalfyParams = { [key: string]: any }

interface UsableModalComponentProp<
  P extends ModalfyParams,
  M extends keyof P
> extends<UsableModalProp<P>> {
  addListener: ModalListener
  getParam: <N extends keyof P[M], D extends P[M][N]>(
    paramName: N,
    defaultValue?: D,
  ) => D extends P[M][N] ? P[M][N] : undefined
  removeAllListeners: () => void
  params?: P[M]
}

type ModalListener = (
  eventName: 'onAnimate',
  callback: (value?: number) => void,
) => ModalEventListener

type ModalEventListener = { remove: () => boolean }
```
{% endtab %}
{% endtabs %}

{% embed url="https://github.com/colorfy-software/react-native-modalfy/blob/master/types.ts#L332" %}
Types have been simplified for the sake of clarity. Refer to the exact definitions here.
{% endembed %}

## API reference

### `addListener`&#x20;

```typescript
addListener: ModalListener
```

Function that allows you to hook a listener to the `animatedValue` of the modal represented by the component you're in.

**Example:**&#x20;

{% tabs %}
{% tab title="React JSX" %}
{% code title="./modals/EmptyModal.js" %}
```typescript
import React from 'react'

const EmptyModal = ({ modal: { addListener }) => {
  const modalListener = React.useRef()

  const handleAnimation = (value) => {
    console.log('Modal animatedValue:', value)
  }

  React.useEffect(() => {
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

### `getParam`&#x20;

```typescript
getParam: <N extends keyof P[M], D extends P[M][N]>(
  paramName: N,
  defaultValue?: D,
) => D extends P[M][N] ? P[M][N] : undefined
```

Function that looks inside `params` and returns you the value of the key corresponding to `paramName`. Returns the provided `defaultValue` if nothing was found.

**Example:**&#x20;

{% tabs %}
{% tab title="React JSX" %}
{% code title="./modals/ErrorModal.js" %}
```javascript
import React from 'react'
import { Button, Text, View } from 'react-native'

const TITLES = {
  login: 'We could not log you in',
  signup: 'We could not create your account',
  unknown: 'An error has occurred!',
}

const ErrorModal = ({ modal: { closeModal, getParam } }) => {
  const origin = getParam('origin', 'unknown')
  const message = getParam('message', 'Something went wrong... 🤔')

  return (
    <View>
      {/* 'An error has occurred!' */}
      <Text>{TITLES[origin]}</Text>
      {/* 'Your token has expired user' */}
      <Text>{message}</Text>
      <Button onPress={closeModal} title="OK" />
    </View>
  )
}

export default ErrorModal
```
{% endcode %}
{% endtab %}
{% endtabs %}

### `removeAllListeners`&#x20;

```javascript
removeAllListeners: () => void
```

Removes all the listeners connected to the modal component you're in.

**Example:**&#x20;

```typescript
import React from 'react'

const EmptyModal = ({ modal: { addListener, removeAllListeners }) => {
  const modalListener = React.useRef()

  const handleAnimation = (value) => {
    console.log('Modal animatedValue:', value)
  }

  React.useEffect(() => {
    modalListener.current = addListener('onAnimate', handleAnimation)
    
    return () => {
      removeAllListeners()
    }
  }, [])

  return (
    //...
  )
}

export default EmptyModal
```

### `params`&#x20;

```javascript
params?: P[M]
```

Optional params you provided when opening the modal you're in.

**Example:**

{% tabs %}
{% tab title="React JSX" %}
{% code title="./modals/ErrorModal.js" %}
```javascript
import React from 'react'
import { Button, Text, View } from 'react-native'

const ErrorModal = ({ modal: { closeModal, params } }) => (
  <View>
    <Text>{params?.title}</Text>
    <Text>{params?.message}</Text>
    <Button onPress={closeModal} title="Close" />
  </View>
)

export default ErrorModal
```
{% endcode %}
{% endtab %}
{% endtabs %}
