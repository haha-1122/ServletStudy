# DAO & DTO & Entity



## DAO(Data Access Object)

-   실제로 DB에 접근하는 객체로 Persistence Layer라고도 한다.

    Persistence Layer: DB에 data를 CRUD하는 계층

-   Service와 DB를 연결하는 고리의 역할을 한다.

-   SQL을 사용하여 DB에 접근한 후 적절한 CRUD API를 제공한다.



## DTO(Data Transfer Object)

-   계층간 데이터 교환을 위한 객체(Java Beans)이다.

    -   DB에서 데이터를 얻어 Service나 Controller 등으로 보낼 때 사용하는 객체를 말한다. 즉, DB의 데이터가 Presentaion Logic Tier로 넘어오게 될 때는 DTO의 모습으로 바뀌어서 오고가는 것이다.
    -   로직을 갖고 있지 않는 순수한 데이터 객체이며, getter/setter 메서드만 갖는다. 하지만 DB에서 꺼낸 값을 임의로 변경할 필요가 없기 때문에 DTO클래스에는 setter가 없고 대신 생성자에서 값을 할당한다.

-   VO (Value Object)

    DTO의 read only 속성만 가지고 있는 클래스



## Entity Class

-   실제 DB의 테이블과 매칭될 클래스

    즉, 테이블과 링크될 클래스임을 나타낸다. Entity 클래스 또는 가장 Core한 클래스라 부른다.

-   최대한 외부에서 Entity 클래스의 getter method를 사용하지 않도록 해당 클래스 안에서 필요한 로직 method를 구현한다.

    -   단, Domain Logic만 가지고 있어야 하고 Presentation Logic을 가지고 있어서는 안된다. 또한 여기서 구현한 method는 주로 Service Layer에서 사용한다.



## Entity 클래스와 DTO클래스를 분리하는 이유

-   View Layer와 DB Layer의 역할을 철저하게 분리하기 위해서
-   테이블과 매핑되는 Entity 클래스가 변경되면 여러 클래스에 영향을 끼치게 되는 반면 View와 통신하는 DTO클래스는 자주 변경되므로 분리해야 한다.
-   