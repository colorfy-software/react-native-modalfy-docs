---
description: Wednesday, June 24, 2020
---

# Announcing Modalfy v2

Exactly a year and a month ago we open-sourced React Native Modalfy. This was our solution for a problem we've been facing at colorfy for quite a while: how to easily manage lots of modals.

Quite a few commits later, we're proud to introduce Modalfy v2, completely rewritten from the ground-up with Hooks & TypeScript at its core. This new version brings long time requested features that we couldn't implement with our previous architecture. Let's go briefly through what changed with Modalfy v2!

## Sometimes we got to destroy before we elevate

Modalfy promise has always been to allow you to do 3 things:

**1.** Write your modals once, call them from anywhere in your app.\
**2.** Display several modals at once, stack as many as you like.\
**3.** Complete control over animations & transitions between each modal in the stack.

This trifecta is what motivated the creation of the library as we couldn't find anything that offered this; so that's why it's been the backbone of Modalfy since its first release.

Our initial architecture was a good representation of what the React ecosystem looked like a year ago. We used React revamped Context API to expose a Context Provider that would render modals stored in the Provider state. This approach had several benefits. The main one was allowing us to keep up our promise by enabling modals stacking and invoking those modals from anywhere in the code using a component connected to the Context thanks to the [`withModal()`](../api/withmodal.md) HOC or [`useModal()`](../api/usemodal.md) Hooks.

However, Modalfy started to face its limits when more advanced features were brought into the discussion. At colorfy, our main issue quickly became the impossibility to use Modalfy outside of React. As we just saw, Modalfy v1 architecture relies entirely on React Context API. This meant that if we're not in a modal component or in a "normal" component using `withModal()`/`useModal()`, we're outside of React Context's area, so in other words: we can't interact with the modal stack anymore.

One solution could have been to remove Context so that we wouldn't have to deal with its limitations. Unfortunately,  that wouldn't work in our case as we need it to render modals on top of everything in our apps. Given that we couldn't get rid of the whole thing, we focused our attention on the single piece of the puzzle that was blocking us: the state!

## Introducing: modalfy

With Modalfy v2, we stripped out the modals stack state outside of Context to keep it in plain JavaScript. Our Provider state now subscribes to the changes emitted by that new external state to know what modals to render. The main benefit of doing this is to have other items than Context subscribed to the changes of that external state. This is what we used to gain the ability to control our stack outside of React via Modalfy latest addition:  [`modalfy()`](../guides/outside-react.md).

{% tabs %}
{% tab title="JavaScript" %}
{% code title="./utils/logErrorWithModal.js" %}
```javascript
import { modalfy } from 'react-native-modalfy'
import crashlytics from '@react-native-firebase/crashlytics'

const { openModal } = modalfy()

export default function(origin, error) {
  console.log(error)
  
  crashlytics().recordError(error)
  
  if (!modalfy().currentModal) {
    openModal('ErrorModal', { origin, error })
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

As you can see in this little snippet, we can now interact with the same object `withModal()` and `useModal()` expose but in plain JavaScript! In this example, for instance, we can use`logErrorWithModal(origin, error)`  to log any error to our debug console & Firebase's Crashlytics and open an `'ErrorModal'`, if we don't already have any modal open.

We're really excited about what this means as you'll now be able to provide an even better experience to your users. `modalfy()` opens the door to even more complex and sophisticated use cases and we can't wait to see what you'll be able to build with it!

## TypeScript all the things

Modalfy v2 completely embraced TypeScript with its overhaul. We go more in-depth in the new [Type checking with TypeScript guide](../guides/typing.md) but there are 3 main changes worth mentioning here.

The first one is that we entirely moved away from Flow. If you're still using it, please feel free to use our current TS types definitions to provide some for Flow and submit a PR to [flow-typed](https://github.com/flow-typed/flow-typed).

The second and third changes are somewhat related: Modalfy finally supports global params typing! This means that you can now have a single TypeScript interface representing your entire modal stack and the types of the params each stack item is expecting to receive when using `modal.openModal()`. The main benefit, as you can guess, is a really really really nice autocomplete experience. Once again: the new [Type checking with TypeScript guide](../guides/typing.md) is waiting for you if you're interested in this matter. If you just want to see it in action: there is a new Expo demo project waiting for you [here](https://snack.expo.io/@charles.m/react-native-modalfy)!

## More control than ever before

Rebuilding the library allowed us to improve the existing and prepare for the upcoming. If you dive into the internals of Modalfy you'll realize that it's now based on Hooks, for instance. If you only focus on the externals, you'll discover that you can now customise your `backdropColor`, `backdropOpacity` & `backBehavior` per modal via [modalOptions](../guides/stack.md#configuring)!

On top of that Modalfy v2 brings a long-awaited feature with the fling gesture detection to close modals! Finally, the new [`modalfy.getParam()`](../guides/params.md) gives you granular access to the params you pass to your modals.

## And much more!

Please read the [changelog](https://github.com/colorfy-software/react-native-modalfy/releases/tag/v2.0.0) to have the complete list of what's different with Modalfy v2. If you're coming from Modalfy v1, check out the [Upgrading from v1.x](../guides/upgrading.md) guide.

We've been working on this new version for quite a while now and couldn't be more thrilled to finally be able to share it with you. Now it's your turn: give Modalfy v2 a try, if there is anything missing, not working or a feature you'd like to have: [please let us know](https://github.com/colorfy-software/react-native-modalfy/issues/new)!
