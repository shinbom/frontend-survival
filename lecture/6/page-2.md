# TSyringe

```
키워드

- TSyringe
- 의존성 주입(Dependency Injection)
- reflect-metadata
- singleton (싱글톤)
```

Context를 사용하는 것은 전체를 바꾸기 떄문에 비효율적일 수 있다.

> Context를 사용하는 것은 전체를 Force Update하는 것에 가깝다.

## TSyringe

> [TSyringe](https://github.com/microsoft/tsyringe)
>

> [reflect-metadata](https://github.com/rbuckton/reflect-metadata)
>

> [The problem with passing props](https://beta.reactjs.org/learn/passing-data-deeply-with-context#the-problem-with-passing-props)
>

TypeScript용 DI 도구(IoC Container). External Store를 관리하는데 활용할 수 있다. React 컴포넌트 입장에서는 “전역”처럼 여겨진다. “Prop Drilling” 문제를 우아하게 해결할 수 있는 방법 중 하나(React로 한정하면 Context도 쓸 수 있다).

---

의존성 설치

```bash
npm i tsyringe reflect-metadata
```

`src/main.tsx` 파일과 `src/setupTests.ts` 파일에서 reflect-metadata import

```tsx
import 'reflect-metadata';
```

싱글톤으로 관리할 CounterStore 클래스를 준비:

```tsx
import { singleton } from 'tsyringe';

@singleton()
class CounterStore {
 // …(중략)...
}
```

싱글톤 CounterStore 객체를 사용:

```tsx
import { container } from 'tsyringe';

const counterStore = container.resolve(CounterStore);
```

```json
    "jsx": "react-jsx", /* Specify what JSX code is generated. */
    "experimentalDecorators": true, /* Enable experimental support for TC39 stage 2 draft decorators. */
    "emitDecoratorMetadata": true, /* Emit design-type metadata for decorated declarations in source files. */

```

experimentalDecorators, emitDecoratorMetaData의 주석을 해제한다.

테스트에서 TSyringe에서 관리하는 객체를 초기화할 수 있다.

```tsx
container.clearInstances();
```

## 상태 변경 알림

Store는 어떤 식으로든 action을 처리하고, 상태가 바뀌면 연결된 컴포넌트를 forceUpdate한다.

컴포넌트는 해당 Store에서 상태를 얻어서 UI를 업데이트하게 되는데, 선언형 UI가 얼마나 편한지 절실히 느낄 수 있다.

---

### Counter Store

```tsx

// Store
import { singleton } from "tsyringe";

type Listener = () => void;

@singleton()
export default class Store {
  count = 0

  listeners = new Set<Listener>()

  publish() {
    this.listeners.forEach((listener) => {
      listener();
    })
  }

  addListener(listener: Listener) {
    this.listeners.add(listener)
  }

  removeListener(listener: Listener) {
    this.listeners.delete(listener)
  }

}

```

```tsx
// Count.tsx

import { container } from "tsyringe";

import Store from '../stores/Store'
import { useForceUpdate } from "../hooks/useForceUpdate";
import { useEffect } from "react";

export default function Counter() {
  const store = container.resolve(Store);

  const forceUpdate = useForceUpdate()

  useEffect(() => {
    store.addListener(forceUpdate);

    return () => {
      store.removeListener(forceUpdate);
    }
  }, [store, forceUpdate])

  return (
    <div>
      <p>Count : {store.count}</p>
    </div>
  )
}
```

```tsx
// CountControl.tsx

import { useForceUpdate } from "../hooks/useForceUpdate";
import { container } from "tsyringe";

import Store from '../stores/Store'

export default function Counter() {

  const store = container.resolve(Store);

  const handleClickIncrese = () => {
    store.count += 1;
    store.publish()
  }

  const handleClickDecrease = () => {
    store.count -= 1;
    store.publish()
  }

  return (
    <div>
      <button type="button" onClick={handleClickIncrese}> Increase </button>
      <button type="button" onClick={handleClickDecrease}> Decrease </button>
    </div>
  )
}
```
