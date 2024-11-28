When modelling a circle and an ellipse, mathematically, a circle is a specific type of ellipse, but modelling circle as a subclass can lead to issues

## Problem
- **Ellipse**: An ellipse has two radii (semi-major and semi-minor axes), allowing it to stretch along two axes independently.
- **Circle**: A circle is a special case of an ellipse where both radii are equal, so it only needs one radius.

Modelling this with OOP:
```cpp
class Ellipse {
public:
	virtual void setRadii(double r1, double r2) {
		this->r1 = r1;
		this->r2 = r2;
	}
protected:
	double r1, r2;
};

class Circle : public Ellipse {
public:
	void setRadius(double radius) {
		// Ensure both radii are the same
		setRadii(radius, radius);
	}
};
```

However, this violates <u>Liskov Substitution Principle</u>
>Functions using pointers/reference to base classes must also be able to use objects of derived classes without knowing

Example of violation:
```cpp
Ellipse* shape = new Circle(); 
shape->setRadii(5.0, 10.0); // Circle can't fulfill this properly
```

## Possible Solutions
- **Separate Classes**: Have `Circle` and `Ellipse` as separate, unrelated classes, each implementing a common `Shape` interface if needed. This way, each class maintains its own distinct behavior without imposing incompatible constraints.
```cpp
class Shape { /* common shape behavior */ };
class Circle : public Shape { /* circle-specific methods */ };
class Ellipse : public Shape { /* ellipse-specific methods */ };
```

- **Composition Over Inheritance**: If you need a relationship, consider using composition instead of inheritance, where `Circle` could internally contain an `Ellipse` object and ensure it behaves consistently as a circle.

- **Parameterize the Shape Type**: Use a general shape class where properties are configurable based on parameters, allowing flexibility without rigid sub-classing.