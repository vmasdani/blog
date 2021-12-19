+++
title = "Modern C++ on Arduino"
[taxonomies]
tags = [ "casual", "coding", "cpp", "c", "microcontroller", "arduino", "embedded" ]
+++

Embedded programming is always different with regular high-level programming with humanlike APIs--they tend to be closer to the **metal**, and less easy for humans to read.  

However, recent developments of the C++ has some convenient features which you can use with Arduino--one of the most popular embedded system platform--and its API are quite in higher level than C or Assembly, and oftentimes use dynamic memory allocations, so it is not very good for critical applications like healthcare devices or aviations.  

But it still can be used for many, many applications other than healthcare devices or aviations, such as the field of Internet of Things, CNC machines, smart farming, smart trackers, etc etc.  

With other high level tools like [MicroPython](/blog/modern-cpp-arduino/), [Go for Embedded](/blog/modern-cpp-arduino/), and [Embedded Rust](/blog/modern-cpp-arduino/) on the rise, and more powerful microcontrollers like ESP8266, ESP32-S2 and ESP32-C3 which offers many, many hardware resources (like 160MHz clock and 512KB-ish SRAM), people are becoming less afraid of using dynamic memory allocations in a microcontroller.

Let's just jump into the ride and see what the modern C++ has to offer!

**Content below are still WIP**

### The `auto` keyword

```cpp
String newString = String();
```

```cpp
auto newString = String();
```

```cpp
auto myInt = 1;
auto myFloat = 3.30;
auto str = "Hello" + ", world!";
```

### Lambda function and closure
```cpp
void setup() {
    auto inScope = "I am still in scope.";

    auto func = [&inScope](String& text, int arg) { 
        Serial.println(
            "Hello, " + 
            text + 
            "!, I have " +
            arg + 
            " apples."
        );

        Serial.println(inScope);

        inScope += " yay!";

        return inScope;
    };

    auto res = func("world", 5);
    Serial.println(res);
}
```

```cpp
String func(String& inScope, String& text, int arg) { 
    Serial.println(
        "Hello, " + 
        text + 
        "!, I have " +
        arg + 
        "apples."
    );

    Serial.println(inScope);

    inScope += " yay!";
}

void setup() {
    auto inScope = "I am still in scope.";

    func(inScope, "world", 5);

    Serial.println(inScope);
}
```

### String reference
```cpp
void (String& builder) {
    builder += "string ";
    builder += "to ";
    builder += "build.";
}
```

### `foreach` loop
```cpp
auto contents = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

foreach (auto item : contents) {
    Serial.println(item);
}
```

### Class, Enum class, and Template
```cpp
enum class StepperDirection {
    CLOCKWISE,
    COUNTERCLOCKWISE
}

class Stepper {
public:
    Stepper(int pinA, int pinB) {
        
    }

    void move(StepperDirection direction, int steps) {
        switch (direction) {
            case StepperDirection::CLOCKWISE:
                // Toggle digital pins to move stepper clockwise here for n steps
                break;

            case StepperDirection::COUNTERCLOCKWISE:
                // Toggle digital pins to move stepper counterclockwise here for n steps
                break;
        }
    }
};

void setup() {
    Stepper stepper(PIN_A, PIN_B);

    stepper.move(StepperDirection::CLOCKWISE, 500);
    stepper.move(StepperDirection::COUNTERCLOCKWISE, 200);    
}
```

## Example refactors

See how using modern C++ tidies up your code:

#### From this:
```cpp
digitalWrite(1, LOW);
delay(500);

digitalWrite(1, HIGH);
delay(500);

digitalWrite(2, LOW);
delay(500);

digitalWrite(2, HIGH);
delay(500);
```

#### To this
```cpp
auto pins = {1, 2};

foreach (auto pin : pins) {
    // Turn LED on
    digitalWrite(pin, HIGH);
    delay(500);

    // Turn LED off
    digitalWrite(pin, LOW);
    delay(500);
}
```

## Verdict