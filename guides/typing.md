# Type checking with TypeScript

TypeScript is now supported by default as our type system of choice: Modalfy v2 has been completely rewritten with it. We decided to move away from Flow as we considered that a good majority of the community is leaning towards TypeScript and decided to make the jump.

Our main focus for this new version has been to provide a developer experience as enjoyable as we could. So if you use an IDE with IntelliSense support, you might appreciate how simple it's going to be to type functions/variables and get some nice autocomplete.

There are **6** main interfaces that you'll use throughout your experience with Modalfy:

* **ModalStackConfig** - Interface of the modal stack configuration (needed once).
* **ModalOptions** - Interface of the modal configuration options.
* **ModalProp -** Interface of the `modal` prop exposed by the library.
* **ModalComponentProp** - Interface of the `modal` prop exposed by the library (specifically for modal components).
* **ModalComponentWithOptions** - Interface that adds type support of the `modalOptions` property (specifically for Hooks modal components).

The 6th and last main interface will actually be provided **by you**, as Modalfy v2 finally brings support for modal params type. That interface is going to be used mainly by [**ModalProp**](../api/types/modalprop.md) , [**ModalComponentProp**](../api/types/modalcomponentprop.md) and [**modalfy()**](../api/modalfy.md). But for now: let's see how to use the other interfaces we just mentioned.

{% hint style="info" %}
Please refer to the [**Types**](../api/types/) section of the API reference to get a complete overview of each of these interfaces.
{% endhint %}

## **ModalStackConfig & ModalOptions** <a href="#config-and-options" id="config-and-options"></a>

#### ****[**> ModalStackConfig API**](../api/types/modalstackconfig.md)****

#### ****[**> ModalOptions API**](../api/types/modaloptions.md)****

The interfaces should be the one you use the less. They're the ones that will ensure the type safety of the 2 arguments `createModalStack()` You remember about 6th, modal params, interface? That's exactly what we have  expects. So if we were to reuse the same initial example we say in the [**Creating a stack section**](stack.md), we'd now have:

{% tabs %}
{% tab title="TypeScript" %}
{% code title="./App.tsx" %}
```javascript
import React from 'react'
import {
  ModalOptions,
  ModalStackConfig,
  createModalStack,
  ModalProvider,
} from 'react-native-modalfy'

import Navigation from './navigation'
import { ErrorModal } from './components/Modals'

const modalConfig: ModalStackConfig = { ErrorModal }
const defaultOptions: ModalOptions = { backdropOpacity: 0.6 }

const stack = createModalStack(modalConfig, defaultOptions)

const App = () => (
  <ModalProvider stack={stack}>
    <Navigation />
  </ModalProvider>
)


export default App
```
{% endcode %}
{% endtab %}
{% endtabs %}

And from there, the type checker will get to work and let you know if you're doing something wrong.

{% hint style="info" %}
If you directly provide the 2 objects instead of using variables like so:

```javascript
createModalStack({ ErrorModal }, { backdropOpacity: 0.6 })
```

No need to use these 2 interfaces as`createModalStack()`is already doing it under the hood.
{% endhint %}

## ModalProp

#### [> ModalProp API](../api/types/modalprop.md)

This interface allows you to type check the `modal` prop that your regular component will get access to by using `withModal()` HOC. This means that you'll have to keep a few things in mind:

{% hint style="danger" %}
* If you're inside **a modal component and not a "regular" component,** you should use **`ModalComponentProp` instead**.
* If you're using ** `useModal()` Hook,** no need to employ`ModalProp`**a**s the Hook itself will take care of all the typing. **Simply provide your params interface to the Hook as such `useModal<ModalStackParams>()`**(explained below)**.**
* The main and potentially **only use case for** `ModalProp` **then is when you're using a Class component**
{% endhint %}

Now that we've covered the gotchas, let's see `ModalProp` in action. In this example, we created a `<PokedexCard>` component that's will open a modal with the full details about a specific Pokemon, with its name, type and entry number in the [Pokédex](https://www.pokemon.com/pokedex):

{% tabs %}
{% tab title="Class" %}
{% code title="./components/PokedexCard.tsx" %}
```typescript
import React from 'react'
import { Text, TouchableOpacity, View } from 'react-native'
import { ModalProp, withModal } from 'react-native-modalfy'

import { ModalStackParams } from 'App'

interface OwnProps {
  pokemon: ModalStackParams['PokedexEntryModal']['name']
  type: ModalStackParams['PokedexEntryModal']['type']
  id: ModalStackParams['PokedexEntryModal']['id']
}

type Props = ModalProp<ModalStackParams, OwnProps>

class PokedexCard extends React.Component<Props> {
  onPress = () => {
    const {
      modal: { openModal },
      pokemon,
      type,
      id,
    } = this.props

    openModal('PokedexEntryModal', { id, name: pokemon, type })
  }

  render() {
    const { id, pokemon, type } = this.props
    return (
      <TouchableOpacity onPress={this.onPress}>
        <View>
          <Text>Nr. {id}Text>
          <Text>{pokemon}</Text>
          <Text>{type}</Text>
          <Text>Show more</Text>
        </View>
      </TouchableOpacity>
    )
  }
}

export default withModal(PokedexCard)
```
{% endcode %}
{% endtab %}

{% tab title="Hooks" %}
```
// ❌ Don't use ModalProp with the useModal() Hook. If you want to fully type it
// simply proviede your params interface as such: useModal<ModalStackParams>() 
// (ModalStackParams is explained below).
```
{% endtab %}
{% endtabs %}

Lots of things are happening in this snippet, but if you're already familiar with [TypeScript generics](https://www.typescriptlang.org/docs/handbook/generics.html), this should get you excited! Let's dissect this snippet.

`L#5` with `ModalStackParams`. It's an interface you'll have to build that will to represent the complete tree of your modals and the types their params are expecting.&#x20;

From `L#7` to `L#11`, we're letting TypeScript know that  `<PokedexCard>` expects 3 props that should comply with the types specified in `ModalStackParams`. We're doing this to ensure the type safety of these 3 props because we're using them `L#24` to open `'PokedexEntryModal'` and pass them as params.

If we were to write `ModalStackParams`, we can now _guesstimate_ that it could look something like this a minima:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
interface ModalStackParams {
  PokedexEntryModal: {
    name: string
    type: string
    id: number
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
You can have a look at the Example provided [in the repository](https://github.com/colorfy-software/react-native-modalfy/tree/master/Example) and available [on Expo](https://snack.expo.io/@charles.m/react-native-modalfy) to see what `ModalStackParams`could look like/be used in a real-world scenario.
{% endhint %}

You'd also realize that we didn't pass `ModalStackParams` as a generic to `withModal()` `L#42`, instead, we directly provided it to `React.Component` `L#15`, via `Props` created `L#13`. As you may know, in TypeScript, `React.Component` is a [generic class](https://www.typescriptlang.org/docs/handbook/generics.html#generic-classes) that accepts up to 2 arguments: `React.Component<Props, State>`. That's why `ModalProp` also accepts up to 2 arguments, your params interface and your component props and returns a type with your props type + the new `modal` prop. There a few things to notice here:

* If you have any `State` interface, you'll have to provide it to `React.Component` as a second argument, not `ModalProp`.
* If your component doesn't expect any props, you don't have to provide a second argument to `ModalProp`. If you want, you can even use it without providing the params type. This means that the most basic way of using `ModalProp` is `class PokedexCard extends React.Component<ModalProp>`
* On the contrary, providing your params types to `ModalProp` gives you access to some sweet autocompleting experience (try to see what you get when you trigger it on `openModal()` for instance)!

## **ModalComponentProp**

#### ****[**> ModalComponentProp API**](../api/types/modalcomponentprop.md)

This interface works on the same principles as `ModalProp` with just some key differences to keep in mind. The first and most important is:

{% hint style="danger" %}
&#x20;`ModalComponentProp` **should only be used with modal components (rendered by Modalfy)!**
{% endhint %}

&#x20;**** If the component you're working on is not rendered by Modalfy directly/part of your `createModalStack()` config, you should use `ModalProp` instead.

Given that we're in a specific modal component, `ModalComponentProp` accepts a 3rd argument, corresponding to the name of the modal represented by this component. If we reuse our Pokédex example, `'PokedexEntryModal'` file could look like:

{% tabs %}
{% tab title="Hooks" %}
{% code title="./modals/PokedexEntryModal.tsx" %}
```typescript
import React from 'react'
import { ModalComponentProp } from 'react-native-modalfy'

import { ModalStackParams } from 'App'

type Props = ModalComponentProp<
  ModalStackParams,
  void,
  'PokedexEntryModal',
>

const PokedexEntryModal = (props: Props) => {
  return (
    // ...
  )
}

export default PokedexEntryModal
```
{% endcode %}
{% endtab %}

{% tab title="Class" %}
{% code title="./modals/PokedexEntryModal.tsx" %}
```typescript
import React from 'react'
import { ModalComponentProp } from 'react-native-modalfy'

import { ModalStackParams } from 'App'

type Props = ModalComponentProp<
  ModalStackParams,
  void,
  'PokedexEntryModal',
>

class PokedexEntryModal extends React.Component<Props> {
  render() {
    return (
      // ...
    )
  }
}

export default PokedexEntryModal
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Given that you can reuse the same component for several modals, you can replace that 3rd argument with a [union type](https://www.typescriptlang.org/docs/handbook/unions-and-intersections.html#union-types) to make everything work! ie:&#x20;

```typescript
type Props = ModalComponentProp<
  ModalStackParams,
  void,
  'PokedexEntryModal' | 'FavouritePokemonModal',
>
```
{% endhint %}

Although you'll never manually render `<PokedexEntryModal>`yourself, `ModalComponentProp` voluntarily expects props types as its second argument as your modal component could be getting props from some HOCs. ie:

{% tabs %}
{% tab title="Class" %}
{% code title="./modals/PokedexEntryModal.tsx" %}
```typescript
// ...

import { connect } from 'react-redux'

// ...

import { ReduxState } from 'App'

interface OwnProps {
  favouritePokemon: ReduxState['user']['favouritePokemon']
}

type Props = ModalComponentProp<
  ModalStackParams,
  OwnProps,
  'PokedexEntryModal',
>

class PokedexEntryModal extends React.Component<Props> {
  //...
}

const mapStateToProps = (state: ReduxState): OwnProps => {
  favouritePokemon: state.user.favouritePokemon
}

export default connect(mapStateToProps)(PokedexEntryModal)
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Please check out the [**ModalComponentProp**](../api/types/modalcomponentprop.md) API reference to have an exhaustive list of what it brings with it.
{% endhint %}

## **ModalComponentWithOptions**

#### ****[**> ModalComponentWithOptions API**](../api/types/modalcomponentwithoptions.md)****

{% hint style="danger" %}
`ModalComponentWithOptions` **is only meant to be used with Hooks modal components**. **** If you're working with classes, simply use the **** static`modalOptions`property as explained below.
{% endhint %}

As we saw in the [**Configuring a stack**](stack.md#configuring-the-stack) guide, you have 3 different ways to provide options to a modal. While the first 2 are type-checked during the modal stack creation, only the 3rd one involves typing `modalOptions` from within the modal component itself.&#x20;

To do so, simply pass your component props to `ModalComponentWithOptions` and you're done! The interface will also directly take care of the fact that you're using it on a component, so no need to use `React.FC` with it. ie:

{% tabs %}
{% tab title="Hooks" %}
{% code title="./modals/PokedexEntryModal.tsx" %}
```typescript
import React from 'react'
import {
  ModalComponentWithOptions,
  ModalComponentProp,
} from 'react-native-modalfy'

import { ModalStackParams } from 'App'

type Props = ModalComponentProp<
  ModalStackParams,
  void,
  'PokedexEntryModal',
>

const PokedexEntryModal: ModalComponentWithOptions<Props> = () => {
  return (
    // ...
  )
}

PokedexEntryModal.modalOptions = {
  backdropColor = "rebeccapurple"
}

export default PokedexEntryModal
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Notice how`ModalComponentWithOptions<Props>` is used right after the modal variable name, not inside the parenthesis of the arrow function!
{% endhint %}

If you're working with a class, you'll just have to directly type the static`modalOptions`property with the same`ModalOptions`we used to type our modal stack. ie:

{% tabs %}
{% tab title="Class" %}
{% code title="./modals/PokedexEntryModal.tsx" %}
```typescript
import React from 'react'
import {
  ModalComponentProp,
  ModalOptions
} from 'react-native-modalfy'

import { ModalStackParams } from 'App'

type Props = ModalComponentProp<
  ModalStackParams,
  void,
  'PokedexEntryModal',
>

class PokedexEntryModal extends React.Component<Props> {
  static modalOptions: ModalOptions = {
    backdropColor: 'rebeccapurple'
  }

  render() {
    return (
      // ...
    )
  }
}

export default PokedexEntryModal
```
{% endcode %}
{% endtab %}
{% endtabs %}
