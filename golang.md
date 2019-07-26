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
  ```
  
