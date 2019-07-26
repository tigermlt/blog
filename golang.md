- Go CLI (command line interface)
  - go build: only compile
  - go run: compile and run
  - go fmt: format all the code in each file in the current directory
  - go install: compile and install a package
  - go get: download th eraw source code of someone else's package
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
  