# SOLID Principles
Is software engineering an actual engineering discipline? Proponents of the argument that it is not contend this is because software engineers aren't "designated", meaning that there's no board that accredits software engineers. Oxford dictionary defines engineering as "_the action of working artfully to bring something about_". Solid principles add to the artful aspect of software engineering, by codifying how software maintainability and efficiency may be achieved via high cohesion and other best practices. 

## The Single Responsibility Principle
The single responsibility principle states that a software component (ideally a class), must have one concrete responsibility. Consider the following `Invoice` class:

```python
class Invoice:
    def __init__(self, items):
        self.items = items

    def calculate_total(self):
        return sun(item["price"] for item in self.items)

    def print_invoice(self):
        for item in self.items:
            print(f"{item['name']}: {item['price']}")
        print(f"Total: {self.calculate_total()}")
```

At a glance, the looks like a standard implementation. In many situations, this would work. However, this implementation is not artful as it misses a core principle of clean code implementation. A closer look reveals that our `Invoice` class has more that one responsibility. This can introduce various vulnerability to the class. The `Invoice` class will have to change if we need to change both printing or total calculation which may affect other parts of the software implementation. In addition, this implementation complicates testing and extension of our class.

A better implementation splits the responsibility between two classes.

```python
class Invoice:
    def __init(self, items):
        self.items = items

    def calculate_total(self):
        return sum(item["price"] for item in self.items)

class InvoicePrinter:
    def __init(self, invoice):
        self.invoice = invoice

    def print_invoice(self):
        for item in self.invoice.items:
            print(f"{item['name']}: {item['price']}")
        print(f"Total: {self.invoice.calculate_total()}")
```

## The Open/Closed Principle
The Open/Closed Principle states that our software components should simultaneously be open and closed but with respect to different aspects. Our components should be open to extension but closed to modification. This means that we should be able to introduce new functionality without breaking existing code. Suppose we have the implementation below to calculate the total area of different shapes. Suppose also that we need to only include a portion of the area of a circle via a supplied `ratio` value which defaults to 1 in which case we consider the full circle.

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def calculate_area(self):
        return self.width * self.height

class Circle:
    def __init__(self, radius):
        self.radius = radius

    def calculate_area(self, ratio = 1):
        return 3.14 * ratio * self.radius ** 2

class Area:
    # Circle ratio
    ratio = .88
    def __init__(self, shapes):
        self.shapes = shapes

    def total_area(self):
        total_area = 0
        for shape in self.shapes:
            if isinstance(shape, Circle):
                total_area += shape.calculate_area(shape.ratio)
            elif isinstance(shape, Rectangle):
                total_area += shape.calculate_area()
```

This issue with the above implementation is that every time we needed to add a new shape, we would have to modify the `total_area` method in the `Area` class, meaning that the `Area` class is open to modification. This will introduce maintainability perils as we may also need to modify the components using the `Area` class's `total_area` method. A better implementation uses OOP polymorphism to to adhere to Open/Closed Principle. Consider the following implementation:

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def calculate_area(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def calculate_area(self):
        return self.width * self.height

class Circle(Shape):
    ratio = .88
    def __init__(self, radius):
        self.radius = radius
    
    def calculate_area(self):
        3.14 * ratio * self.radius ** 2 

class Triangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def calculate_area(self):
        return .5 * self.width * self.height

class Area:
    def __init__(self, shapes):
        self.shapes = shapes

    def total_area(self):
        return sum(shape.calculate_area() for shape in self.shapes)
```

In the above implementation, to add a new shape (triangle for example), we simply need to add a new shape class rather than modifying our `Area`.

```python
class Triangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def calculate_area(self):
        return .5 * self.width * self.height
```

This way, the Open/Closed principle is obeyed.

