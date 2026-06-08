# Software Design Comparison: Modifying Classes vs. Using Interfaces (OCP)

### 1. Risk of Breaking Existing Features
* **Modifying `ProductFilter`**: Every time you open an existing class to edit it, you risk accidentally breaking features that already work. You might introduce a syntax error, change a variable name, or disrupt existing tests.
* **Creating a New Class**: Your existing code remains completely untouched. You write your new rule in isolation. It is physically impossible to break old code because you are not touching it.

### 2. The Test-Once Principle
* **Modifying `ProductFilter`**: If you add code to `ProductFilter`, you must re-test the entire class and every single feature that depends on it to ensure nothing broke (regression testing).
* **Creating a New Class**: You only need to write tests for the new class. Your old classes are already compiled, tested, and locked away.

### 3. Maintainability (Avoiding "God Classes")
* **Modifying `ProductFilter`**: As your business grows, `ProductFilter` will grow into a massive, thousands-of-lines-long "God Class." It becomes terrifying to maintain because everyone is editing the same file.
* **Creating a New Class**: Your code stays modular. Each class has one job (Single Responsibility). If you need to fix a bug in the color filter, you look at `ColorSpecification`, which is only 15 lines long.

### 4. Dynamic Combinations at Runtime
With the interface approach, you can combine rules dynamically without writing code for the combination. If you want to filter for **"Green AND Large AND Cheap"**, look at how you do it with both approaches:

* **Class Approach (Bad)**: You have to manually write a brand new method inside the class: 
  ```java
  filterByColorAndSizeAndPrice(...)
  ```
* **Interface Approach (Good)**: You do not write any new methods. You just chain your existing pieces together at runtime like LEGO blocks:
  ```java
  new AndSpecification<>(
      new ColorSpecification(Color.GREEN),
      new AndSpecification<>(
          new SizeSpecification(Size.LARGE),
          new PriceSpecification(Price.CHEAP)
      )
  )
  ```
