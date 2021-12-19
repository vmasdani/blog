+++
title = "Top 5 Programming Language for Serious Backend Programmer"
[taxonomies]
 tags = [ "coding", "web", "backend" ]
+++

There are tons upon tons of programming languages in the market nowadays, and it might be overwhelming for people who just started programming. This question is often asked among the beginner programmers:

**How do we pick the right programming language?**

Here's the thing about picking the most powerful, best ever programming language:

1. You can't
2. It depends

Why **can't you**? Because there are always a few considerations in picking a programming language for a certain field of work/expertise. In my experience, some of them includes:

1. **Popularity**  
   Is the programming used by many people? Does it have big community? Are big companies using it, supporting it, and endorsing it?  
   Popularity should be a strong factor in deciding which tech to use, because the bigger the community is, the easier for you to find a solution to a sometimes weird and quirky problem in a programming language, compiler, tooling, etc. You don't want to spend a few days trying to fix a weird bug when the fix is actually just adding a semicolon or comma, adding a single configuraton line, or simple stupid things that can cost you precious time when trying to fix it.

2. **Demand**  
   This one applies if you are looking for work related to a programming language. You do not want to learn a language which has little no none vacancies if you are looking for a paying job to put food on your table.

3. **Local community**  
   Sometimes it's best to learn some tech within a local community in your area (city, country, university, etc), because in some cases you can share your knowledge easily with someone near your area, brainstorm ideas more clearly in person, and comfortability in talking to a person who you might be able to relate to in general.

4. **Paradigm & use cases**  
   Always use the right tool for the right job. For example, you don't want to use low level languages (C, C++, Rust) to make a prototype of a proof of concept quickly, we have high level languages (Javascript, Python, Go) for that. And you also don't want to use a high level language for something like a smart watch, set top boxes, or a micro-controller because they tend to have very little memory, and in this case low level language is better than the high level ones.

And now we are going to be reviewing 5 top programming languages according to the considerations above which works best for backend web programming.

## 4. Spring Boot + Kotlin

- **Typing**: Strong, static
- **Founder**: Jetbrains (Kotlin), VMWare (Spring)
- **Paradigm**: multi paradigm, mainly object oriented

If you are looking for a "powerful" web backend solution which has been around for a long time and you want to build an enterprise web application for very cheap, enter Spring and Kotlin.

Spring boot is a very mature open source web framework which runs on top of the [JVM (Java Virtual Machine)](https://en.wikipedia.org/wiki/Java_virtual_machine) and is developed by the team at VMWare. It provides a complete solution for building web applications and utilizes modern web technologies such as **HATEOAS** & **microservices gateway** (Netflix Zuul & Eureka server) for data processing/presentation, and server templating engine such as [Thymeleaf](https://www.thymeleaf.org/) or [JSP](https://www.thymeleaf.org/).

While primarily designed for **Java** in the userland aspect, **Kotlin** support in Spring Boot is fantastic. I think what the Java flavored Spring/Spring Boot framework lacks is modern programming language features like type inference, null safety, immutable struct/classes/variables, declarative queries, expression-based syntax etc in which Kotlin has all of them.

On top of the modern programming language features, Kotlin has 100% interoperability with Java, which makes Spring Boot also 100% compatible with Kotlin!

Here's how a Spring Boot REST API project in Kotlin typically looks like:

- Model

```kotlin
@Entity
data class Person(
   @Id
   @GeneratedValue(strategy=GenerationType.AUTO)
   var id: Long? = null,
   var name: String? = null,

   @ManyToOne
   var department: Department? = null,

   @CreationTimestamp
   var createdAt: Instant? = null
   @UpdateTimestamp
   var updatedAt: Instant? = null
)
```

- Repository

```kotlin
interface PersonRepository : JpaRepository<Person, Long>, JpaSpecificationExecutor<Person> {
}
```

- Controller

```kotlin
@RestController
class PersonController(
   private val environment: Environment,
   private val personRepository : PersonRepository
) {
   @GetMapping("/people")
   fun all() = personRepository.findAll()

   @GetMapping("/people/{id}")
   fun get(@PathVariable id: Long) = personRepository.findById(id)

   @PostMapping("/people")
   fun post(@RequestBody person: Person)
      = ResponseEntity.status(HttpStatus.CREATED).body(personRepository.save(person))
}
```

Writing Spring Boot in Kotlin is very productive. Many of the reasons are because of:

- **function return inference** which can lead to writing very concise controller functions.
- **dataclasses**
  which enables you to create a struct/record data type without **getters** and **setters**. Although you can generate getters and setters through most JVM-based IDEs like **IntelliJ Idea Community** and **Eclipse IDE**, not having to write getters and setters is very intuitive and not time-consuming.
- **declarative utility functions** such as **map, filter, fold** which Java does not (natively) have, and makes writing loops very, very concise.

```kotlin
// find something in array
val found = people.find { p -> p.id == selectedID }

// filter people more than 22 years old
val ageTwentyTwo = people.filter { p -> p.age > 22 }

// map people's ages
val onlyAges = people.map { p -> p.age }

```

- Lastly, one of the most important benefit of writing in Kotlin is **null safety**. Rolling-release project model which has very fast-changing RDBMS is very prone to error, and the data that is returned from the RDBMS might not always have a value, which might be a result of **column update**. But the null safety feature in Kotlin prevents your program from getting a random **null pointer exception** which you might will often encounter while writing Java due to Java not having nullable types, so it assumes that every variables are nullable by default, so you can't accidentally unpack a property which is actually **null** while writing Kotlin.

However, as good as Spring + Kotlin is, I would not choose Spring + Kotlin or any JVM based runtime to create a new web-based project these days, because of:

1. **Memory constraints**: JVM apps are notorious for having very big memory usage
2. **Asynchronous routines not first-class**. Although Kotlin has a neat green threads/coroutines system in the `kotlinx` library, Spring/Spring Boot does not natively support them, and setting them up is painful if you are not used to JVM project management system such as Gradle/Maven

If you or your company already has a Spring Boot project with Java, I really, really recommend migrating to Kotlin because the developer experience and productivity is like night and day.

## 4. Golang

- **Typing**: Strong, static
- **Founder**: Google
- **Paradigm**: multi paradigm, mainly functional & imperative

Golang is a relatively new programming language made by Google. Its original purpose was to replace C/C++ for some parts of internal Google code because there were too much boilerplate and the codebase was becoming too verbose. It has now become one of the most used programming language for web backend and CLI tools such as big companies, project, and/or services like Docker, Gojek, Kubernetes, yay (arch linux AUR helper) etc.

See [Golang's FAQ](https://golang.org/doc/faq#creating_a_new_language)

> Go was born out of frustration with existing languages and environments for the work we were doing at Google. Programming had become too difficult and the choice of languages was partly to blame. One had to choose either efficient compilation, efficient execution, or ease of programming; all three were not available in the same mainstream language. Programmers who could were choosing ease over safety and efficiency by moving to dynamically typed languages such as Python and JavaScript rather than C++ or, to a lesser extent, Java.

Golang was built with simplicity, and it is **very pragmatic**. Meaning that a lot of actions you do while writing in the language affects your program's behaviour.

For example, in Golang you do not have to explicitly declare that a variable, function, or struct is exported or not exported like you do in javascript/typescript.

```js
// Exported
export const formatDateTime = () => {};

// Not exported
const formatDateTime = () => {};
```

Instead, you use **uppercase first letter** to export something.

```go
// Exported
func FormatDateTime() {

}

// Not exported
func formatDateTime() {

}
```

and there are many more pragmatic language designs and shorthand/sugar syntaxes. Such as variable declaration:

```go
// these both are the same
var myVar int = 0
myVar := 0
```

If you have written Python before, you might notice that variable declaration is the same in both python and go, just write the variable name and then assign it to a value, like `myVar := 0`. But the difference is that Go **infers** the variable type of a value, meaning that Go knows the type of the value zero (0) and will automatically assign zero as `int`. This is a modern programming language compiler technique called [Type inference](https://en.wikipedia.org/wiki/Type_inference), while Python uses [Duck typing](https://en.wikipedia.org/wiki/Duck_typing).

Creating an HTTP server with router is very easy in Golang. Using the `gorilla/mux` package, we can write a barebone REST server as simple as this:

```go
package main

import (
    "net/http"
    "log"
    "github.com/gorilla/mux"
)

func YourHandler(w http.ResponseWriter, r *http.Request) {
    w.Write([]byte("Gorilla!\n"))
}

func main() {
    r := mux.NewRouter()
    r.HandleFunc("/", YourHandler)
    log.Fatal(http.ListenAndServe(":8000", r))
}
```

And there are also projects like [GORM](https://gorm.io/index.html) for database object relational mapper and [HTTP frameworks](https://github.com/mingrammer/go-web-framework-stars) such as Gin Gonic, Echo, Fiber, Beego, etc. The web programming community in Golang is quite rich. You can even implement your own http web server with built-in Golang packages easily from the ground up using `net/http` module.

Golang is a great language, but the reason why Go scored very low in this list is because of two things that I find very obnoxious about Golang:

1. **Error handling, `if err != nil` hell**  
   Golang has a very innovative error catching system, because of the **multiple return value** functionality, you can return an error alongside data as a return type in a function, in contrast to many programming languages which still use `try/catch/finally` mechanism. For example if we want to convert string to int, we can execute:

```go
numStr := "35"
num, err := strconv.Atoi(numStr)

if err != nil {
   panic("Error converting " + numStr + "!" )
}

fmt.Println("Your number:", num)
```

This seems to work really well, protects your program from unnecessarily hair-pulling bugs and is very convenient so that errors can be caught early in compile-time. However, if you have **many functions that has error return values**, you have to write something like this:

```go
numStr := "35"
num, err := strconv.Atoi(numStr)

if err != nil {
   panic("Error converting " + numStr + "!" )
}

fmt.Println("Your number:", num)

resultOne, err := DoSomethingwithNum(num)

if err != nil {
   panic("Error doing something to num!" )
}

resultTwo, err := DoAnotherThingwithNum(resultOne)

if err != nil {
   panic("Error doing another thing to num!" )
}
```

So many `if err != nil`!

2. **Lack of `map, filter, reduce, foreach` in built-in golang std library**  
   Golang **does not have** the map, reduce, filter, foreach functionality in its std library. Which is a bit of a dealbreaker for me because most modern programming language has this feature, I tried very hard to adapt to the fact that golang does not have these features in the language, but I just could not get comfortable with the fact to this date.

In Javascript/Typescript, you can easily do this:

```js
employees.find((e) => e.id === selectedId)?.name;
```

But in Go, since there is no `Find(arr, func)` function built into the language, I have to manually loop for each items and break if I found the item.

```go
var employee Employee
var found = false

for _, e := range employees {
   if e.ID == selectedId {
      employee = e
      found = true
      break
   }
}

if found {
   fmt.Println("Employee name:", employee.Name)
}
```

Or you can use the [go-funk](https://github.com/thoas/go-funk) utility library:

```go
import (
	"fmt"

	"github.com/thoas/go-funk"
)

...

r, ok := funk.Find(employees, func(e Employee) bool {
   return e.Name == "Valian"
}).(Employee)

if !ok {
   fmt.Println("Employee not found.")
   return
}

fmt.Println("Employee found", r)
```

3. **Lack of generics**  
   One of the programming principle that you should hold dear is **DRY**, a.k.a. **Don't Repeat Yourself**, which means tedious tasks should be minimized as much as possible by utilizing the power of a programming language whenever possible. One of the concepts used for reducing code repetition is **generics**.  
   Generics allow you to define **types** to programming elements such as functions or classes so that th element can behave according to the type you have defined.  
   For example, if you want to create a function which serializes an object to indented JSON in Dart, you can write this:

```dart
String serializeIndented<T>(T json) {
  return JsonEncoder.withIndent(" ").convert(json);
}

// invocation
serializeIndented<Person>(person);

// or by type inference
serializeIndented(person);

```

But in Go, you can't supply a generic. Instead, you have to rely on **empty interfaces** which is basically a dynamic type that you can cast.

```go
func SerializeIndented(i interface{}) ([]byte, error) {
   return json.MarshalIndent(i, "", " ")
}

// invocation
ser, err := Serialize(person)
```

Works, but really uncomfortable to use in big projects which has many complex types.

Despite all these cons, Go is a really, really nice language which balances between developer experience which makes writing code fast and safe, but also does not compromise performance--meaning that code written in Go is really fast, and very robust if your error handling game is played correctly. Memory usage is very lightweight too with Go. Definitely a correct option to pick Go for creating new projects in 2021. However, there are still many languages I haven't mentioned which you should really consider before picking Go.

## 3. Typescript NodeJS

- **Typing**: Strong, static
- **Founder**: Ryan Dahl (NodeJS), Typescript (Anders Hejlsberg/Microsoft)
- **Paradigm**: multi paradigm, mainly functional

**NodeJS** is probably one of the most common tools used in the backend web programming community, and that is not wihout reason. There are tons upon tons of free services providing NodeJS-based runtime because the runtime/language is **that** versatile and lightweight, and I think should be crowned the **write once, run anywhere** title--rather than Java--because Javascript/Node does run everywhere.

One of the most popular tool for creating web HTTP APIs for Node.JS is **Express**. It's an unopinionated web framework which is designed to create web APIs with great freedom, and the setup is very, very simple. Here is an example Express project:

```js
const express = require("express");
const app = express();
const port = 3000;

app.get("/", (req, res) => {
  res.send("Hello World!");
});

app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`);
});
```

The above code will spin up an HTTP server on port 3000 inside the machine you are running your code in.

To send JSON, just use the `res.json()` function:

```js
app.get("/", (req, res) => {
  res.send({
    hello: "world!",
  });
});
```

Very, very simple.

One of the greatest thing about using NodeJS for backend server is being able to use **Typescript**. Dynamic languages like PHP or Python nowadays have something called **type hinting** in order for developers to be able to describe types for an object/variable/function return type. But in my opinion, a simple type hint feature is not really that powerful, and only helps developers in the development process. In the actual program runtime, these "types" are bound to change dynamically and oftentimes the developers are not aware of the "change" due to internal bugs or bugs from external APIs.

Typescript took the "weak type hinting" problem to a whole different level. Instead of just type hinting, Typescript has a full-blown **compiler** which really catches type errors in a runtime, assuming that your program is fully written in Typescript. If you have some Javascript in your code, or there's a library you are using which still uses Javascript but you want to use it in a Typescript program, there's an **escape hatch** for you to use Javascript in a Typescript application, by using the `any` type.

Typescript is actually a **transpiler** which converts your Typescript files to Javascript files before putting it in a NodeJS runtime. However, you can directly run a Typescript project directly in NodeJS by using `ts-node`, which setup is as simple as this:

1. Create a NodeJS project

```sh
npm init
```

2. Install NPM dependencies

```
npm i @types/node @types/express ts-node
```

3. Create an `index.ts` file containing:

```ts
import express from "express";

const app = express();

app.get("/", (req, res) => {
  res.send({ hello: "world!" });
});

app.listen(3000);
```

4. Add `start` script to `package.json` file

```json
...
"scripts": {
   "test": "echo \"Error: no test specified\" && exit 1",
   "start": "ts-node index.ts"
},
...

```

5. Run your project

```sh
npm start
```

The server will spin up a Node Typescript project in `http://localhost:3000`.

TODO: persistence layer

## 2. Dotnet / C#

- **Typing**: Strong, static
- **Founder**: Microsoft
- **Paradigm**: multi paradigm, mainly object-oriented

This framework which is created by Microsoft is often given a negative impression at first glance because of the name **Microsoft** associated with .NET framework, but I assure you, nothing is about money-grabbing software made by greedy big corporations here.

## Honorable mention

- Python
- **Typing**: Strong, dynamic (optional static)
- **Founder**: Guido Van Rossum
- **Paradigm**: multi paradigm, mainly imperative, object-oriented

## 1. PHP

- **Typing**: Weak, dynamic (optional static)
- **Founder**: Rasmus Lerdorf
- **Paradigm**: multi paradigm, mainly imperative

PHP is still the king of web programming, as it is the true pioneer and kickstarter of dynamic web applications. In fact, around 50% or more of the world wide web is still using PHP.
