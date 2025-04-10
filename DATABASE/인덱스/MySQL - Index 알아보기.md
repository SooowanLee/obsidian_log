---
상태: 완료
시작일: 2024-06-03
마감일: 2024-06-03
---
## 인덱스

> 테이블의 데이터에 대한 검색 성능을 향상하기 위해 사용되는 구조  
> 빠르게 정렬 그룹핑하기 위해 사용

특정 열(또는 열들)에 대한 데이터를 정렬된 형태로 저장하여 빠른 검색을 가능하게 합니다. 책의 색인처럼 작동, 데이터 검색 시에 Full Scan을 하지 않고 빠르게 접근할 수 있게 합니다.

### 구조

-   **[[B Tree - 데이터 삽입|B-Tree]]**
-   **해시 테이블 구조**

### Index를 쓰는 이유

**특정 조건을 만족하는 데이터를 빠르게 조회하기 위해서 사용합니다.**

아래 코드에서 SELECT, DELETE, JOIN등에서 WHERE나 ON에서 데이터를 빠르게 조회하기 위해 Index를 사용합니다.

```SQL
SELECT * FROM customer WHERE first_name = 'Minsoo';
DELETE FROM logs WHERE log_datatime < '2024-01-01';
SELECT * FROM employee E JOIN department D ON E.dept_id = D.id;
```

### Index 생성 방법

#### 테이블에 데이터가 이미 존재

중복을 허용하는 인덱스

-   `CREATE INDEX player_name_idx ON player(name);`

중복을 허용하지 않는 인덱스

-   `CREATE UNIQUE INDEX team_id_backnumber_idx ON player(team_id, backnumber);`

#### 테이블 생성 시에 인덱스 적용

CREA TABLE에서 INDEX를 적용하면 INDEX 이름을 생략해도 DBMS에서 자동으로 이름을 생성해 준다.

```
CREATE TABLE player(
    id INT PRIMARY KEY,
    name VARCHAR(20) NOT NULL,
    team_id INT,
    backnumber INT,
    INDEX player_name_idx(name),
    UNIQUE INDEX team_id_backnumber_idx(team_id, backnumber)
);
```

두 개 이상의 컬럼으로 구성된 인덱스를 multicolumn index, composite index라고 한다.

대부분의 RDBMS에서는 primary key에는 index가 자동 생성된다.

### 테이블에 걸린 인덱스 보기
SHOW INDEX FROM TABLE
- 예) `SHOW INDEX FROM player`


![](https://i.imgur.com/tefqTIZ.png)

`Table` : 테이블 이름  
`Non_unique`: 인덱스의 유니크 여부  
`Key_name`: Index 이름  
`Seq_in_index`: 인덱스 내에서 열(column)의 순서

-   Seq\_in\_index가 1, 2인 것으로 보아 multicolumn index라는 걸 알 수 있다.

`Column_name`: 컬럼 이름  
`Null`: 해당 INDEX의 Attribute의 Null 허용 여부

### B-Tree 기반의 index가 동작하는 방식

INDEX는 키, 값 형태로 데이터를 저장하고 있습니다.  
키에는 컬럼의 값을 가지고 있고, 값에는 값의 주소를 가지고 있습니다.

MEMBERS라는 테이블의 a컬럼을 기준으로 인덱스를 생성합니다.

![](https://i.imgur.com/UbtWgze.png)

인덱스는 기본적으로 정렬해서 값을 가지고 있습니다.

**여기서 `WHERE a = 8`로 a를 조회한다면**

![](https://i.imgur.com/8XpYIOI.png)

가운데 값인 5를 가지고 조건과 비교를 합니다. 그런데 **where의 값은 8인데 가운데 값은 5이기 때문에 위쪽에 있는 값을 가지고 확인**을 합니다.

그럼 이제 위쪽 부분에서 가운데를 고릅니다. 9겠죠?  
그럼 9를 기준으로 확인을 합니다. 그런데 `where a = 8`이기 때문에 아래쪽을 확인합니다.  
조건과 맞는 값을 찾았기 때문에 `ptr`에 있는 주소를 따라 `Member` 테이블에 가서 값을 가지고 옵니다.

하지만 여기서 끝내기 않고 또 위아래를 비교합니다. 그 이유는 8과 중복되는 튜블이 존재할 수도 있기 때문입니다. 위 아래를 비교 한 다음 같은 값이 없다면 조회를 종료합니다.

**이번에는 `WHERE a = 8 AND b = 80`로 조회하겠습니다.**  
**INDEX에서 앞의 컬럼을 기준으로 정렬을 하고 중복 값이 있다면 뒷 컬럼을 가지고 정렬합니다**

-   예) a 컬럼으로 먼저 정렬을 하고 8과 같은 중복된 값이 있다면 b 컬럼을 가지고 정렬을 합니다.

단일 조회일 때와는 살짝 다른데 처음에 a를 가져오는 방식은 같습니다.

![](https://i.imgur.com/91mJKJf.png)

`WHERE a = 8;`을 조회했을 때와 같은 방식으로 가운데 값인 5를 가지고 비교를 합니다.  
8이 더 크기 때문에 그럼 위쪽에서 가운데 값인 9를 가지고 비교를 합니다.(5 아래의 값은 8보다 작기 때문에 조회 대상에서 제외)  
8은 9보다 작기 때문에 9 아래의 값을 가지고 비교를 합니다.(12는 8보다 크기 때문에 조회 대상에서 제외)

![](https://i.imgur.com/dKVPbdC.png)

`where = 8`과 a가 일치합니다. 그럼 이제 b를 보면 100이기 때문에 아래로 훑으면서 검색을 합니다.

![](https://i.imgur.com/z9fBbhg.png)

바로 아래의 b가 80이기 때문에 ptr에 있는 주소를 따라서 `MEMBERS`테이블에서 값을 가져오게 됩니다.  
하지만, 마지막 `a = 8`인 컬럼이 남아있기 때문에 `a=8, b=78`인 컬럼까지 확인을 하고 최종 값을 반환합니다.

### 쿼리가 어떤 인덱스를 사용하는지 확인하기

우리는 INDEX를 지정하지 않았는데 신기하게 DBMS는 INDEX를 적용해서 데이터를 조회합니다. 어떻게 이런 일이 일어나는가? **바로 옵티마이저가 적절한 INDEX 사용하기 때문입니다.**

SELECT 앞에 EXPLAIN을 적어주면 해당 쿼리가 어떤 INDEX를 사용하는지 알 수 있습니다.  
`EXPLAIN SELECT * FROM player WHERE backnumber = 7;`

![](https://i.imgur.com/YMdSXqe.png)

**table** : index를 통해서 조회한 테이블  
**possible\_keys**: 어떤 인덱스를 사용해서 조회했는지

### 인덱스 지정하기

SELECT 쿼리를 통해 데이터를 조회할 때 성능이 안 나오거나 이상한 INDEX를 사용해서 조회해 오는 경우가 있습니다.  
그럴 때 우리가 생성하고 사용하고 싶은 인덱스를 지정해서 조회할 수 있습니다.

-   USE INDEX
-   FORCE INDEX

**USE INDEX**(가급적 내가 원하는 INDEX를 사용해 주세요~의 느낌)  
`SELECT * FROM player USE INDEX (backnumber_idx) WHERE backnumber = 7;`

**FORCE INDEX**(꼭 내가 지정한 INDEX를 사용해 주세요!)  
`SELECT * FROM player FORCE INDEX (backnumber_idx) WHERE backnumber = 7;`

**TIP)**

> FORCE를 사용해서 INDEX를 지정하면 옵티마이저가 최대한 지정한 INDEX를 사용해서 데이터를 조회하겠지만 **정말 적절한 INDEX가 없다면 FULL SCAN을 할 수 있습니다.**

### 인덱스 사용 주의사항

인덱스를 사용하면 데이터 조회 속도가 빠르니까 모든 컬럼에 인덱스를 만들어서 사용해도 괜찮을까?

**인덱스를 만들 때는 원래 테이블의 데이터 말고 INDEX를 위한 부가적인 데이터가 생성됩니다.**  
만약, 원래 테이블에 생성, 수정, 삭제 같은 write 행위를 할 때마다 INDEX에도 변경이 발생합니다.  
그럼 변경하기 위한 시간이 소요됩니다. 인덱스가 많아질수록 오버헤드가 커집니다.

**추가적인 저장 공간 차지**  
인덱스마다 인덱스를 위한 데이터가 생성이 되기 때문에 각가의 저장공간을 차지합니다.

### Covering index

만약 조회하려는 컬럼이 모두 인덱스에 포함되어 있다면 원래 테이블까지 조회하지 않고 INDEX에서 데이터를 바로 반환합니다.  
**예를 들어) team_id, backnumber를 조회하려 할 때** -> **INDEX(team_id, backnumber)를** 사용한다면 INDEX안에 team_id와 backnumber가 모두 있기 때문에 원래 테이블을 조회할 필요가 없습니다.