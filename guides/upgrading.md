# Upgrading from v2.x

We tried to keep the number of breaking changes as low as possible. As of now, here is the list of the changes we know you'll have to make if you're coming from Modalfy v2.

{% hint style="info" %}
If you're coming from the v1 API, we'd highly suggest going through the [Upgrading from v1.x](https://colorfy-software.gitbook.io/react-native-modalfy/v/2.x/guides/upgrading) guide first.
{% endhint %}

### `ModalOptions.shouldAnimateOut` has been dropped

As announced in the v2 release, the `shouldAnimateOut` modal option has been dropped after being deprecated since June 2020. If you haven't transitioned yet, you can use [**`animationOut`**](../api/types/modaloptions.md#animationout) or [**`animationConfigOut`**](../api/types/modaloptions.md#animateoutconfig) instead.

### `ModalOptions.animateOut`: new mandatory `callback` argument

v2.1 brought up a new way to drive animations by putting you in full control whenever a modal is being opened and/or closed: [**`animationIn`**](../api/types/modaloptions.md#animationin) & [**`animationOut`**](../api/types/modaloptions.md#animationout).

Since v3, you _**must**_** ** use the new 3rd `callback` argument provided to you. This is required so that [your callbacks can be invoked](triggering-a-callback.md) at the end of the animation(s) you'll define, for instance. Example:

{% tabs %}
{% tab title="TypeScript" %}
{% code title="src/App.tsx" %}
```typescript
import { Animated } from 'react-native'
import type { ModalOptions } from 'react-native-modalfy'

const animate = (
  animatedValue: Animated.Value,
  toValue: number,
  callback?: () => void,
) => {
  Animated.spring(animatedValue, {
    toValue,
    damping: 10,
    mass: 0.35,
    stiffness: 100,
    overshootClamping: true,
    restSpeedThreshold: 0.001,
    restDisplacementThreshold: 0.001,
    useNativeDriver: true,
  }).start(({ finished }) => {
    if (finished) callback?.()
  })
}

const defaultOptions: ModalOptions = {
  animationIn: animate,
  animationOut: animate,
  transitionOptions: (animatedValue) => ({
    opacity: animatedValue.interpolate({
      inputRange: [0, 1, 2],
      outputRange: [0, 1, 0.9],
    }),
  }),
}

// ...
```
{% endcode %}
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Invoking`callback`is _required_ if you are using[**`animationOut`**](../api/types/modaloptions.md#animationout)in particular. That's because, since v3, Modalfy relies on that callback being invoked to mark a modal as closed in its own internal state. That's why`callback` is an optionally provided argument for [**`animationIn`**](../api/types/modaloptions.md#animationin) but always provided for[**`animationOut`**](../api/types/modaloptions.md#animationout).
{% endhint %}

**And that's it!** Everything else should just work as expected, enjoy! :partying\_face:&#x20;
