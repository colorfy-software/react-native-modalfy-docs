# ModalComponentWithOptions

{% hint style="info" %}
Interface for a React component containing its props and the `modalOption` static property.
{% endhint %}

{% hint style="danger" %}
**Note:** Only use with **Hooks modal components** (present in your `createModalStack()`'s config). If you're working with a **Class modal component**, you can directly use `static modalOptions: ModalOptions.`
{% endhint %}

{% tabs %}
{% tab title="TypeScript" %}
```typescript
export type ModalComponentWithOptions<
  Props = unknown
> = React.ComponentType<Props> & {
  modalOptions?: ModalOptions
}
```
{% endtab %}
{% endtabs %}

{% embed url="https://github.com/colorfy-software/react-native-modalfy/blob/master/types.ts#L360" %}
