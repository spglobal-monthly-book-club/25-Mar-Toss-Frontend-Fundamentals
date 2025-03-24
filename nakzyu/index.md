문서를 쭉 한번 읽어보고 다른분들의 의견이 궁금한 부분을 정리했습니다.

## 가독성

### 맥락 줄이기 - 같이 실행되지 않는 코드 분리

```tsx
function SubmitButton() {
  const isViewer = useRole() === "viewer";

  useEffect(() => {
    if (isViewer) {
      return;
    }
    showButtonAnimation();
  }, [isViewer]);

  return isViewer ? (
    <TextButton disabled>Submit</TextButton>
  ) : (
    <Button type="submit">Submit</Button>
  );
}
```

위와같은 코드를 아래처럼 분리한다면, 각 컴포넌트 책임에 맞게 훅과 함수가 불리된다.

```tsx
function SubmitButton() {
  const isViewer = useRole() === "viewer";

  return isViewer ? <ViewerSubmitButton /> : <AdminSubmitButton />;
}

function ViewerSubmitButton() {
  return <TextButton disabled>Submit</TextButton>;
}

function AdminSubmitButton() {
  useEffect(() => {
    showAnimation();
  }, []);

  return <Button type="submit">Submit</Button>;
}
```

- 정리된 코드를 하나의 파일로 본다면 파일을 어떤식으로 분리할것인가?
  - `SubmitButton` `ViewerSubmitButton` `AdminSubmitButton` 를 각각의 파일로 만든다.
  - `SubmitButton` 안에 셋 모두를 선언한 후 `SubmitButton`만 export한다.
    - 나는 개인적으로 이게 더 좋은거같다. 코드를 한눈에 보기가 쉬워서.. but 코드 길이가 길어진다면...? 코드 내용 자체가 짧은경우에는 허용되어도 되는걸까..?

## 응집도

### 함께 수정되는 파일을 같은 디렉토리에 두기

- 도메인별로 파일을 수정하도록 하기.
- 작업 할때 최대한 가까운 디렉토리에 파일들이 쌓이는게 편하더라. 당장 작업할때 뿐만 아니고 유지보수에도 시간이 덜 든다고 느낌.
- 댓글중에 `ssi02014` 님이 쓴 `캐시의 지역성` 관련 이야기가 있는데, 왜 가까운 디렉토리에 두는게 당장 작업하기도 편하고 유지보수할때도 좋은가?에 대해서 명확히 설명하기 어려웠는데, 이걸로 어느정도 설명이 되는듯. 한번 읽어보시기를..

## 결합도

### Props Drilling 지우기

- 조합(Composition)을 활용하여 props 드릴링 최소화하기
- 컴포넌트를 설계할때부터 무슨 책임을 할것지가 명확해야 단순히 `children`을 넘겨주는 형태로 짤 수 있음
  - 처음에 못하더라도 중간중간 이것을 Composition 형태로 변경할 수 있을까?에 대한 고민을 수시로 해야하는듯.. 계속 어떻게 해야겠다를 생각하지 않으면 놓치기 쉬운 부분인거같음
