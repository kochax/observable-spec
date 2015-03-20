# Observable

**An open standard for sound, interoperable JavaScript Observables.**

An *Observable* is an object that can send multiple values to a consumer.  An Observable has a `subscribe` method accepts a generator and send it any number of values.

This specification details the behavior of the `subscribe` method, providing an interoperable base which all conformant Observable implementations can be depended on to provide. As such, the specification should be considered very stable. Although the Observable organization may occasionally revise this specification with minor backward-compatible changes to address newly-discovered corner cases, we will integrate large or backward-incompatible only after careful consideration, discussion, and testing.

The core Observable specification does not deal with how to create Observables, Generators, or Subscriptions, choosing instead to focus on providing an interoperable a `subscribe` method. Future work in companion specifications may touch on these subjects.

## Terminology

1. "observable" is an object or function with a `subscribe` method whose behavior conforms to this specification.

## Requirements

### The `subscribe` Method

An observable must provide a `subscribe` method to allow consumers to receive its values. An observable's `subscribe` method accepts one argument and returns one value:

```js
var subscription = observable.subscribe(generator);
```

1. The generator is a required argument.
    1. It must be an object.
1. If generator is an object with a `next` method.
    1. it must never be called after the `dispose` method on the subscription generated by `subscribe` has been called.
    1. it may optionally be passed a value.
    1. it may optionally return a value when called.
    1. if it returns an object with a `done` value of true, the `dispose` method on the subscription generated by the call to `subscribe` must be called.
1. If generator is an object with a `throw` method.
    1. it must never be called after the `dispose` method on the subscription generated by `subscribe` has been called.
    1. otherwise it must be called if an error thrown anywhere in code either executed synchronously or asynchronously by observable. The error must be passed as the argument to the function.
    1. it may optionally return a value when called.
    1. after it is called, the `dispose` method on the subscription generated by the call to `subscribe` must be called.
1. If generator is an object with a `return` method.
    1. it must never be called after the `dispose` method on the subscription generated by `subscribe` has been called.
    1. it may be called with a value designated as the return value.
    1. it must be called to indicate that the observable will send no further values.
    1. it may optionally return a value when called.
    1. after it is called, the `dispose` method on the subscription generated by the call to `subscribe` must be called.

1. `subscribe` must return an subscription.
    1. it must be an object with a dispose method.
    1. a call to the `dispose` method should free all references to the generator held by the observable.
    1. the `dispose` method may be called multiple times. However every subsequent call after the first one should not have any side effects.

---

<p xmlns:dct="http://purl.org/dc/terms/" xmlns:vcard="http://www.w3.org/2001/vcard-rdf/3.0#">
  <a rel="license"
     href="https://creativecommons.org/publicdomain/zero/1.0/">
    <img src="https://i.creativecommons.org/p/zero/1.0/88x31.png" style="border-style: none;" alt="CC0" />
  </a>
  <br />
  To the extent possible under law,
  <a rel="dct:publisher"
     href="https://github.com/observable-spec">
    <span property="dct:title">the Observable organization</span></a>
  has waived all copyright and related or neighboring rights to
  <span property="dct:title">Observable Specification</span>.
This work is published from:
<span property="vcard:Country" datatype="dct:ISO3166"
      content="US" about="https://github.com/observable-spec">
  United States</span>.
</p>
