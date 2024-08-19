### Async components can now control when they are hydrated by providing a hydration strategy.

Vue provides a number of built-in hydration strategies. These built-in strategies need to be individually imported so they can be tree-shaken if not used.

The design is intentionally low-level for flexibility. Compiler syntax sugar can potentially be built on top of this in the future either in core or in higher level solutions (e.g. Nuxt).

Why is it coupled to async components? Mostly because async components are already natural boundaries for async operations inside a render tree. Async sub trees require special handling in regards of reactivity tracking and suspense resolution. By coupling async hydration to async components we can support the feature without introducing extra complexities when it interacts with those other areas of concerns.

The result of this coupling is that one can't have a component that is eagerly loaded but lazily hydrated - which seems illogical to begin with.

#### Hydrate on Idle
```ts
Hydrates via requestIdleCallback:

import { defineAsyncComponent, hydrateOnIdle } from 'vue'

const AsyncComp = defineAsyncComponent({
  loader: () => import('./Comp.vue'),
  hydrate: hydrateOnIdle(/* optionally pass a max timeout */)
})
```
#### Hydrate on Visible
Hydrate when element(s) become visible via IntersectionObserver.

```ts
import { defineAsyncComponent, hydrateOnVisible } from 'vue'

const AsyncComp = defineAsyncComponent({
  loader: () => import('./Comp.vue'),
  hydrate: hydrateOnVisible()
})
```
#### Can optionally pass in an options object value for the observer:

```ts
hydrateOnVisible({ rootMargin: '100px' })
Hydrate on Media Query
Hydrates when the specified media query matches.

import { defineAsyncComponent, hydrateOnMediaQuery } from 'vue'

const AsyncComp = defineAsyncComponent({
  loader: () => import('./Comp.vue'),
  hydrate: hydrateOnMediaQuery('(max-width:500px)')
})
```
#### Hydrate on Interaction
Hydrates when specified event(s) are triggered on the component element(s). The event that triggered the hydration will also be replayed once hydration is complete.
```ts

import { defineAsyncComponent, hydrateOnInteraction } from 'vue'

const AsyncComp = defineAsyncComponent({
  loader: () => import('./Comp.vue'),
  hydrate: hydrateOnInteraction('click')
})
Can also be a list of multiple event types:

hydrateOnInteraction(['wheel', 'mouseover'])
```
#### Custom Strategy
```ts
import { defineAsyncComponent, type HydrationStrategy } from 'vue'

const myStrategy: HydrationStrategy = (hydrate, forEachElement) => {
  // forEachElement is a helper to iterate through all the root elememts
  // in the component's non-hydrated DOM, since the root can be a fragment
  // instead of a single element
  forEachElement(el => {
    // ...
  })
  // call `hydrate` when ready
  hydrate()
  return () => {
    // return a teardown function if needed
  }
}

const AsyncComp = defineAsyncComponent({
  loader: () => import('./Comp.vue'),
  hydrate: myStrategy
})
```
