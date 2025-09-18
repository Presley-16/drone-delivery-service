Drone Dispatch Service





A comprehensive REST API service for managing drone fleet and medication delivery built with Spring Boot 3.2.0 and Java 23.



1\. Features



&nbsp;All Required Features Implemented:



* Register a drone
* Load drone with medication items  
* Check loaded medication items for a given drone
* Check available drones for loading
* Check drone battery level for a given drone
* Prevent drone from being loaded with more weight than it can carry
* Prevent drone from being in LOADING state if battery level is below 25%
* Periodic task to check drones battery levels and create audit event log
* Input/output data in JSON format
* H2 in-memory database with preloaded dummy data
* Comprehensive validation and error handling
* OpenAPI documentation (Swagger UI)



2\. Technology Stack



* Java 21
* Spring Boot 3.2.0
* Spring Data JPA
* H2 Database (in-memory)
* Maven\*3.11.0\* for build management
* OpenAPI 3 for API documentation
* JUnit 5 for testing (optional)



3\. Project Structure



drone-deliver-service

src/

├── main/

│   ├── java/com/droneservice/

│   │   ├── DroneDispatchServiceApplication.java      # Main application

│   │   ├── controller/

│   │   │   └ DroneController.java                # REST endpoints

│   │   ├── service/

│   │   │   ├─ DroneService.java               # Business logic

│   │   │   └─ BatteryAuditService.java        # Battery monitoring

│   │   ├── model/

│   │   │   ├─ Drone.java                      # Drone entity

│   │   │   ├─ Medication.java                 # Medication entity

│   │   │   ├─ BatteryAudit.java              # Battery audit entity

│   │   │   ├─DroneModel.java                 # Drone model enum

│   │   │   └──DroneState.java                 # Drone state enum

│   │   ├── dto/

│   │   │   ├── DroneRegistrationRequest.java   # Registration request

│   │   │   ├── LoadDroneRequest.java          # Load request

│   │   │   ├── MedicationRequest.java         # Medication request

│   │   │   ├── DroneResponse.java             # Drone response

│   │   │   └── MedicationResponse.java        # Medication response

│   │   ├── repository/

│   │   │   ├── DroneRepository.java           # Drone data access

│   │   │   ├── MedicationRepository.java      # Medication data access

│   │   │   └── BatteryAuditRepository.java    # Battery audit data access

│   │   ├── exception/

│   │   │   ├── DroneException.java            # Custom exception

│   │   │   └── GlobalExceptionHandler.java     # Global error handling

│   │   └── config/

│   │       └── DataInitializer.java           # Dummy data loader

│   └── resources/

│       ├── application.properties              # Configuration

│       └── data.sql                           # SQL initialization (optional)

└── test/

&nbsp;   └── java/com/droneservice/                 # Test classes





4\. Prerequisites



* Java 17 or higher
* Maven 3.8+
* IntelliJ IDEA (recommended) or any IDE
* Git



5\. Setup Instructions



1.Clone or Create Project





* &nbsp;If creating new project in IntelliJ:
* &nbsp;File -> New -> Project -> Maven -> Create from archetype (skip archetype)
* &nbsp;Use groupId: Drone-delivery-service
* &nbsp;Use artifactId: Drone-delivery-service





2\. Update pom.xml



Replace your existing pom.xml with the provided version that includes Spring Boot dependencies.



3\. Create Package Structure



Create the following package structure in `src/main/java`:



com/droneservice/

├── controller/

├── service/

├── model/

├── dto/

├── repository/

├── exception/

└── config/





4\. Add Source Files



Copy all the provided Java source files into their respective packages.



5\. Add Configuration



Create `src/main/resources/application.properties` with the provided configuration.





*Build Instructions*



Using Maven Command Line:





Clean and compile

mvn clean compile



Run tests (if you are to implement)

mvn test



Package the application

mvn clean package



Run the application

mvn spring-boot:run





***NB!!!!!!!***

***I faced an error where Maven is unable to download the artifact org.apache.maven.surefire:common-java5:jar:3.0.0 from the Maven Central Repository (https://repo.maven.apache.org/maven2). This issue is likely caused by a network connectivity problem or a proxy/firewall blocking the connection.Ensure that your machine has internet access and can reach the Maven Central Repository.***

***Also if the artifact was partially downloaded or corrupted, clear the local cache for the surefire plugin: ~/.m2/repository/org/apache/maven/surefire/***







***Using IntelliJ IDEA:***



1\. \*\*Import Project:\*\*

&nbsp;  - File -> Open -> Select your project folder

&nbsp;  - IntelliJ will automatically detect it as a Maven project



2\. \*\*Sync Maven Dependencies:\*\*

&nbsp;  - Click the Maven icon in the right panel

&nbsp;  - Click "Reload All Maven Projects"



3\. \*\*Run Application:\*\*

&nbsp;  - Right-click on `DroneDispatchServiceApplication.java`

&nbsp;  - Select "Run DroneDispatchServiceApplication" or use the green play button

&nbsp;  





*Running the Application*



Method 1: Using Maven

`bash

mvn spring-boot:run





Method 2: Using Java directly

`bash

First build the jar

mvn clean package





Then run

java -jar target/Drone-delivery-service-1.0-SNAPSHOT.jar





Method 3: Using IntelliJ

\- Right-click main application class and select "Run"





*Testing the Application*



Once the application starts, you can access:



1\. API Documentation (Swagger UI)

\- \*\*URL:\*\* http://localhost:8080/swagger-ui.html

\- Interactive API documentation with try-it-out functionality





2\. H2 Database Console

\- \*\*URL:\*\* http://localhost:8080/h2-console

\- \*\*JDBC URL:\*\* `jdbc:h2:mem:dronedb`

\- \*\*Username:\*\* `sa`

\- \*\*Password:\*\* (leave empty)



\### 3. Sample API Endpoints



\#### Register a Drone

```bash

POST http://localhost:8080/api/drones/register

Content-Type: application/json



{

&nbsp; "serialNumber": "DRONE\_TEST\_001",

&nbsp; "model": "HEAVYWEIGHT",

&nbsp; "weightLimit": 500,

&nbsp; "batteryCapacity": 80

}

```



\#### Load Drone with Medications

```bash

POST http://localhost:8080/api/drones/DRONE\_TEST\_001/load

Content-Type: application/json



{

&nbsp; "medications": \[

&nbsp;   {

&nbsp;     "name": "Aspirin-100mg",

&nbsp;     "weight": 50,

&nbsp;     "code": "ASP\_100",

&nbsp;     "imageBase64": null

&nbsp;   },

&nbsp;   {

&nbsp;     "name": "Paracetamol-500mg",

&nbsp;     "weight": 75,

&nbsp;     "code": "PARA\_500",

&nbsp;     "imageBase64": null

&nbsp;   }

&nbsp; ]

}





Get Available Drones

`

GET http://localhost:8080/api/drones/available

```



Check Drone Battery

GET http://localhost:8080/api/drones/DRONE\_TEST\_001/battery

`



Get Drone Medications

```bash

GET http://localhost:8080/api/drones/DRONE\_TEST\_001/medications

```



\#Get Battery Audit History



GET http://localhost:8080/api/drones/DRONE\_TEST\_001/battery-history

```



Key Features Explained



1\. Drone Models

\- `LIGHTWEIGHT` - For light medications

\- `MIDDLEWEIGHT` - For medium medications  

\- `CRUISERWEIGHT` - For heavier medications

\- `HEAVYWEIGHT` - For heaviest medications



2\. Drone States

\- `IDLE` - Ready for loading

\- `LOADING` - Currently being loaded

\- `LOADED` - Loaded and ready for delivery

\- `DELIVERING` - Out for delivery

\- `DELIVERED` - Delivery completed

\- `RETURNING` - Returning to base



3\. Validation Rules

\- Serial number: Max 100 characters

\- Weight limit: Max 500gr

\- Battery capacity: 0-100%

\- Medication name: Only letters, numbers, hyphens,



