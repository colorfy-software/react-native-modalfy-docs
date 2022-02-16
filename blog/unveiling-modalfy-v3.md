---
description: Tuesday, February 15, 2022
---

# Unveiling Modalfy v3

Almost two years ago we released Modalfy v2. Amongst other things, the new version came with a complete rewrite to TypeScript and a brand new [**`modalfy()`**](../api/modalfy.md) API that now allows you to use Modalfy outside of React.&#x20;

We're back today with a new major version that brings a lot of long-requested fixes and features!

## Web support!

{% embed url="https://streamable.com/eh7g5p" %}
Web support in action with Modalfy v3
{% endembed %}

This feature has been [a long time coming](https://github.com/colorfy-software/react-native-modalfy/issues/38) and finally, starting with Modal v3, the library supports React Native Web as well. There is a demo app [available on Expo](https://snack.expo.io/@charles.m/react-native-modalfy) for you to give it a try. Massive props here to [@jakobo](https://twitter.com/jakobo) who did a lot of the groundwork that helped assess the feature feasibility as well as identify some edge cases! We're really excited to see Modalfy allow you to target different platforms with more ease and consolidate your codebases.

## Actions callback

Another feature that has been in the works [for quite some time as well](https://github.com/colorfy-software/react-native-modalfy/issues/32) was the ability to invoke a callback after the stack was done animating modals. Now, with Modalfy v3, each function has received a new argument where a callback can be invoked:

```typescript
openModal('ErrorModal', { message: '😅 Something went wrong...' }, () => {
  console.log('✅ Opened ErrorModal')
})
    
closeModal(undefined, () => console.log('✅ Closed latest opened modal'))

closeModals('ErrorModal', () => console.log('✅ Closed all ErrorModal'))

closeAllModals(() => console.log('✅ Closed all modals'))
```

## Closing animations fixes

Modalfy v2 brought new ways to animate your modals in and out ([**`animationIn`**](../api/types/modaloptions.md#animationin), [**`animationOut`**](../api/types/modaloptions.md#animationout), [**`animationInConfig`**](../api/types/modaloptions.md#animateinconfig), [**`animationOutConfig`**](../api/types/modaloptions.md#animateoutconfig), etc). Unfortunately, in some instances, closing your modals was not triggering the expected behavior: the modal(stack) was just [disappearing abruptly](https://github.com/colorfy-software/react-native-modalfy/issues/26). These issues have been addressed in Modalfy v3. You can now expect your modals & the backdrop to animate as expected, no matter which Modalfy method you're calling.

## And so much more...

Some of these modifications required us to make breaking changes, hence the new major version. That's why we'd suggest you go over the [**Upgrading from v2.x**](../guides/upgrading.md) guide to get yourself up to date.

We hope Modalfy v3 addresses most of the issues that users have been facing with the v2 (or even v1), while allowing them to do even more now, with Web support.&#x20;

Give Modalfy v3 a try, if there is anything missing, not working or a feature you'd like to have: [please let us know](https://github.com/colorfy-software/react-native-modalfy/issues/new)!

