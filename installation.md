---
id: installation
title: Installation
icon: cloud-arrow-down
---

# Installation

## Bare React Native projects

**1.** Install the `react-native-modalfy` package.

{% tabs %}
{% tab title="Yarn" %}
```
yarn add react-native-modalfy
```
{% endtab %}

{% tab title="npm" %}
```bash
npm install --save react-native-modalfy
```
{% endtab %}
{% endtabs %}

**2.** Then you'll need to install and link [`react-native-gesture-handler`](https://docs.swmansion.com/react-native-gesture-handler/docs/#installation) to your project.

**3.** Wrap your app entry point into [`<GestureHandlerRootView />`](https://docs.swmansion.com/react-native-gesture-handler/docs/installation#js) from [`react-native-gesture-handler`](https://docs.swmansion.com/react-native-gesture-handler/docs/#installation).

That's it! No further action is required!

Now you can move on to actually using the library through the modal stack.

## Expo managed projects

If you're using Expo, on top of [setting up `<GestureHandlerRootView />`](https://docs.swmansion.com/react-native-gesture-handler/docs/installation#js), you'll only need to run this single command from the root of your project:

{% tabs %}
{% tab title="Bash" %}
```bash
expo install react-native-modalfy react-native-gesture-handler
```
{% endtab %}
{% endtabs %}
