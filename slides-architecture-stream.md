---
# You can also start simply with 'default'
theme: default
title: Tidy First
info: |
  ## Tidy First
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# https://sli.dev/guide/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations#slide-transitions
transition: none
# enable MDC Syntax: https://sli.dev/guide/syntax#mdc-syntax
mdc: true
layout: image-left
image: /fowler.jpg
class: text-2xl
---

# Definition

**Refactoring (Verb)**: Umstrukturierung von Software durch Anwendung einer Reihe von Refactorings, ohne das beobachtbare Verhalten der Software zu ändern.

**Refactoring (Substantiv)**: eine Änderung der internen Struktur einer Software, um sie verständlicher und einfacher modifizierbar zu machen, ohne das beobachtbare Verhalten zu verändern.


<p>&mdash; Martin Fowler</p>

---
layout: image-right
image: /beck.jpeg
class: text-2xl
---

"Refactoring" hat ernsthaften Schaden genommen, als die Leute begannen, damit lange Pausen bei der Feature-Entwicklung zu erklären.

Dabei verschwand sogar der Teil mit dem »Verhalten beibehalten«, sodass ein Refactoring das System problemlos aus dem Tritt bringen kann. Keine neuen Features, möglicher Schaden und nichts, was man am Ende vorzeigen kann. Danke, ich verzichte.

<p>&mdash; Kent Beck</p>

---
layout: image
image: /burn.jpg
---
---
layout: image-right
image: images/fuzzy.jpg
class: text-2xl
---

Aufräumereien sind eine Untermenge der Refactorings. Sie sind niedliche, knuddelige kleine Refactorings, die niemand wirklich hassen kann.

<p>&mdash; Kent Beck</p>

<!--

Tidyings sind wirklich wirklich klein!

es hilf evtl in manchen Kontexten nicht von Refactoring zu sprechen
Wenn Du das Problem nicht hast, kann Du gerne weiterhin von Refactoring sprechen.

-->

---
layout: cover
background: /title.jpg
class: text-center
---

# Tidyings!

<img src="/tidyings.png" absolute top-0 right-0 w-90>


---
zoom: 1.2

---


<v-click>

# Guard Clauses

</v-click>

````md magic-move {lines: true}
```ts{*}{class:'!children:text-[18px]'}
function calculateDiscount(price: number, isHoliday: boolean, isVIP: boolean) {
    let discount = 0;
    if (isHoliday) {
        if (!isVIP) {
            discount = price * 0.1;
        }
    }
    return discount;
}
```

```ts
function calculateDiscount(price: number, isHoliday: boolean, isVIP: boolean) {
    if (!isHoliday) return 0;
    if (isVIP) return 0;

    return price * 0.1;
}
```


````

---

# Guard Clauses

<div></div>

```ts {*}{class:'!children:text-[14px]'}
function calculateDiscount(price: number, isHoliday: boolean, isVIP: boolean) {
    let discount = 0;
    if (isHoliday) {
        if (!isVIP) {
            discount = price * 0.1;
        }
    }
    return discount;
}
```

<arrow  x1="350" y1="280" x2="350" y2="382" color="#953" width="2" z-40 arrowSize="1" />

<div mt-20>

```ts{*}{class:'!children:text-[14px]'}
function calculateDiscount(price: number, isHoliday: boolean, isVIP: boolean) {
    if (!isHoliday) return 0;
    if (isVIP) return 0;

    return price * 0.1;
}
```

</div>


---
layout: two-cols
---
<v-click>

# Dead Code

</v-click>

<div w-105>

````md magic-move {lines: true}

```ts
class BankAccount {
    private balance: number = 0;
    private transactionHistory: number[] = [];

    private calculateAverageTransaction(): number {
        const sum = this.transactionHistory.reduce(
          (a, b) => a + b, 0);
        return sum / this.transactionHistory.length;
    }

    public getBalance(): number {
        return this.balance;
    }

    public printStatementFormA(): void {
        console.log("Balance: ", this.getBalance());
        console.log("Average Transaction: ",
          this.calculateAverageTransaction());
    }

    public printStatementFormB(): void {
        console.log("Balance: ", this.getBalance());
        console.log(account.getTransactionHistory());
    }

```

```ts
class BankAccount {
    private balance: number = 0;
    private transactionHistory: number[] = [];

    public getBalance(): number {
        return this.balance;
    }

    public printStatementFormB(): void {
        console.log("Balance: ", this.getBalance());
        console.log(account.getTransactionHistory());
    }

```

````
</div>


::right::

```ts
    public deposit(amount: number): void {
        this.balance += amount;
        this.transactionHistory.push(amount);
    }

    public getTransactionHistory(): number[] {
        return this.transactionHistory;
    }

    public withdraw(amount: number): void {
        if (amount <= this.balance) {
            this.balance -= amount;
            this.transactionHistory.push(-amount);
        } else {
            console.log("Insufficient balance");
        }
    }
}

const specialTransactionPrint =
    (inValueFromT1, inValueFromT2, outValueFromT3) => {
        const account = new BankAccount();
        account.deposit(inValueFromT1);
        account.deposit(inValueFromT2);
        account.withdraw(outValueFromT3);
        printStatementFormB();
}
```

```ts
    public deposit(amount: number): void {
        this.balance += amount;
        this.transactionHistory.push(amount);
    }

    public getTransactionHistory(): number[] {
        return this.transactionHistory;
    }

    public withdraw(amount: number): void {
        if (amount <= this.balance) {
            this.balance -= amount;
            this.transactionHistory.push(-amount);
        } else {
            console.log("Insufficient balance");
        }
    }
}

const specialTransactionPrint =
    (inValueFromT1, inValueFromT2, outValueFromT3) => {
        const account = new BankAccount();
        account.deposit(inValueFromT1);
        account.deposit(inValueFromT2);
        account.withdraw(outValueFromT3);
        printStatementFormB();
}
```


---
zoom: 1.2
---

<v-click>

# Normalize Symmetries

</v-click>

````md magic-move {lines: true}
```java
public class Person {
...
    // Using string concatenation
    public String getFullName() {
        return firstName + " " + lastName;
    }

    // Using String.format
    public String getLocation() {
        return String.format("%s, %s", city, country);
    }

    // Using StringBuilder
    public String getDetailedInfo() {
        return new StringBuilder(firstName).append(" ").append(lastName)
                .append(" from ").append(city).append(", ").append(country).toString();
    }
}
```

```java
public class Person {
...
    public String getFullName() {
        return firstName + " " + lastName;
    }

    public String getLocation() {
        return city + ", " + country;
    }

    public String getDetailedInfo() {
        return firstName + " " + lastName + " from " + city + ", " + country;
    }
}
```
````
---
layout: default
zoom: 1.1
---

<div mt--10>

```java
    // Using string concatenation
    public String getFullName() {
        return firstName + " " + lastName;
    }
    // Using String.format
    public String getLocation() {
        return String.format("%s, %s", city, country);
    }
    // Using StringBuilder
    public String getDetailedInfo() {
        return new StringBuilder(firstName).append(" ").append(lastName)
                .append(" from ").append(city).append(", ").append(country).toString();
    }
```
</div>

<arrow  x1="350" y1="245" x2="350" y2="300" color="#953" width="2" z-40 arrowSize="1" />


<div mt-10>

```java
    public String getFullName() {
        return firstName + " " + lastName;
    }

    public String getLocation() {
        return city + ", " + country;
    }

    public String getDetailedInfo() {
        return firstName + " " + lastName + " from " + city + ", " + country;
    }
```

</div>

---

# New Interface, Old Implementation

```python
class Database:
    def connect(self, url, username, password):
        ...

    def execute_query(self, query):
        ...

    def disconnect(self):
        ...
```
<arrow  x1="350" y1="245" x2="350" y2="310" color="#953" width="2" z-40 arrowSize="1" />

<div mt-10>

```python
    def connect(self, url, username, password):
    def execute_query(self, query):
    def disconnect(self):

    def execute_query(self, url, username, password, query):
        self.connect(url, username, password)
        self.execute_query(query)
        self.disconnect()
```
</div>

#### Usage
```python
db = Database()
db.execute_query("url", "username", "password", "SELECT * FROM table")
```

---
zoom: 1.2
---

<v-click>

# Reading Order

</v-click>

````md magic-move {lines: true}
```python
class Car:
    def start_engine(self):
        if self.check_battery():
            print("Engine started.")
        else:
            print("Engine failed to start. Battery is dead.")

    def __init__(self, make, model, year, color):
        self.make = make
        self.model = model
        self.year = year
        self.color = color

    def check_battery(self):
        # For simplicity, let's assume the battery is good if the car is less than 5 years old
        return datetime.datetime.now().year - self.year < 5
```

```python
class Car:
    def __init__(self, make, model, year, color):
        self.make = make
        self.model = model
        self.year = year
        self.color = color

    def check_battery(self):
        # For simplicity, let's assume the battery is good if the car is less than 5 years old
        return datetime.datetime.now().year - self.year < 5

    def start_engine(self):
        if self.check_battery():
            print("Engine started.")
        else:
            print("Engine failed to start. Battery is dead.")

```
````


---
zoom: 1.1
---

## Cohesion Order


````md magic-move {lines: true}
```python
class ShoppingCart:
    def __init__(self):
        self.items = []

    def apply_discount(self, code):
        if self.is_valid_discount_code(code):
            total = self.calculate_total()
            print(f"Total after discount: {total * 0.9}")
        else:
            print("Invalid discount code.")

    def add_item(self, name, price, quantity):
        self.items.append({'name': name, 'price': price, 'quantity': quantity})

    def calculate_item_total(self, item):
        return item['price'] * item['quantity']

    def calculate_total(self):
        ...

    def is_valid_discount_code(self, code):
        return code.startswith("DISC")

```

```python
class ShoppingCart:
    def __init__(self):
        self.items = []

    def add_item(self, name, price, quantity):
        self.items.append({'name': name, 'price': price, 'quantity': quantity})

    def calculate_item_total(self, item):
        return item['price'] * item['quantity']

    def calculate_total(self):
        ...

    def is_valid_discount_code(self, code):
        return code.startswith("DISC")

    def apply_discount(self, code):
        if self.is_valid_discount_code(code):
            total = self.calculate_total()
            print(f"Total after discount: {total * 0.9}")
        else:
            print("Invalid discount code.")
```
````


---

# Move Declaration an Initialization Together


```ts
    let total: number;
    // ...some code that doesn't use total
    total = 0;
    for (let item of items) {
        let price: number;
        // ...some more code, maybe it uses total but doesn't use price
        price = item.price;
        total += price * item.quantity;
    }
    return total;
```

<arrow  x1="350" y1="270" x2="350" y2="380" color="#953" width="2" z-40 arrowSize="1" />

<div mt-20>

```ts
    let total = 0;
    // ...some code that doesn't use total
    for (let item of items) {
        let price: number;
        // ...some more code, maybe it uses total but doesn't use price
        price = item.price;
        total += price * item.quantity;
    }
    return total;
```
</div>

---

<v-click>

# Explaining Variables

</v-click>

```python
def calculate_point():
    return (math.sqrt(16) * math.sin(math.radians(45)), math.sqrt(16) * math.cos(math.radians(45)))
```


<v-click>

<arrow  x1="350" y1="140" x2="350" y2="220" color="#953" width="2" z-40 arrowSize="1" />

<div mt-20>


```python
def calculate_point():
    radius = math.sqrt(16)
    angle_in_radians = math.radians(45)

    x = radius * math.sin(angle_in_radians)
    y = radius * math.cos(angle_in_radians)

    return (x, y)
```

</div>

</v-click>

---

# Explaining Constants

```ts
function handleResponse(response: {code: number, data: any}): void {
    if (response.code === 404) {
        console.log("Page not found.");
    } else if (response.code === 200) {
        console.log("Success!");
    }
}
```

<v-click>

<arrow  x1="350" y1="240" x2="350" y2="320" color="#953" width="2" z-40 arrowSize="1" />

<div mt-20>

```ts
const PAGE_NOT_FOUND = 404;
const SUCCESS = 200;

function handleResponse(response: {code: number, data: any}): void {
    if (response.code === PAGE_NOT_FOUND) {
        console.log("Page not found.");
    } else if (response.code === SUCCESS) {
        console.log("Success!");
    }
}
```


</div>

</v-click>


---

# Explicit Parameters

```java
    public static void main(String[] args) {
        HashMap<String, Integer> params = new HashMap<>();
        params.put("width", 10);
        params.put("height", 20);
        calculateArea(params);
    }
    static void calculateArea(HashMap<String, Integer> params) {
        int area = params.get("width") * params.get("height");
        System.out.println("Area: " + area);
    }
```

<arrow x1="350" y1="270" x2="350" y2="340" color="#953" width="2" z-40 arrowSize="1" />

<div mt-10>

```java
    public static void main(String[] args) {
        HashMap<String, Integer> params = new HashMap<>();
        params.put("width", 10);
        params.put("height", 20);
        calculateArea(params.get("width"), params.get("height"));
    }
    static void calculateArea(int width, int height) {
        int area = width * height;
        System.out.println("Area: " + area);
    }
```

</div>


---
layout: image-left
image: /title.jpg
---

# More Tidyings

* Chunk Statements
* Extract Helpers
* One Pile
* Explaining Comments
* Delete Redundant Comments
* even more...

---
layout: cover
background: /title.jpg
class: text-center
---

# Tidyings!

<img src="/tidyings.png" absolute top-0 right-0 w-90>

