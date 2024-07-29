## Clean Code: Chapter 9. 단위 테스트

### 목차

1. **TDD 법칙 세 가지**
2. **깨끗한 테스트 코드 유지하기**
   - 테스트는 유연성, 유지보수성, 재사용성을 제공한다
3. **깨끗한 테스트 코드 작성하기**
   - 테스트 코드 작성에 필요한 기본 지식
   - 가독성 향상
   - 도메인에 특화된 테스트 언어
   - 이중 표준
   - 테스트 당 assert 하나
   - 테스트당 개념 하나
   - F.I.R.S.T
4. **결론**

---

### 1. TDD 법칙 세 가지

#### 테스트 주도 개발(TDD)의 의미와 등장 배경

테스트 주도 개발(Test Driven Development, TDD)은 코드를 작성하기 전에 테스트를 먼저 작성하는 개발 방법론입니다. TDD는 초기부터 품질 높은 코드를 작성하도록 유도하고, 지속적인 리팩토링을 통해 코드를 개선하는 데 중점을 둡니다. 1990년대 말부터 2000년대 초반, 소프트웨어 개발 방법론이 애자일(Agile) 방식으로 변화하면서 TDD도 함께 주목받기 시작했습니다. 이 방법론은 짧은 개발 주기를 통해 빠른 피드백을 얻고, 코드의 품질과 유지보수성을 높이는 데 도움을 줍니다.

#### TDD 법칙 세 가지

테스트 주도 개발(TDD)의 핵심은 다음 세 가지 법칙으로 요약됩니다.

1. **실패하는 테스트를 작성하기 전에는 제품 코드를 작성하지 않는다.**
2. **실패하는 테스트를 하나 이상 만들기 위한 분량 이상으로 테스트 코드를 작성하지 않는다.**
3. **현재 실패하는 테스트를 통과하기 위한 분량 이상으로 제품 코드를 작성하지 않는다.**

이 세 가지 법칙을 따르면 자연스럽게 짧은 주기로 테스트와 구현을 반복하게 되고, 이 과정에서 불필요한 코드 없이 꼭 필요한 코드만 작성하게 됩니다.

TDD는 다음과 같은 이유로 깨끗한 코드를 작성하는 데 효과적이라고 알려져 있습니다.

- **명확한 요구사항 정의:** 테스트를 먼저 작성함으로써 코드가 충족해야 할 요구사항을 명확히 정의하게 됩니다.
- **작고 단순한 단위로 개발:** 짧은 주기로 개발을 진행하기 때문에 복잡한 기능을 작고 단순한 단위로 나누어 개발할 수 있습니다.
- **빠른 피드백:** 테스트 주기를 짧게 유지함으로써 코드 변경에 따른 영향을 빠르게 확인할 수 있습니다.
- **높은 코드 품질:** 반복적인 테스트와 리팩토링을 통해 코드의 품질과 유지보수성을 지속적으로 향상시킵니다.

### 2. 깨끗한 테스트 코드 유지하기

#### 테스트는 유연성, 유지보수성, 재사용성을 제공한다

깨끗한 테스트 코드는 단순히 버그를 찾는 것을 넘어, 다음과 같은 이점을 제공합니다.

| 이점           | 설명                                                                                         |
| -------------- | -------------------------------------------------------------------------------------------- |
| **유연성**     | 깨끗한 테스트 코드는 코드 변경에 대한 두려움을 줄여주고, 새로운 기능 추가를 용이하게 합니다. |
| **유지보수성** | 깨끗한 테스트 코드는 읽고 이해하기 쉬워 유지보수 비용을 줄여줍니다.                          |
| **재사용성**   | 깨끗한 테스트 코드는 다른 테스트에서 재사용될 수 있어 테스트 코드 작성 시간을 단축시킵니다.  |

깨끗한 테스트 코드를 유지하기 위해서는 테스트 코드 작성에 노력을 기울여야 합니다. 테스트 코드는 실제 코드만큼 중요하게 생각하고 관리해야 합니다.

### 3. 깨끗한 테스트 코드 작성하기

#### 테스트 코드 작성에 필요한 기본 지식

| 개념                         | 설명                                                                | 예시                                                                                  |
| ---------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| **테스트 프레임워크**        | 다양한 테스트 프레임워크가 있으며, 여기서는 Jest를 예로 설명합니다. | Jest, Mocha, Jasmine                                                                  |
| **기본 함수**                | 테스트 코드 작성에 필요한 주요 함수입니다.                          | `describe()`, `it()`, `test()`, `expect()`                                            |
| **매처 함수**                | 테스트 결과를 검증하는 데 사용되는 함수입니다.                      | `toBe()`, `toEqual()`, `toBeTruthy()`, `toBeFalsy()`, `toContain()`, `toHaveLength()` |
| **비동기 테스트**            | 비동기 코드를 테스트하는 방법입니다.                                | `async/await`, `done` 콜백                                                            |
| **모의 함수(Mock Function)** | 모의 함수를 생성하여 테스트에 사용하는 방법입니다.                  | `jest.fn()`, `jest.mock()`                                                            |

**기본 함수 예시**:

```javascript
describe("간단한 테스트 예제", () => {
  it("1 더하기 1은 2여야 한다", () => {
    expect(1 + 1).toBe(2);
  });

  it("객체 비교 테스트", () => {
    expect({ name: "John" }).toEqual({ name: "John" });
  });

  it("비동기 코드 테스트", async () => {
    const data = await fetchData();
    expect(data).toBe("some data");
  });
});
```

#### 가독성 향상

테스트 코드의 가독성은 매우 중요합니다. 테스트 코드는 실제 코드만큼 자주 읽히지는 않지만, 버그 발생 시 원인을 파악하고 수정하는 데 중요한 역할을 합니다. 따라서 테스트 코드는 누구나 쉽게 이해할 수 있도록 명확하고 간결하게 작성되어야 합니다.

**예시:**

```javascript
// 좋지 않은 예: 여러 assertion이 섞여 있어 테스트의 의도를 파악하기 어렵습니다.
it("사용자 프로필을 올바르게 렌더링합니다", () => {
  render(<UserProfile userId={1} />);
  expect(screen.getByText("John Doe")).toBeInTheDocument();
  expect(screen.getByText("john.doe@example.com")).toBeInTheDocument();
  expect(
    screen.getByRole("button", { name: "Edit Profile" })
  ).toBeInTheDocument();
});

// 좋은 예: 각 assertion을 분리하고 설명을 추가하여 테스트의 의도를 명확하게 나타냅니다.
it("사용자 이름을 표시합니다", () => {
  render(<UserProfile userId={1} />);
  expect(screen.getByText("John Doe")).toBeVisible();
});

it("사용자 이메일을 표시합니다", () => {
  render(<UserProfile userId={1} />);
  expect(screen.getByText("john.doe@example.com")).toBeVisible();
});

it("프로필 편집 버튼을 렌더링합니다", () => {
  render(<UserProfile userId={1} />);
  expect(screen.getByRole("button", { name: "Edit Profile" })).toBeEnabled();
});
```

#### 도메인에 특화된 테스트 언어

도메인에 특화된 언어(DSL)는 특정 도메인에 특화된 언어를 의미합니다. 테스트 코드에서 DSL을 사용하면 테스트 코드를 보다 간결하고 읽기 쉽게 만들 수 있습니다.

도메인에 특화된 언어(DSL)를 테스트 코드에 구현함으로써 얻을 수 있는 장점은 다음과 같습니다.

- **간결함:** DSL을 사용하면 코드가 간결해져서 가독성이 높아집니다.
- **명확한 의도:** 테스트의 의도를 명확하게 표현할 수 있어, 코드 작성자와 독자가 쉽게 이해할 수 있습니다.
- **유지보수 용이:** DSL을 사용하면 코드의 중복을 줄이고, 테스트 코드의 유지보수가 용이해집니다.
- **재사용성:** DSL을 통해 자주 사용되는 패턴이나 로직을 재사용할 수 있습니다.

**예시:**

```javascript
// DSL을 사용하지 않은 예: 테스트 코드가 장황하고 이해하기 어렵습니다.
it("버튼 클릭 시 카운터가 증가합니다", () => {
  render(<Counter />);
  fireEvent.click(screen.getByText("+"));
  expect(screen.getByText("1")).toBeInTheDocument();
});

// DSL을 사용한 예: 테스트 코드가 간결하고 이해하기 쉽습니다.
it("버튼 클릭 시 카운터가 증가합니다", () => {
  render(<Counter />);
  clickButton("+");
  expectCounterValueToBe(1);
});

// DSL 함수 정의
function clickButton(buttonText) {
  fireEvent.click(screen.getByText(buttonText));
}

function expectCounterValueToBe(value) {
  expect(screen.getByText(value)).toBeInTheDocument();
}
```

#### 이중 표준

테스트 코드에서도 실제 코드와 동일한 수준의 품질을 유지해야 합니다. 테스트 코드라고 해서 복잡하고 지저분한 코드를 작성해서는 안 됩니다. 실제 코드와 테스트 코드는 서로 다른 환경에서 실행되지만, 품질과 가독성은 동일하게 유지해야 합니다. 효율성이나 성능보다는 단순함, 간결함, 표현력을 우선시하는 것이 좋습니다.

**예시:**

```javascript
@Test
public void turnOnLoTempAlarmAtThreshold() throws Exception {
  wayTooCold();
  assertEquals("HBchL", hw.getState());
}
```

#### 테스트 당 assert 하나

하나의 테스트 케이스는 하나의 개념만 테스트하는 것이 좋습니다. 여기서 `assert`는 테스트의 결과를 확인하는 검증 단계를 의미합니다. 여러 개의 `assert`를 사용해야 하는 경우, 각 `assert`를 별도의 테스트 케이스로 분리하는 것이 좋습니다. 이는 테스트의 가독성을 높이고, 각 테스트가 명확한 목적을 가지게 합니다.

하지만, 테스트 중복을 피하고 유지보수를 용이하게 하기 위해 `템플릿 메서드 패턴`을 사용할 수 있습니다. 이 패턴은 상위 클래스에 공통된 로직을 정의하고, 하위 클래스에서 구체적인 구현을 제공하는 방식입니다. 이를 통해 중복을 줄이고, 테스트의 일관성을 유지할 수 있습니다.

**예시:**

```javascript
// 좋지 않은 예: 두 가지 개념을 테스트하고 있어 테스트 실패 시 원인 파악이 어렵습니다.
it("폼 유효성 검사와 데이터 제출을 모두 테스트합니다", () => {
  // ...
});

// 좋은 예: 각 개념을 별도의 테스트 케이스로 분리하여 테스트 실패 시 원인 파악을 용이하게 합니다.
it("폼 유효성 검사", () => {
  // ...
});

it("데이터 제출", () => {
  // ...
});
```

**템플릿 메서드 패턴을 적용한 예시:**

```javascript
import React from "react";
import { render, screen, fireEvent } from "@testing-library/react";
import "@testing-library/jest-dom";
import LoginForm from "./LoginForm";

class LoginFormTestTemplate {
  renderLoginForm() {
    render(<LoginForm />);
  }

  expectUsernameFieldToBeRendered() {
    expect(screen.getByLabelText(/username/i)).toBeInTheDocument();
  }

  expectPasswordFieldToBeRendered() {
    expect(screen.getByLabelText(/password/i)).toBeInTheDocument();
  }

  expectSubmitButtonToBeRendered() {
    expect(screen.getByRole("button", { name: /submit/i })).toBeInTheDocument();
  }
}

class LoginFormUsernameTest extends LoginFormTestTemplate {
  run() {
    this.renderLoginForm();
    this.expectUsernameFieldToBeRendered();
  }
}

class LoginFormPasswordTest extends LoginFormTestTemplate {
  run() {
    this.renderLoginForm();
    this.expectPasswordFieldToBeRendered();
  }
}

class LoginFormSubmitButtonTest extends LoginFormTestTemplate {
  run() {
    this.renderLoginForm();
    this.expectSubmitButtonToBeRendered();
  }
}

test("로그인 폼에 사용자 이름 필드가 렌더링됩니다", () => {
  new LoginFormUsernameTest().run();
});

test("로그인 폼에 비밀번호 필드가 렌더링됩니다", () => {
  new LoginFormPasswordTest().run();
});

test("로그인 폼에 제출 버튼이 렌더링됩니다", () => {
  new LoginFormSubmitButtonTest().run();
});
```

**템플릿 메서드 패턴을 객체 지향 프로그래밍으로 작성한 이유:**

| 이유                                 | 설명                                                                                                                                                                                                                                                                       | 예시 코드                                                                                                                                                                                                                    |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **코드 재사용성 및 구성 향상**       | - 템플릿 메서드 패턴은 공통 로직을 상위 클래스에 정의하고, 하위 클래스에서 필요에 따라 재정의하여 사용합니다. <br> - 이를 통해 중복 코드를 줄이고, 각 테스트 케이스를 간결하게 유지할 수 있습니다.                                                                         | - `LoginFormTestTemplate` 클래스는 렌더링, 검증 등의 공통 로직을 제공합니다. <br> - 하위 클래스들은 `renderLoginForm`, `expectUsernameFieldToBeRendered` 등의 메서드를 재사용합니다.                                         |
| **테스트 케이스 간의 관계성 명확화** | - 클래스 상속을 통해 테스트 케이스 간의 관계를 명확하게 보여줄 수 있습니다.                                                                                                                                                                                                | - `LoginFormUsernameTest`, `LoginFormPasswordTest`, `LoginFormSubmitButtonTest` 클래스는 모두 `LoginFormTestTemplate` 클래스를 상속받습니다. <br> - 이는 각 테스트 케이스가 공통 로직을 공유하고 있음을 명확하게 보여줍니다. |
| **JavaScript 환경**                  | - JavaScript는 객체 지향 프로그래밍을 지원하며, 클래스는 코드 구성 및 재사용에 널리 사용되는 방법입니다. <br> - 함수형 프로그래밍도 점점 인기를 얻고 있지만, 테스팅 프레임워크와의 통합 및 기존 코드와의 호환성 측면에서 객체 지향 프로그래밍이 더 자연스러울 수 있습니다. | - React 컴포넌트는 클래스형 컴포넌트로 작성될 수 있습니다. <br> - 테스팅 라이브러리 또한 객체 지향 방식으로 디자인된 경우가 많습니다.                                                                                        |

#### 테스트당 개념 하나

테스트 케이스는 하나의 개념만 테스트해야 합니다. 여러 개념을 테스트하는 경우, 테스트 케이스를 여러 개로 분리하는 것이 좋습니다.

#### F.I.R.S.T

깨끗한 테스트 코드를 작성하기 위한 다섯 가지 규칙인 F.I.R.S.T를 기억하고 적용해야 합니다.

| 약어 | 의미                           | 설명                                                                                                                           |
| ---- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------ |
| F    | 빠르게 (Fast)                  | 테스트는 빠르게 실행되어야 합니다. 느린 테스트는 개발 속도를 저하시키는 주범입니다.                                            |
| I    | 독립적으로 (Independent)       | 각 테스트는 서로 의존하지 않아야 합니다. 테스트 순서에 따라 결과가 달라져서는 안 됩니다.                                       |
| R    | 반복가능하게 (Repeatable)      | 테스트는 어떤 환경에서든 동일한 결과를 출력해야 합니다. 외부 환경이나 의존성에 영향을 받아서는 안 됩니다.                      |
| S    | 자가검증하는 (Self-Validating) | 테스트는 성공 또는 실패 여부를 명확하게 나타내야 합니다. 수동으로 확인해야 하는 테스트는 좋은 테스트가 아닙니다.               |
| T    | 적시에 (Timely)                | 테스트는 코드를 작성하기 전이나 코드 작성 직후에 작성해야 합니다. 테스트 작성 시점이 늦어질수록 테스트 작성 비용이 증가합니다. |

### 4. 결론

깨끗한 테스트 코드 작성은 매우 중요합니다. 가독성을 높이고, DSL을 활용하고, 테스트 당 assert 하나를 유지하고, F.I.R.S.T 규칙을 준수하여 깨끗하고 효율적인 테스트 코드를 작성하여 애플리케이션의 품질을 향상시키고 개발 생산성을 높여야 합니다.
