# 생성자 대신 정적 팩터리 메서드를 고려하라

## 장점 

- 이름을 가질 수 있다. 

    - 생성자로는 이렇게 사용 못함 ㅎㅅㅎ...

        ```java

            public Order(final boolean prime, final Product product) {
            this.prime = prime;
            this.product = product;
            }   
        
            public Order(final boolean urgent, final Product product) {
            this.urgent = urgent;
            this.product = product;
            }

            // 동일한 인자값을 가진 생성자는 생성불가
            // 인자값의 순서를 바꿔서 우회할 수 있지만, 좋지 않은 방법! 
            

        ```

    - 정적 팩터리 메서드를 통해 명확히 역할이 무엇인지 알게된다. 

        ```java
            public static Order primeOrder(Product product) {
                Order order = new Order();
                order.prime = true;
                order.product = product;
                return order;
            }

            public static Order urgentOrder(Product product) {
                Order order = new Order();
                order.urgent = true;
                order.product = product;
                return order;
            }

        ```

    >생성자명은 고정이 되어있다. 메서드 명을 주게 되면 명확한 기능을 가지게 된다.
    <br>
    만들어주는 객체의 특성을 메서드의 이름으로 표현할 수 있다는 점! 


<br>


- 호출될 때마다 인스턴스를 새로 생성하지 않아도 된다.

    - public 생성자를 통해 인스턴스를 생성한다면 컨트롤 할 수 없다.

        즉, public 으로 열려있다면, 생성자를 통해 인스턴스를 마음껏 만들 수 있다. 

    ```yaml
    me.whiteship.chapter01.item01.Settings@22f71333
    me.whiteship.chapter01.item01.Settings@13969fbe


    -> 인스턴스의 주소가 새로 생성이 된다. 
    ```

    ```java
        private Settings() {}

        private static final Settings SETTINGS = new Settings();

        // 외부에서 호출 할 수 있는 방법은 유일하게 이 정적 메소드를 호출하는 방법 밖에 없다.
        public static Settings getInstance() {
            return SETTINGS;
        }
    ```


   -  하지만, private으로 생성자를 선언하고, 정적 펙토리 메소드로 제공하면 통제가 가능하다.

    ```yaml
    me.whiteship.chapter01.item01.Settings@22f71333
    me.whiteship.chapter01.item01.Settings@22f71333

    -> 인스턴스의 주소가 동일하다.
    ```


- 반환 타입의 하위 타입 객체를 반환할 수 있다. 

- 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.

- 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.


## 단점

- 상속을 하려면 public 이나 protected 생성자가 


## 주의

- 반드시 정적 팩터리 메서드를 만들라는 말이 아니다. 

- 정적 팩터리 메서드가 유효할 때가 있다. 
