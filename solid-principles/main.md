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