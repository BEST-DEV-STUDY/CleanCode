## Chapter 2. 의미 있는 이름


### 의도를 분명히 밝혀라

- 좋은 이름을 지으려면 시간이 걸리지만 좋은 이름으로 절약하는 시간이 훨씬 더 많다.

- 따로 주석이 필요하다면 의도를 분명히 드러내지 못했다는 말이다.

  

```java
public List<int []> getThem() {
	List<int []> list1 = new ArrayList<int []>();
	for (int[] x : theList)
		if (x[0] ==4)
			list1.add(x);
		return list1;
}
```

- 코드 맥락이 코드 자체에 명시적으로 드러나지 않는다.



```java
public List<int []> getFlaggedCells() {
	List<int []> flaggedCells = new ArrayList<int []>();
	for (int [] cell : gameBoard)
		if(cell[STATUS_VALUE] == FLAGGED)
			flaggedCells.add(cell);
	return flaggedCells;
}
```

- 각 개념에 이름만 붙여도 코드가 상당히 나아진다.



```java
public List<Cell> getFlaggedCells() {
	List<Cell> flaggedCells = new ArrayList<Cell>();
	for (Cell cell : gameBoard)
		if(cell.isFlagged())
			flaggedCells.add(cell);
	return flaggedCells;
}
```

- (int 배열 → 클래스 사용) 단순히 이름만 고쳤는데도 함수가 하는 일을 이해하기 쉬워졌다.



### 그릇된 정보를 피하라

- 널리 쓰이는 의미가 있는 단어를 다른 의미로 사용해도 안된다.
  - ex) hp, aix, sco
- 서로 흡사한 이름을 사용하지 않도록 주의한다.
- 유사한 개념은 유사한 표기법을 사용한다. 이것도 정보다. 일관성이 떨어지는 표기법은 그릇된 정보다.
(이름으로 그릇된 정보를 제공하는 예시로 소문자 `l`과 대문자 `O`가 있다. 이 둘은 각각 숫자 `1`, 숫자 `0`과 비슷한 모양을 갖고 있기 때문에 혼란을 가져올 수 있다. 또한, Name이 부동소수가 될 가능성이 있는 경우 'NameString'으로 이름을 지을 경우 그릇된 정보에 해당한다.)




### 의미 있게 구분하라

- 연속된 숫자를 덧붙이거나 불용어(의미가 없는 단어)를 추가하는 방식은 적절하지 못하다.

  - 그릇된 정보를 제공X 즉, 아무런 정보를 제공하지도 못하는 이름

  

```java
public static void copyChars(char a1[], char a2[]) {
	for (int i = 0; i< a1.lenght; i++) {
		a2[i] = a1[i];
}
```

  - a1과 a2가 무엇을 의미하는지 알 수 없다.

  ```java
  public static void copyChars(char source[], char destination[]){
      for (int i = 0; i < source.length; i++){
          destination[i] = source[i];
      }
  }
  ```

  - 함수 인수 이름을 source와 destination으로 변경한다면 훨씬 더 코드 읽기가 쉬울 것이다.

```
getActiveAccount vs getActiveAxcounts vs getActiveAccountInfo
moneyAmount vs money
customerInfo vs customer
accountData vs account
theMessage vs message
```
  - 불용어를 추가한 이름 역시 아무런 정보도 제공하지 못함을 볼 수 있다.

**`읽는 사람이 차이를 알도록 이름을 지어야한다.`**

### 발음하기 쉬운 이름을 사용하라

- 발음하기 어려운 이름은 토론하기도 어렵다
(ex : `genymdhms` = `generate date, year, month, day, hour, minute, second`)

### 검색하기 쉬운 이름을 사용하라

- `WORK_DAYS_PER_WEEK` 과 같은 검색하기 쉬운 이름을 사용해야 한다.

**이름 길이는 범위 크기에 비례해야 한다**

### 인코딩을 피하라 -> 옛날 방식으로 보여 패스

### 자신의 기억력을 자랑하지 마라

- 독자가 코드를 읽으면서 변수 이름을 자신이 아는 이름으로 변환해야 한다면 그 변수 이름은 바람직하지 못하다.

- 루프 범위가 아주 작을 때, 루프에서 반복 횟수를 세는 변수 i,j,k는 괜찮다. (l은 절대 안된다!)

### 클래스 이름

- `명사`나 `명사구`가 적합

  - Customer
  - WikiPage
  - Account
  - AddressParser

- Manager, Processor, Data, Info 등과 같은 단어는 피하고, 동사는 사용하지 않는다.

  

### 메서드 이름

- `동사`나 `동사구`가 적합

  - postPayment
  - deletePage
  - save

- `접근자, 변경자, 조건자`는 javabean 표준에 따라 값 앞에 `get, set, is`를 붙인다.

### 기발한 이름은 피하라

- 특정 문화에서만 사용하는 농담과 같은 기발한 이름은 피하는 편이 좋다.

### 한 개념에 한 단어를 사용하라

- 똑같은 메서드를 클래스마다 fetch, retrieve, get으로 제각각 부르면 혼란스럽다.
  - 어느 클래스에서 어느 이름을 썼는지 기억하기 어려워 과거 코드를 살피느라 엄청난 시간을 소모할 가능성이 있다.

**`메서드 이름은 독자적이고 일관적이어야 한다.`**

- 한 단어를 두가지 목적으로 사용하지 마라.
  - "한 개념에 한 단어를 사용하라" 규칙을 따르다 보면, 같은 맥락이 아닌데도 '일관성'을 고려해 같은 단어를 사용하는 경우가 있다. 주의하자.

### 해법 영역/문제 영역에서 가져온 이름을 사용하라

- 기술 개념에는 기술 이름이 가장 적합한 선택이다. (ex : 전산 용어, 알고리즘 이름, 수학 용어 등)
- 문제 영역 개념과 관련이 깊은 코드라면 문제 영역에서 이름을 가져와야 한다.

### 의미 있는 맥락을 추가하라

- firstName, lastName, street, houseNumber, city, state, zipcode

  - 변수명을 보면 주소라는 사실을 알 수 있다.
- 만약 state 변수 하나만 사용한다면?

  - state가 주소 일부라는 사실을 알아차리기 어렵다.

  - 그럼 어떻게 해야할까?

    - 주소라는 것을 알아차리기 쉽게 접두어를 추가한다.
- ex : addrFirstName, addrLastName, addrState
    - Address라는 클래스를 생성하면 더 좋다.변수가 좀 더 큰 개념에 속한다는 사실이 컴파일러에게도 분명해진다.

#### [참고]

###### 루프속 i j k 자제

```java
//bad
for (int i = 0; i < messages.size(); i++) {}

//good
for (String message : messages) {}
messages.stream().forEach(
	message -> //...
)
```

- i, j -> row, col / width, height
- i, j, k -> row, col, depth


### 마치면서

- 코드를 개선하려는 노력을 중단해서는 안된다.
- 이 장에서 소개한 규칙 몇 개를 적용해 코드 가독성이 높아지는지 살펴보라.
