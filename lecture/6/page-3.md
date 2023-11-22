# Redux

```text
키워드

- Redux
- Reflect
```

```typescript
state = {};

reducer = (state, action) => {
  return {
    ...state,
  };
};
```

Reducer는 기존에 있던 state과 action을 받음

---

## Action Create Function

```typescript
function increase() {
  return {
    type: "increase",
  };
}

// JSX
<button type="button" onClick={() => dispatch(increase())}>
  Increase
</button>;
```

## Selector

```typescript
type Selector<T> = (state: State) => T;

function useSelector<T>(selector: Selector<T>): T {
  const store = useStore();
  return selector(store.state);
}

...

const count = useSelector((state) => state.count);

```

## Reflect
