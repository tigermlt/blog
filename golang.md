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
  - interfaces are not generic types
  - interfaces are 'implicit'. We don't write any manual code to link code with interface (like implements in java)
  - interfaces are a contract to help manage types
  - An example in http
    - The body in response of get is an interface that groups Reader and Closer
    - Reader and Closer are also interface (interface can embed into antoher)
    - Interface is like an adapter that accepts different sources of inputs (such as http request body, text file on hard drive etc) and output data that anyone can work with
    ```
    func main() {
	resp, err := http.Get("http://www.google.com")
	if err != nil {
		fmt.Println("Error:", err)
		os.Exit(1)
	}
	// Read function will take a buffer and fill it of response. If the buffer
	// is full, the Read function will not write data to it. We use Reader interface in this example
	bs := make([]byte, 99999)
	resp.Body.Read(bs)
	fmt.Println(string(bs))
    }
    ```
    We can also make use of the built-in library to avoid hacky buffer
    ```
    func main() {
	resp, err := http.Get("http://www.google.com")
	if err != nil {
		fmt.Println("Error:", err)
		os.Exit(1)
	}
	// this make uses of Writer interface
	// io.Copy(dst Writer, src Reader) (wirtten int64, err error), it copies data from src to dst
	// resp.Body implements the Reader interface and os.Stdout implements the Writer interface
	io.Copy(os.Stdout, resp.Body)
    }
    ```
    We can also write our own interface. The logWriter has the Write function so it implements Write interface implicitly
    ```
    type logWriter struct{}

	func main() {
		resp, err := http.Get("http://www.google.com")
		if err != nil {
			fmt.Println("Error:", err)
			os.Exit(1)
		}

		lw := logWriter{}
		io.Copy(lw, resp.Body)
	}

	func (logWriter) Write(bs []byte) (int, error) {
		fmt.Println(string(bs))
		return len(bs), nil
	}
    ```
- Concurrency
  - Every running program (a process) has at least a Go Routine (engine that executes code)
  - add ```go``` to a function which would launch a new Go routine to execute the function
  - Main Routine, Go Routine will interact with each other, if a Go Routine blocks, it will go back to Main Routine and Main Routine will let another Go Routine run
  - each go routine is a thread
  - one CPU core -> Go Scheduler -> multiple Go Routine. Scheduler runs one routile until it finishes or making a blocking call (such as HTTP Get)
  - multiple CPU core -> Go Scheduler -> multiple Go Routine. In this case, scheduler runs one thread on each "logical" core. It tries to assign each Go routine to each CPU core so we have true parallelism.
  - Concurrency - we can have multiple threads executing code. If one thread blocks, another one is picked up and worked on
  - Parallelism - Multiple threads executed at the exact same time. Requires multiple CPUs
  - **channel** is used to communicate between go routines. With channel, the main routine can know if child routines have completed their code. Main routine -> Channel -> multiple child go routines
  ```
  // send message into channel
  channel <- 5
  // wait for a value to be sent into this channel
  myNumber <- channel
  // normally we let function (child go routine) send value and let main function (main routine) wait for the value
  ```
  - function literal in go is like lambda or anonymous function in java
  ```
  go func() {
    ....
  }() // () is used to execute the function
  ```
  - never ever try to access the same variable from different child routines. Use pass by value to avoid it
