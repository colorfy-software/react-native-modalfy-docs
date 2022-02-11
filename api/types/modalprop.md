# ModalProp

{% hint style="info" %}
Interface of the `modal` prop exposed by the library to regular components.
{% endhint %}

{% tabs %}
{% tab title="TypeScript" %}
```typescript
type ModalProp<P extends ModalfyParams, Props = unknown> = Props & {
  modal: UsableModalProp<P>
}

// ------------------ INTERNAL TYPES ------------------ //

type ModalfyParams = { [key: string]: any }

interface UsableModalProp<
  P extends ModalfyParams,
  M extends Exclude<keyof P, symbol | number> = Exclude<
    keyof P,
    symbol | number
  >
> {
  closeAllModals: () => void
  
  closeModal: (modalName?: M) => void
    
  closeModals: (modalName: M) => boolean
  
  currentModal: M | null
  
  openModal: (modalName: M, params?: P[M]) => void
}
```
{% endtab %}
{% endtabs %}

{% embed url="https://github.com/colorfy-software/react-native-modalfy/blob/master/types.ts#L310" %}
Types have been simplified for the sake of clarity. Refer to the exact definitions here.
{% endembed %}

## API reference

### `closeAllModals`&#x20;

```javascript
closeAllModals: () => void
```

This function closes every open modal.

**Example:** `modal.closeAllModals()`

### `closeModal`&#x20;

```javascript
closeModal: (modalName?: M) => void
```

This function closes the currently displayed modal by default. Incidentally, you can also provide a `modalName` if you want to close a different modal than the latest opened.

**Example:** `modal.closeModal()`

### `closeModals`&#x20;

```javascript
closeModals: (modalName: M) => boolean
```

This function closes all the instances of a given modal. You can use it whenever you have the same modal opened several times, to close all of them at once.

**Example:** `modal.closeModals('ErrorModal')`

**Returns:** boolean indicating whether or not Modalfy found any open modal corresponding to the provided `modalName` (and then closed them).

### `currentModal`&#x20;

```typescript
currentModal: M | null
```

This value returns the current open modal (`null` if none).

**Example:** `modal.currentValue`

### `openModal`&#x20;

```typescript
openModal: (modalName: M, params?: P[M]) => void
```

This function opens a modal based on the provided `modalName`. It will look at the stack passed to `<ModalProvider>` and add the corresponding component to the current stack of open modals. Alternatively, you can also provide some `params` that will be accessible to that component.

**Example:** `modal.openModal('PokedexEntryModal', { id: 619, name: 'Lin-Fu' })`
