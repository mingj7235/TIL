# 생성자 대신 정적 팩터리 메서드를 고려하라

## 장점 

1.  이름을 가질 수 있다. 

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

    -  정적 팩터리 메서드를 통해 명확히 역할이 무엇인지 알게된다. 

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


2.  호출될 때마다 인스턴스를 새로 생성하지 않아도 된다.

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
    
    <br>

    ```yaml
    me.whiteship.chapter01.item01.Settings@22f71333
    me.whiteship.chapter01.item01.Settings@22f71333

    -> 인스턴스의 주소가 동일하다.
    ```

<br>

3. 반환 타입의 하위 타입 객체를 반환할 수 있다. 

    - 가령, 인터페이스를 리턴하는 ``반환 타입을 상위 클래스``를 하고,

        실제로 리턴할 때는 ``구현체를 리턴`` 해줄 수도 있다.

    ```java

        // HelloService 는 인스턴스이고, 
        // KoreanHelloService 와 EnglishHelloService 는 구현체다.
        public static HelloService of (String args) {
            if (args.equals("ko")) {
                return new KoreanHelloService();
            } else {
                return new EnglishHelloService();
            }
        }
    ```


<br>

4.  입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
    
    > 3번과 장점과 연관되어 클래스의 객체를 유연하게 가져갈 수 있다.

    ```java
            HelloService eng = HelloService.of("eng");
            HelloService ko = HelloService.of("ko");

            System.out.println(eng.hello()); // hello
            System.out.println(ko.hello()); // 안녕하세요

    ```


5.  정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.

    - 이 부분이 이해하기 좀 어려움 ... !

    ```java

        // HelloService 인터페이스를 구현하고 있는 구현체가 없는 상황

        // HelloService 에 구현되어 있는 모든 클래스를 찾아온다.
        ServiceLoader<HelloService> loader 
                        = ServiceLoader.load(HelloService.class);
        
        // 첫번째 구현된 클래스를 가지고 온다.
        Optional<HelloService> helloServiceOptional = loader.findFirst();
        

        helloServiceOptional.ifPresent(h -> {
            System.out.println(h.hello());
        }); // Ni Hao 가 출력된다.
    ```

    - 굳이 아래와 같이 왜 안함?

    ```java
        ChineseHelloService helloService = new ChineseHelloService();
        System.out.println(helloService.hello());
    ```

    >  이유 : ChineseHelloService 에 의존적이게 된다.
    <br> import를 해야만 구현이 가능하다. 


    어떤 구현체가 올지 모르지만, 유연함을 위해 상위 인터페이스만을 사용하여 코드를 작성해야할 때, 
    이런 방식을 활용하면 ``구현체에 의존적이지 않은`` 코드를 작성할 수 있다.


## 단점

1. 생성자가 아닌, 정적 팩토리 메서드를 통해서만 인스턴스를 얻는다면, 해당 클래스를 상속받을 수 없다.

    ```java

    public class Settings {

        private Settings() {}

        private static final Settings SETTINGS = new Settings();

        public static Settings getInstance() {
            return SETTINGS;
        }
    }
    ```

    > 생성자가 ``private`` 이므로, 절대 Settings 클래스는 상속 할 수 없다. 
    상속을 위해서는 ``public, protected`` 생성자가 필요하기 때문이다.

    - 정적 팩토리 메소드를 제공하면서, 생성자도 열어놓는 방법도있다 ex> java util 의 ``List`` 클래스

    <br>

2. javaDoc 을 통해 확인하기가 어렵다.

    - 그렇기에 통상적으로 사용하는 네이밍 컨벤션으로 메소드명을 짓는다.
    <br>
    ex> of, getInstance 등

    - Class 위에 메모를 통해 표시한다.

    ```java
        /**
        * 이 클래스의 인스턴스는 #getInstance()를 통해 사용한다.
        * @see #getInstance()
        */
        public class Settings {

            private boolean useAutoSteering;

            private boolean useABS;

            private Difficulty difficulty;

            private Settings() {}

            private static final Settings SETTINGS = new Settings();

            public static Settings getInstance() {
                return SETTINGS;
            }

        }
    ```

    > javaDoc 만드는 방법
    <br> Maven 프로젝트인 경우 - ``$ mvn javadoc:javadoc``


## 주의

- 반드시 정적 팩터리 메서드를 만들라는 말이 아니다. 

- 정적 팩터리 메서드가 유효할 때가 있다. 
