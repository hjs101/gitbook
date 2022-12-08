# 1일차

## 디자인 패턴-

### 싱글톤 패턴
---
- 단일 클래스, 단일 인스턴스
- 한 개의 인스턴스만 가지고 그 인스턴스를 공유하는 패턴
- 비용은 감소하지만 종속성 UP
- TDD(Test Drivin Development) : 테스트 주도개발에 방해가 됨 - 의존성 주입으로 해결

**싱글톤 패턴의 사용 예 : DB연결(JS)**
```
const URL = 'mongodb://localhost:27017/kundolapp'
const createConnection = url => ({"url" : url})
class DB {
    constructor(url) {
        if (!DB.instance) {
            DB.instance = createConnection(url)
        }
        return DB.instance
    }
    connect() {
        return this.instance
    }
}
const a = new DB(URL)
const b = new DB(URL)
console.log(a === b) // true
```
**싱글톤 패턴 JAVA**
```
class Singleton {
    private static class singleInstanceHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
    public static Singleton getInstance() {
        return singleInstanceHolder.INSTANCE;
    }
}

public class HelloWorld {
     public static void main(String[] args) {
        Singleton a = Singleton.getInstance();
        Singleton b = Singleton.getInstance();
        System.out.println(a.hashCode());
        System.out.println(b.hashCode());  
        if (a == b) {
         System.out.println(true);
        }
     }
}
```

#### 의존성 주입(DI, Dependency Injection)
- 메인 모듈에서 직접 가져오기 보다는, 의존성 주입 모듈을 별도로 만들어 간접적으로 의존성을 주입하는 것

### 팩토리 패턴
---
- 객체 생성 부분을 추상화한 패턴
- 부모 클래스에서 뼈대를, 자식 클래스에서 구체적인 내용을 결정
- 결합도 Down, 유연성 UP, 유지보수 UP

***팩토리 패턴 예시(JS)***
```
const num = new Object(42);
const str = new Object('abc');
console.log(num.constructor.name); // Number
console.log(str.constructor.name); // String


class Latte {
    constructor() {
        this.name = "latte"
    }
}
class Espresso {
    constructor() {
        this.name = "Espresso"
    }
}

class LatteFactory {
    static createCoffee() {
        return new Latte()
    }
}
class EspressoFactory {
    static createCoffee() {
        return new Espresso()
    }
}
const factoryList = { LatteFactory, EspressoFactory }

class CoffeeFactory {
    static createCoffee(type) {
        const factory = factoryList[type]
        return factory.createCoffee()
    }
}   
const main = () => {
    // 라떼 커피를 주문한다.
    const coffee = CoffeeFactory.createCoffee("LatteFactory")
    // 커피 이름을 부른다.
    console.log(coffee.name) // latte
}
main()
```

***팩토리 패턴 예시(JAVA)***
```
abstract class Coffee {
    public abstract int getPrice();

    @Override
    public String toString() {
        return "Hi this coffee is " + this.getPrice();
    }
}
class CoffeeFactory {
    public static Coffee getCoffee(String type, int price) {
        if ("Latte".equalsIgnoreCase(type)) return new Latte(price);
        else if ("Americano".equalsIgnoreCase(type)) return new Americano(price);
        else {
            return new DefaultCoffee();
        }
    }
}
class DefaultCoffee extends Coffee {
    private int price;

    public DefaultCoffee() {
        this.price = -1;
    }

    @Override
    public int getPrice() {
        return this.price;
    }
}
class Latte extends Coffee {
    private int price;

    public Latte(int price) {
        this.price = price;
    }
    @Override
    public int getPrice() {
        return this.price;
    }
}
class Americano extends Coffee {
    private int price;

    public Americano(int price) {
        this.price = price;
    }
    @Override
    public int getPrice() {
        return this.price;
    }
}
public class HelloWorld {
     public static void main(String[] args) {
        Coffee latte = CoffeeFactory.getCoffee("Latte", 4000);
        Coffee ame = CoffeeFactory.getCoffee("Americano", 3000);
        System.out.println("Factory latte ::" + latte);
        System.out.println("Factory ame ::" + ame);
     }
}
```

### 전략 패턴
---
- 객체의 행위(행위라 하면 메서드?)를 바꾸고 싶을 때, 직접 수정하지 않고 전략(캡슐화한 알고리즘)을 컨택스트 내부에서 바꿔주는 것

![image](https://user-images.githubusercontent.com/97939170/204916388-f74c8685-33c2-44af-8bb6-ff0492ad96f1.png)

***전략 패턴 예시(JAVA)***
```
import java.text.DecimalFormat;
import java.util.ArrayList;
import java.util.List;
interface PaymentStrategy {
    public void pay(int amount);
}

class KAKAOCardStrategy implements PaymentStrategy {
    private String name;
    private String cardNumber;
    private String cvv;
    private String dateOfExpiry;

    public KAKAOCardStrategy(String nm, String ccNum, String cvv, String expiryDate) {
        this.name = nm;
        this.cardNumber = ccNum;
        this.cvv = cvv;
        this.dateOfExpiry = expiryDate;
    }

    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using KAKAOCard.");
    }
}

class LUNACardStrategy implements PaymentStrategy {
    private String emailId;
    private String password;

    public LUNACardStrategy(String email, String pwd) {
        this.emailId = email;
        this.password = pwd;
    }

    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using LUNACard.");
    }
}

class Item {
    private String name;
    private int price;
    public Item(String name, int cost) {
        this.name = name;
        this.price = cost;
    }

    public String getName() {
        return name;
    }

    public int getPrice() {
        return price;
    }
}

class ShoppingCart {
    List<Item> items;

    public ShoppingCart() {
        this.items = new ArrayList<Item>();
    }

    public void addItem(Item item) {
        this.items.add(item);
    }

    public void removeItem(Item item) {
        this.items.remove(item);
    }

    public int calculateTotal() {
        int sum = 0;
        for (Item item : items) {
            sum += item.getPrice();
        }
        return sum;
    }

    public void pay(PaymentStrategy paymentMethod) {
        int amount = calculateTotal();
        paymentMethod.pay(amount);
    }
}  

public class HelloWorld {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();

        Item A = new Item("kundolA", 100);
        Item B = new Item("kundolB", 300);

        cart.addItem(A);
        cart.addItem(B);

        // pay by LUNACard
        cart.pay(new LUNACardStrategy("kundol@example.com","pukubababo"));

        // pay by KAKAOCard
        cart.pay(new KAKAOCardStrategy("Ju hongchul", "123456789","123","12/01"));
    }
}
/*
400 paid using LUNACard.
400 paid using KAKAOCard.
*/
```

:sparkles: 컨텍스트 : 개발자가 어떠한 작업을 완료하는 데 필요한 모든 관련 정보

#### Passport : 전략 패턴을 사용한 JS 라이브러리
- Node.js에서 인증 모듈을 구현할 때 사용하는 미들웨어 라이브러리, 일반 로그인과 소셜 로그인을 전략으로 나누어 지원

### 옵저버(observer) 패턴
---
- 주체가 특정 객체의 상태변화를 관찰하고, 상태변화가 있을 때마다 메서드 등을 통해 옵저버들에게 변화를 알려주는 것
- 주체 : 상태변화를 관찰하는 객체
- 옵저버 : 상태변화에 따라 추가 변동사항이 생기는 객체
- 예시 : 트위터 - 내가 팔로우 한 주체가 포스팅을 올리면 나에게 알림이 온다.

***옵저버 패턴 예시(JAVA)***
```
코드 위치: ch1/7.java
    import java.text.DecimalFormat;
import java.util.ArrayList;
import java.util.List;
interface PaymentStrategy {
    public void pay(int amount);
}

class KAKAOCardStrategy implements PaymentStrategy {
    private String name;
    private String cardNumber;
    private String cvv;
    private String dateOfExpiry;

    public KAKAOCardStrategy(String nm, String ccNum, String cvv, String expiryDate) {
        this.name = nm;
        this.cardNumber = ccNum;
        this.cvv = cvv;
        this.dateOfExpiry = expiryDate;
    }

    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using KAKAOCard.");
    }
}

class LUNACardStrategy implements PaymentStrategy {
    private String emailId;
    private String password;

    public LUNACardStrategy(String email, String pwd) {
        this.emailId = email;
        this.password = pwd;
    }

    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using LUNACard.");
    }
}

class Item {
    private String name;
    private int price;
    public Item(String name, int cost) {
        this.name = name;
        this.price = cost;
    }

    public String getName() {
        return name;
    }

    public int getPrice() {
        return price;
    }
}

class ShoppingCart {
    List<Item> items;

    public ShoppingCart() {
        this.items = new ArrayList<Item>();
    }

    public void addItem(Item item) {
        this.items.add(item);
    }

    public void removeItem(Item item) {
        this.items.remove(item);
    }

    public int calculateTotal() {
        int sum = 0;
        for (Item item : items) {
            sum += item.getPrice();
        }
        return sum;
    }

    public void pay(PaymentStrategy paymentMethod) {
        int amount = calculateTotal();
        paymentMethod.pay(amount);
    }
}  

public class HelloWorld {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();

        Item A = new Item("kundolA", 100);
        Item B = new Item("kundolB", 300);

        cart.addItem(A);
        cart.addItem(B);

        // pay by LUNACard
        cart.pay(new LUNACardStrategy("kundol@example.com","pukubababo"));

        // pay by KAKAOCard
        cart.pay(new KAKAOCardStrategy("Ju hongchul", "123456789","123","12/01"));
    }
}
/*
400 paid using LUNACard.
400 paid using KAKAOCard.
*/
```
