# 토스 Frontend-fundamentals 읽고 난 소감

> 같이 실행되지 않는 코드

- 가독성을 생각하며, 분리하고 한눈에 알아보기 쉽게 작성하지 않은듯 ...

> 복잡한 조건에 이름 붙이기

- 기능 구현이 되는것만 생각했을 뿐, some, filter 등 코드를 읽을 때, 생각을 많이 하게 되는 부분에 대한 반성

> 시점 이동 줄이기

- 단순히 위 => 아래로 편하게 보는게 아닌, 렌더링 시, 선언한 객체 생성에 따른 메모리 사용량 까지 계산하는거에 생각하게 됨 .

```
// AS-IS
function Page() {
  const user = useUser();
  const policy = {
    admin: { canInvite: true, canView: true },
    viewer: { canInvite: false, canView: true }
  }[user.role];

  return (
    <div>
      <Button disabled={!policy.canInvite}>Invite</Button>
      <Button disabled={!policy.canView}>View</Button>
    </div>
  );
}

 =>
// TO-BE
 const USER_POLICY = {
    admin: { canInvite: true, canView: true },
    viewer: { canInvite: false, canView: true }
  }

function Page() {
  const user = useUser();
  const policy = USER_POLICY[user.role];
  ...
}
```

> 중복 코드 허용

- 무조건 중복이 나쁘다는 편견이 있었는데, 필요하다면 사용해도 되는거에 대해 고민하게 됨.
- 각 상황에 따라 다르면 어떻게 할것인가, 설계할때 지속적인 고민이 필요하게 될 듯 ...

> Props Drilling 지우기

- 무분별한 props drilling 하지 않기 위해, 다음과 같은 방법을 어떨까 의견

- getStateProps 에 대한 예측이 힘들어 useSometingHook 을 한번 타고 봐야 하는 불편함이 있지만, 다량의 props를 전달 할 수 있기 때문에,
  상황에 따라 사용하는것도 좋다는 생각이 듬.

```
 const {
    state,
    getStateProps,
    ...
  } = useSometingHook({
    ...
  });

  return (
    <SpecificComponent {...getStateProps()}>
       ...
    </SpecificComponent>)
```
