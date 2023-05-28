COLA 

In Java

The scope OF each layer are as follows:
1. Presentation Layer: This layer receives web requests in Rest format, routes the requests to the application layer for execution, and returns the View Model, which is generally carried by a Data Transfer Object (DTO).
2. Application Layer: This layer obtains the input, assembles the context, verifies the input, calls the domain layer for business processing, and sends a message notification if necessary. The layers are open. If necessary, the application layer can directly access the infrastructure layer.
3. Domain Layer: This layer encapsulates the core business logic and provides external computing and processing for the business logic through Domain Service and Entities functions.
4. Infrastructure Layer: This layer includes Tunnel, Config, and Common. Here, we use the Tunnel to abstract all data sources, including databases (MySQL and NoSQL databases), search engines, file systems, and SOA services. Config is used to configure applications, whereas Common is a common tool class.


In GO

1. Presentation layer: controller layer to receive REST requests (with gin?)
2. Application layer: Obtains input, verifies input, assemble context, calls domain layer
3. Domain layer: encapsulates business logic, computing of data via entities and services (using infra layer)
4. Infrastructure layer: configuration, datasources, file systems, external services.






Notes/Questions 
Dependency injection in GO?
COLA use Pattern command link between controller application (COLA has moved to dependency injection but using Pattern is still an option)


COLA-GOLA

The project cola-gola will generate a template of code in Go with the layers and patterns similar to the COLA framework which is written in Java.
The generated code will contain a TeaOrdering application which will expose 2 Rest Api methods:
to list the different Teas 
to place an order.
For simplicity the data will come from and will be recorded in text files.
Source tree
The main source tree will have the following infrastructure:
 `Main.go`
      `/controller/TeaController.go` Tea order (rest request to list all available Tea)  (Gin very nice)
      `/application/TeaApp.go` (Java Bean/record, Structs in Go)
      `/domain/` entities (Tea entity) and aggregating use of infrastructure layer
      `/infrastructure/TeaService` call to get data from different data sources (database, file, external services) read from a file
      
Files to generate
We need to be able to write manually what we want to generate.
First step. Manually write the customer order in Go BUT respecting the COLA principle of layers. We will have all 4 layers as an example.
Write the template generator, divided in 4 parts
1. Generating the controller layer code (in controller directory)
1.1. TeaInventoryController in Go (using Gin) (methods: list possible teas, detail of a Tea by id) using DefaultTeaService (I’m not sure how to do this in Go e.g dependency injection)
1.2. TeaOrderController in Go (using Gin) (methods:  order a Tea and list orders from email) using DefaultTeaService (I’m not sure how to do this in Go e.g dependency injection)
1.3. TeaService interface in Go (methods: list teas, order and list orders from email)
1.4. Structs TeaDTO and TeaOrderDTO (same as Structs from the domain layer)

2. Generating the application layer code
2.1 DefaultTeaService implementing TeaService (receiving OrderTeaCommand see Command Pattern in GO https://dev.to/tomassirio/command-design-pattern-in-go-3lpl )
2.2.OrderTeaCommand struct to contain number of entities to buy

3.Generating the domain layer code
3.1. Struct (bean) Tea.go (with field such id, name, price)
3.2. Struct (bean) TeaOrder.go (with field such id, email, teaId)

4. Generating the infrastructure layer code
4.1. TeaFileReader (read teas from a file, return collection of Tea struct)
4.2. TeaOrderWriter (write order to a file, read orders)

















