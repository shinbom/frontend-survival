# 장바구니에 상품 담기

`Product`와 관련된 `Option` 정보, 수량 등 다양한 조합이 조합되어 `Cart Item`을 구성하는 것을 장바구니에 상품을 담는다는 의미가 된다.

> 프론트엔드와 백엔드가 같이 타입을 논의하면서, 진행하는 것이 좋다.

---

## AddToCartForm 컴포넌트

1. 옵션을 보여주고, 선택할 수 있다.
2. 수량을 정하게 한다.(기본값 : 1)
3. 수량에 맞는 비용을 보여준다.
4. 장바구니에 담기 버튼이 있고, 버튼 실행 시 장바구니에 담았다는 메세지가 나타난다.

Props Drilling을 피하기 위해 Store에서 가져와 사용하도록 했다.

```tsx
export default function AddToCartForm() {
  return (
    <div>
      <Options />
      <Quantity />
      <Price />
      <SubmitButton />
    </div>
  );
}
```

> Store를 잘 관리하는 것이 중요하다.

> 역할에 맞게 분리하여 구성하는 것이 좋다.

> 비즈니스 로직을 구분하므로써, 테스트도 더 간편해진다.

---

## Immutatble(불변성)

[Array를 Immutable하게 변경하기](https://github.com/ahastudio/til/blob/main/javascript/immutable-array.md)

---

## 비즈니스 로직

비즈니스 로직(Business logic)은 컴퓨터 프로그램에서 실세계의 규칙에 따라 데이터를 생성·표시·저장·변경하는 부분을 일컫는다.

이 용어는 특히 데이터베이스, 표시장치 등 프로그램의 다른 부분과 대조되는 개념으로 쓰인다.

---

## 테스트

유닛테스트로는 개별적인 요소가 잘 완성되었는지 확인이 가능하며, E2E테스트를 통해 맡겨진 업무가 잘 끝낫는지 확인하면 된다.

---

## 리액트 디자인 패턴
