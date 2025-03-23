
1.가독성 (읽고 느낀 생각)
  - 이름 붙이기
    - B 매직 넘버에 이름 붙이기
      - 최근에 작업한 works중에서 전체 기간동안의 데이터를 조회하는 API가 있었는데 안전한 날자를 정해놓고(2024-01-01) 그 값을 string값으로 그대로 넣어놓고 주석을 달아놨었는데 이렇게 작성하기보다는 다음과 같이 해당 값의 의미를 알 수 있도록 작성하는게 더 낫다고 생각했습니다.
            
            ```jsx
            const entirePeriod = '2024-01-01'
            const eventStartDate = '2024-01-01'
            const eventEndDate = '2024-01-01'
            ```

2. 응집도 (함께 이야기를 나누고 싶은 부분)
  - 함께 수정되는 파일을 같은 디렉토리에 두기
    - 다음과 같은 코드 구조는 atomic design을 사용하는 우리 서비스에는 적용을 하기 힘들 것 같다는 생각이 들었음.
      ```jsx
      └─ src
         │  // 전체 프로젝트에서 사용되는 코드
         ├─ components
         ├─ containers
         ├─ hooks
         ├─ utils
         ├─ ...
         │
         └─ domains
            │  // Domain1에서만 사용되는 코드
            ├─ Domain1
            │     ├─ components
            │     ├─ containers
            │     ├─ hooks
            │     ├─ utils
            │     └─ ...
            │
            │  // Domain2에서만 사용되는 코드
            └─ Domain2
                  ├─ components
                  ├─ containers
                  ├─ hooks
                  ├─ utils
                  └─ ...
      ```
