# 1....

## 리컴포지션 이란?

- Compose 앱은 구성 가능한 함수를 호출하여 데이터를 UI로 변환합니다. 데이터가 변경되면 Compose는 새 데이터로 이러한 함수를 다시 실행하여 업데이트된 UI를 만듭니다. 이를 리컴포지션이라고 합니다.

### 컴포즈가 변경되지 않은 UI 부분을 처리하는 방식

- 또한, Compose는 데이터가 변경된 구성요소만 다시 구성하고 영향을 받지 않는 구성요소는 다시 구성하지 않고 건너뛰도록 개별 컴포저블에서 필요한 데이터를 확인합니다.

### 주의 할 점

- 구성 가능한 함수는 자주 실행될 수 있고 순서와 관계없이 실행될 수 있으므로 코드가 실행되는 순서 또는 이 함수가 다시 구성되는 횟수에 의존해서는 안 됩니다.

## State 와 MutableState 란?

- State 및 MutableState는 어떤 값을 보유하고 그 값이 변경될 때마다 UI 업데이트(리컴포지션)를 트리거하는 인터페이스입니다.

## remember 로 상태 기억 하기

- 여러 리컴포지션 간에 상태를 유지하려면 remember를 사용하여 변경 가능한 상태를 기억해야 합니다.
- remember는 리컴포지션을 방지하는 데 사용되므로 상태가 재설정되지 않습니다.
- ex) 상위 컴포저블 에서 커스텀 컴포저블이 불필요하게 리컴포지션 될 수 있는 것을 막아준다.

### 상태 버전을 가진 UI 요소

- 화면의 서로 다른 부분에서 동일한 컴포저블을 호출하는 경우 자체 상태 버전을 가진 UI 요소를 만듭니다. 내부 상태는 클래스의 비공개 변수로 보면 됩니다.

### 상태 구독

- 구성 가능한 함수는 상태를 자동으로 '구독'합니다. 상태가 변경되면 이러한 필드를 읽는 컴포저블이 재구성되어 업데이트를 표시합니다.

## 상태 호이스팅

- 구성 가능한 함수에서 여러 함수가 읽거나 수정하는 상태는 공통의 상위 항목에 위치해야 합니다. 이 프로세스를 상태 호이스팅이라고 합니다. 호이스팅이란 들어 올린다 또는 끌어올린다라는 의미입니다.
- 상태 값을 상위 요소와 공유하는 대신 상태를 호이스팅
  - 즉, 상태 값에 액세스해야 하는 공통 상위 요소로 상태 값을 이동하기만 하면 됩니다.
### 상태 호이스팅의 장점

- 상태를 호이스팅할 수 있게 만들면 상태가 중복되지 않고
- 버그가 발생하는 것을 방지할 수 있으며 
- 컴포저블을 재사용할 수 있고 
- 훨씬 쉽게 테스트할 수 있습니다.

### 주의

- 이에 반하여, 컴포저블의 상위 요소에서 제어할 필요가 없는 상태는 호이스팅되면 안 됩니다. 정보 소스는 상태를 생성하고 관리하는 대상에 속합니다.

## by

- shouldShowOnboarding은 = 대신 by 키워드를 사용하고 있습니다. 이 키워드는 매번 .value를 입력할 필요가 없도록 해주는 속성 위임입니다.


## Compose에서는 UI 요소를 숨기지 않습니다. 
- 대신, 컴포지션에 UI 요소를 추가하지 않으므로 Compose가 생성하는 UI 트리에 추가되지 않습니다.

## 한번에 보이는 항목 만 보이게 하는 녀석

- LazyColumn은 화면에 보이는 항목만 렌더링하므로 항목이 많은 목록을 렌더링할 때 성능이 향상됩니다.
- 참고: LazyColumn과 LazyRow는 Android 뷰의 RecyclerView와 동일합니다.
- LazyColumn은 RecyclerView와 처럼 하위 요소를 재활용하지 않습니다. 컴포저블을 방출하는 것은 Android Views를 인스턴스화하는 것보다 상대적으로 비용이 적게 들므로 LazyColumn은 스크롤 할 때 새 컴포저블을 방출하고 계속 성능을 유지합니다.

## remember 의 문제점

- remember 함수는 컴포저블이 컴포지션에 유지되는 동안에만 작동합니다
- 기기를 회전하면 전체 활동이 다시 시작되므로 모든 상태가 손실됩니다. 
- 이 현상은 구성이 변경되거나 프로세스가 중단될 때도 발생합니다.

### 해결 방법

- remember를 사용하는 대신 rememberSaveable을 사용하면 됩니다. 이 함수는 구성 변경(예: 회전)과 프로세스 중단에도 각 상태를 저장합니다.

## animateDpAsState

- 이 컴포저블은 애니메이션이 완료될 때까지 애니메이션에 의해 객체의 value가 계속 업데이트되는 상태 객체를 반환합니다. 유형이 Dp인 '목표 값'을 사용합니다.
- animate*AsState를 사용하여 만든 애니메이션은 중단될 수 있습니다. 즉, 애니메이션 중간에 목표 값이 변경되면 animate*AsState는 애니메이션을 다시
  시작하고 새 값을 가리킵니다. 특히 스프링 기반 애니메이션에서는 중단이 자연스럽게 보입니다.

## padding 버그

- 참고로, 패딩이 음수가 되지 않도록 해야 합니다. 패딩이 음수가 되면 앱이 다운될 수 있습니다. 이로 인해 미세한 애니메이션 버그가 발생하며 이 버그는 추후 설정 완료에서 수정할 예정입니다.
--------------

# more?

- https://developer.android.com/jetpack/compose/kotlin#higher-order
- https://kotlinlang.org/docs/lambdas.html#function-types


--------------

# Jetpack Compose Basic Codelab
Week1 Jetpack Compose Basic 코드랩은 새 프로젝트를 만드는 것으로 시작합니다.

## 작업 전 필수 확인
Basics에 올려둔 프로젝트는 new project 입니다. 바로 이 프로젝트 위에서 코드랩을 시작하시면 됩니다. 

## 작업 후
Git 명령어 또는 [SourceTree](https://www.sourcetreeapp.com/), [GitKraken](https://www.gitkraken.com/) 등을 이용해 작업 결과를 push 해주세요.
