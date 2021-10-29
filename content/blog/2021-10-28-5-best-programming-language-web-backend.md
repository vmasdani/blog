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

## 5. Golang

- **Typing**: Strong, static
- **Founder**: Google
- **Paradigm**: multi paradigm, mainly functional

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

Creating a HTTP server with router is very easy in Golang. Using the `gorilla/mux` package, we can write a REST server backbone as simple as this:

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
employees.find((e) => e.id === selectedId)?.name
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

3. **Lack of generics**


## 4. Spring Boot + Kotlin

- **Typing**: Strong, static
- **Founder**: Jetbrains (Kotlin), VMWare (Spring)
- **Paradigm**: multi paradigm, mainly object oriented

If you are looking for a "powerful" web backend solution which has been around for a long time and you want to build an enterprise web application for very cheap, enter Spring and Kotlin.

## 3. Typescript Node

- **Typing**: Strong, static
- **Founder**: Ryan Dahl (NodeJS), Typescript (Anders Hejlsberg/Microsoft)
- **Paradigm**: multi paradigm, mainly functional

NodeJS is probably one of the most common tools used in the backend web programming community, and that is not wihout reason.

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
