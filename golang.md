- Go CLI (command line interface)
  - go build: only compile
  - go run: compile and run
  - go fmt: format all the code in each file in the current directory
  - go install: compile and install a package
  - go get: download the raw source code of someone else's package
  - go test: run any test associated with the current project
- Package:
  - package == project == workspace
  - first line of each file must define the package
  - executable package: generates a file that we can run
  - reusable package: code used as "helpers", used as libraries or code dependencies
  - package main -> go build -> main.exe (main is an indicator as an executable file/package). Other names are resusable package
- variable
  ```
  var card string = "hello"
  var: to create a variable
  card: the name of the variable
  string: only string type will ever be assigned to this variable
  
  card := "hello" -> same as the definition above, but rely on the compiler to infer type, used when first defined
  if later we want to change the value of card, just need to use "="
  card = "hello again"
  ```
  - Go is static typed language (java, C++ as well, assign a type when defining a variable)
    - basic go types: bool, string, int, float64
  - dynamic typed language: javascript, ruby, python
- function
  ```
  // string indicates that a string needs to be returned
  func newCard() string {
    return "hello"
  }
  ```
  - function receiver: 
  ```
  // any variable of type "deck" now gets access to the "print" method, it is sort of like passing a function pointer to a function but much easier than that
  func (d deck) print() {
    for i, card := range d {
      fmt.Println(i, card)
    }
  }
  ```
- Array and Slice in Go
  - Array: fixed length list of things
  - Slice: an array that can grow or shrink
    - every element in a slice must be of same type
    - create a slice ```cards := []string{"1","2"}```
    - add to slice: ```cards = append(cards, "3") // append does not modify the original slice ```
    - iterate slice:  ```for i, card in range cards {fmt.Println(i, card)} // i is the index, card is the content``` 
    - slice are 0 indexed, access can be done by cards[0], cards[1], cards[0:2], cards[:2], cards[2:] etc
- Testing in go
  - to make a test, create a new file ending in _test.go
  - to run all tests in a package, run ```go test```
  
    
