# Database Keys

데이터베이스를 사용하려면 꼭 알아야 하는 Key(키)에 대한 내용 정리입니다.  
해당 내용은 RDBMS를 기준으로 작성되었습니다.

# Key

> 키는 `테이블 내에서 데이터를 구별하고 식별할 수 있는 역할을 하는 속성`이다.

- 키는 데이터 무결성을 보장하고, 테이블 간의 관계를 정의하는 데 사용된다.
- 데이터베이스에서 키는 행 또는 데이터 항목을 고유하게 식별하는 데 중요한 역할을 한다.

## 키 설계에서 중요한 개념

### 유일성
> 유일성은 `데이터베이스에서 키가 각 행(레코드)를 고유하게 식별할 수 있도록 보장하는 속성`이다.
- 각 키 값은 `테이블 내에서 유일`해야 하며, `중복되지 않도록` 해야 한다.  
- 유일성이 보장되어야만 데이터 간에 혼동을 피할 수 있고, 정확한 조회가 가능하다.

### 최소성
> 최소성은 `키를 구성하는 컬럼(속성)들이 불필요한 요소를 포함하지 않도록 해야 한다는 속성`이다.
- 키를 구성하는 컬럼들 중 어떤 컬럼을 제외해도 여전히 유일성을 유지할 수 있으면, 해당 컬럼은 키에 포함될 필요가 없다.
- e.g. 학생을 구별할 때, [학생 번호, 이름, 수강 과목]의 컬럼이 있으면 [학생 번호]만으로 학생을 구별할 수 있으므로 이는 최소성을 만족하고, 키로 사용하는 것이 좋다.
- 최소성은 `후보키의 중요한 속성`으로, 후보키는 가능한 적은 수의 컬럼으로 데이터를 고유하게 식별할 수 있어야 한다. -> 키의 크기를 최소화해야 한다.

### RDBMS에서 키를 사용하는 이유

- 데이터의 고유 식별 및 중복 방지
- 데이터 무결성 및 참조 무결성 보장
- 데이터 검색 성능 향상
- 테이블 간 관계 정의

### 키의 종류

![](/Contents/Database/Images/Database_Key.jpg)

- 기본키(Primary Key)
- 후보키(Candidate Key)
- 대체키(Alternate Key)
- 외래키(Foreign Key)
- 복합키(Composite Key)
- 수퍼키(Super Key)

## 기본키(Primary Key)

> 기본키는 `테이블에서 각 행을 고유하게 식별하는 데 사용되는 키`이다.

- `유일성`과 `최소성`, `Not-NULL`을 보장한다.
    - NULL값을 가질 수 없다.
- 후보키 중에서 `메인으로 선정되는 키`이다.
- 행을 식별할 때 기준이 되는 `반드시 필요한 키`이다.

### 기본키를 위한 조건

1. 값의 변동이 잦지 않은 후보키여야 한다.
2. NULL값을 가질 수 있는 속성이 포함된 후보키가 아니어야 한다.
3. 후보키 중 단순한 키여야 한다.
4. 하나의 테이블에는 반드시 하나의 기본키만 존재한다.


### 예시
```sql
CREATE TABLE Student (
    id INT PRIMARY KEY, -- 기본키
    name VARCHAR(100),
    email VARCHAR(100),
    age INT
);
```

## 후보키(Candidate Key)

> 후보키는 테이블에서 `각 행을 고유하게 식별할 수 있는 여러 키 후보`이다.

- `유일성`과 `최소성`을 보장한다.
- 기본키는 후보키 중에서 선택되며, 선택되지 않은 후보키는 대체키이다.


### 예시
```sql
CREATE TABLE Student (
    id INT PRIMARY KEY, -- 기본키
    name VARCHAR(100),
    email VARCHAR(100), -- 후보키(대체키)
    age INT
);
```


## 대체키(Alternate Key)

> 대체키는 `기본키로 선택되지 않은 후보키`이다.

- `유일성`과 `최소성`을 보장한다.
- 예시는 위와 같다.


## 외래키(Foreign Key)

> 외래키는 `다른 테이블의 기본키를 참조하여 두 테이블 간의 관계를 설정하는 키`이다.

- `최소성`을 보장한다.  
(외래키가 참조하는 부모 테이블의 기본키 또는 고유키(UNIQUE)는 유일성을 보장한다.)
- 외래키는 부모 테이블의 기본키나 고유키를 참조하므로, 최소성이 이미 보장된다.
- 외래키를 설정하지 않아도 되지만, 데이터 무결성 때문에 설정하는 경우가 많다.
- e.g. 아래의 예시에서 Student의 id를 변경시키는 경우, Student_Subject 테이블에 참조되어 있는 student_id가 함께 변경되어야 하기 때문이다.

### 예시
```sql
CREATE TABLE Subject (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE Student (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    age INT
);

CREATE Student_Subject (
    student_id INT,
    subject_id INT,
    FOREIGN KEY (student_id) REFERENCES Student(id), -- 외래키
    FOREIGN KEY (subject_id) REFERENCES Subject(id), -- 외래키
    PRIMARY KEY (student_id, subject_id)
);
```

## 복합키(Composite Key)

> 복합키는 `두 개 이상의 컬럼을 결합한 키`이다.

- `유일성`을 보장한다.
- 단일 컬럼으로는 데이터의 유일성을 보장할 수 없을 때, 컬럼들을 조합하여 사용한다.
- e.g. 아래의 예시에서 order_id와 product_id 모두 있어야 상품을 구분할 수 있으므로, 복합키로 만들어 고유한 행을 식별할 수 있도록 한다.

### 예시
```sql
CREATE TABLE OrderDetails (
    order_id INT,
    product_id INT,
    quanrity INT,
    PRIMARY KEY (order_id, product_id) -- 복합키
);
```

## 수퍼키(Super Key)

> 수퍼키는 테이블에서 `한 개 이상의 컬럼을 포함하고, 그 조합이 각 행을 고유하게 식별할 수 있게 해주는 키`이다.

- `유일성`을 보장한다.
- 모든 후보키는 수퍼키이지만, 수퍼키는 불필요한 컬럼을 포함할 수 있다.  
(최소성을 보장하지 않는다.)
- 아래의 예시에서 id와 이름의 조합은 수퍼키로 사용할 수 있지만, id만으로도 유일성을 만족하므로 수퍼키이지만 후보키로 사용될 수 없다.

### 예시
```sql
CREATE TABLE Student (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT,
    UNIQUE (id, name) -- 수퍼키
);
```

---

## 면접 대비 질문

Q 1)   
기본키(Primary Key)와 복합 키(Composite Key)를 설명해보세요.

A 1)  
기본키는 테이블에서 각 행을 고유하게 식별하는데 사용되는 키이며, 유일성과 최소성을 보장합니다.  
테이블에는 기본키를 반드시 하나만 가지고 있어야 하며, NULL이거나 수정 및 업데이트가 불가능합니다.  

복합키는 두 개 이상의 컬럼을 결합한 키이며, 유일성을 보장합니다.  
단일 컬럼으로 유일성을 보장할 수 없는 경우, 두 개 이상의 컬럼을 결합하여 사용합니다.
