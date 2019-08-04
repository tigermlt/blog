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
    - slice has a slice data structure (pointer to head, capacity, length)) and an array that holds the data
- Testing in go
  - to make a test, create a new file ending in _test.go
  - to run all tests in a package, run ```go test```
- Struct
  ```
  type person struct {
    firstName string
    lastName string
  }
  
  Amy := person{firstName: "Amy", lastName: "Anderson"}
  fmt.Println(alex)
  ```
- Pointer
  - go is pass-by-value language
  ```
  Use pointer in struct
  jimP := &jim
	jimP.updateName("Amy")
  // or we can do a simplification in go to write as:
  jim.updateName("Amy")  // go will infer the pointer based on the type of receiver
  
  func (p *person) updateName(newFirstName string) {
	  (*p).firstName = newFirstName
  }
  ```
  - Notice that if we pass slice to function, since slice holds a pointer to data (array), the copied pointer points to the same array. So if we update the data inside the function, the modification persists
    - value types: int, float, string, bool, structs
    - reference types: slices, maps, channels, pointers, functions (can be passed around as argument to another function)
- Map
  - a collection of key value pairs
  - all keys have to be the same type, all values have to be the same type
  ```
  // declare a map
  colors := map[string]string{
	"red":   "r",
	"green": "g",
  }
  // other ways to declare map
  var colors map[string]string
  // another way
  colors := make(map[string]string)
  // add value to map
  colors["yellow"] = "y"
  // delete key/value in map
  delete(colors, "yellow")
  // iterate map
  for key, value := range mapname {
      ......
  }
  ```
- Interface
  - One usage of interface is to help us reuse code (the same code can work for multiple types)
  ```
  package main

	import "fmt"

	type englishBot struct{}
	type spanishBot struct{}

	type bot interface {
		// if you are a type in this program with a function called 'getGreeting' and return a string
		// then you are now an member of type 'bot'
		// you can also call this 'getGreeting' function
		getGreeting() string
	}

	func main() {
		eb := englishBot{}
		sb := spanishBot{}

		printGreeting(eb)
		printGreeting(sb)
	}

	func (eb englishBot) getGreeting() string {
		return "Hi!"
	}

	func (sb spanishBot) getGreeting() string {
		return "H"
	}

	func printGreeting(b bot) {
		fmt.Println(b.getGreeting())
	}
  ```
