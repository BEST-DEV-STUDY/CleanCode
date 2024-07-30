
## Chapter 10. 클래스

- [클래스](#클래스-체계)
- [클래스는 작아야 한다](#클래스는-작아야-한다)
- [변경하기 쉬운 클래스](#변경하기-쉬운-클래스)

### 클래스 체계

표준 자바 관례에서의 클래스 순서
- 정적 공개 상수
- 정적 비공개 변수
- 비공개 인스턴스 변수
- 공개 함수
- 비공개 함수

점점 더 추상화되는 방식으로 구성

```js
const MAX_NUMBER = 10;

const testFunction = () => {
    const variable = useMemo(()=>{},[]);
    const smallFunction = useCallback(()=>{},[]);
    const { data : {response} } = useCustomHook();
    return <div>{response}{MAX_NUMBER}</div>
}

```

### 클래스는 작아야 한다

- 함수의 크기를 물리적인 행 수로 측정한다면 클래스는 맡은 책임으로 사이즈를 측정한다.
- 단일 책임 원칙 (Single Responsibility Princible, SRP) 를 따른다.
작은 클래스가 많은 시스템이든 큰 클래스가 몇 개 뿐인 시스템이든 결국 익힐 양은 비슷하다! 가독성을 위해 작은 클래스로 나누는 것이 바람직하다. 
- 응집도는 높아야 한다.
응집도란, 클래스에 있는 메서드에서 변수를 충분히 많이 활용하는 것으로 논리적으로 서로 의존성이 있다는 것을 의미한다.

단일 책임 원칙을 따르고 응집도를 높이면 코드의 길이는 늘어날 수 있다. 
길이가 늘어난 이유는
1) 리팩터링한 프로그램은 좀 더 길고 서술적인 변수 이름을 사용한다.
2) 리팩터링한 프로그램은 함수선언과 클래스 선언을 통해 주석처럼 가독성을 높인다.
3) 가독성을 위해 공백을 추가하고 형식을 맞추었다. 

- 개인 경험 예시
MarketplaceBody 하나를 아래의 구조로 분리

변수 이름으로 어떤 컴포넌트인지 알 수 있고(1), 가독성이 높아지고 (2), 디렉토리와 형식을 맞추었습니다.(3)

```
Marketplace Page(route 처리) 
    - MarketplaceExplore(layout 및 내부 컴포넌트 composition) 
        - MarketplaceFilter
            - CategoryField, UtilityField, PropertyField, PriceField..(내부에 RadioField,CheckboxField 등 사용)
        - MarketplaceChipList 
            - Chip
                - CategoryChip, UtilityChip, PropertyChip....
        - MarketplaceSort
        - MarketplaceList

FormField
 - RadioField (추상화)
 - CheckBoxField (추상화)
 ```       

### 변경하기 쉬운 클래스

- OCP(Open-Closed Princible) 확장에 개방적이고 수정에 폐쇄적이어야 한다는 원칙이다. 
- 결합도가 낮은 코드는 각 시스템 요소가 다른 요소와 변경으로부터 잘 격리되어있다는 의미이다. 따라서 유연하고 재사용성이 높다.
- 결합도를 낮추면 DIP(Dependency Inversion Principle) 을 따르게 된다. 클래스가 상세한 구현이 아닌 추상화된 구현에 의존해야한다는 것

```js
const Sql = () => {
    const { createCb } = useCreateSql();
    const { insertCb } = useInsertSql();
    const { selectCb } = useSelectSql();
    ...
}
```
createSql 을 수정할 때는 다른 useInsertSql(), useSelectSql() hook 에 영향이 가지 았고, 

```js
  const { findBySqlCb } = useFindBySql(); 
```
새로운 hook을 추가해도 다른 hook에 영향이 가지 않기 떄문에 OCP 원칙을 잘 지킨 코드가 됩니다.

결합도를 낮춘 코드, 즉 추상화된 코드는 정확한 테스트코드를 작성하기도 쉬워집니다. 

```js
 const Portfolio = () => {
    const { data : { seoulStockPrice } } = useSeoulStockExchange();
}
```

