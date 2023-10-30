
<a target="_blank" href="https://www.linkedin.com/in/vitorhora/"><img height="20" src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" /></a>

# Pokémon Trainer

The customer needs the creation of a Pokemon Trainer Index. In this tale, each trainer's story unfolds with their Name, Email, Instagram profile link, and a treasure trove of their cherished Pokemon companions. These Pokemon bear unique attributes - an ID, a Name that resonates, Abilities that make them extraordinary, and a wealth of Experience.

To share this enchanting world with others, they envision a list pathway. Here, trainers' IDs and Names gleam like beacons, and their Instagram profiles sparkle, ready to be explored by all who embark on this journey. But as with all great stories, there are guarded secrets. The operations to maintain these chapters are locked away, accessible only to those who possess the right authentication keys.

As this grand narrative unfolds, each authenticated user leaves their mark, their username etched into the logs as they venture deeper into the trainer's realm. And to weave an even richer tapestry, they seek to intertwine their tale with that of the PokéAPI, a wellspring of Pokemon wisdom found at https://pokeapi.co/.

Now, it's time for architects to step onto this stage, armed with their creativity and expertise, to craft a solution worthy of this epic adventure, coding in Java, delivering what is expected for your role, and please justify all your assumptions and decisions. 

# Visions

## Use Cases

**1. Explore List of Trainers**
- Main Actor: Any User
- Main Flow:
1.1 User accesses the list of trainers with their respective Pokémon.
1.2 The system displays a list of trainers with their respective Pokémon.

**2. View Trainer Details**
- Main Actor: Any User
- Main Flow:
2.1 User inform a trainer.
2.2 The system displays details about the trainer with their respective Pokémon.

**3. Authenticate**
- Main Actor: Previously registered user
- Main Flow:
3.1 User provides authentication credentials (user/password).
3.2 The system validates the credentials.
3.3 The system returns an OAuth token.
3.4 The user gains access to additional operations.

**4. Create Pokemon Trainer**
- Main Actor: Authenticated User Admin
- Main Flow:
4.1 Trainer provides their name, email, Instagram profile link, and if they wish, details about their Pokémon, such as name, abilities, and experience.
4.2 The system generates a unique ID for the trainer.
4.3 Details are recorded in the system, including information about the Pokemon (ID, Name, Abilities, and Experience), if provided.

**5. Maintain Trainer**
- Main Actor: Authenticated User Admin
- Main Flow:
5.1 User inform a trainer.
5.2 The authenticated user can make changes to their trainer profile, such as updating their Name, Email, Instagram profile link, adding Pokémon, associating, or disassociating with free Pokémon previously loaded through the PokéAPI.

**6. Explore list of Pokémon**
- Main Actor: Authenticated User Admin
- Main Flow:
6.1 User accesses the list of Pokémon and their details.
6.2 The system displays a list of Pokémon and their details.

**7. Integration PokéAPI**
- Main Actor: Batch job
- Main Flow:
7.1 The system accesses the PokéAPI APIs.
7.2 A schedule ingests new Pokémon or updates them if they do not exist.
7.3 The ingested data is saved in batchable sets in the database.

**8. User Activity Logging**
- Main Actor: Listener job
- Main Flow:
8.1 The listener monitors the calls to the Pokémon Trainer API.
8.2 The listener saves the information to a file.

&nbsp;
<center>
  <img src="https://raw.githubusercontent.com/vitorhora/pokemontrainer/main/UseCase.drawio.png" alt="Use Cases">
</center>



&nbsp;
## Components

Component View and their Relationships.


#### PokemonOaut

- Oauth server.

- Component responsible for receiving token requests to access secure API resources.

- These requests contain a username and password for verification.

- Upon initializing the service, it stores examples of usernames and passwords in memory.

- If the provided username and password match any of those stored, the component generates a security token using a private key (private.PEM).

- This token can be used to access secure API resources, with validation performed using the public key (public.PEM).

- Creating Swagger for API documentation.

&nbsp;
#### PokemonTrainer

- Providing maintenance APIs for Trainers.

- Creating Swagger for API documentation.

- Storing in-memory usernames and passwords of administrators to validate when receiving a security token.

- OAuth client to validate security tokens for secure resources.

- Monitoring requests and logging to a file.

- Automatically generate the database tables.

&nbsp;
#### PokemonFree

- Batch service responsible for integrating with the PokeAPI.

- Upon its initial startup, it fetches information about Pokémon and inserts them in batches into the database.

- In scheduled and configurable recurring executions, it only updates the database when there is a Pokémon that is not stored. This could be because it was removed from the database or added as new in the PokeAPI."

&nbsp;
#### PokeAPI

- External service that provides information about Pokémon through APIs.

&nbsp;
#### MySQL

- Database where information about trainers and Pokémon is stored.

&nbsp;
<p align="center">
  <img src="https://raw.githubusercontent.com/vitorhora/pokemontrainer/main/Component.drawio.png" alt="Component">
</p>

&nbsp;
## Classes

- Relationship between entities.

&nbsp;
<p align="center">
  <img src="https://raw.githubusercontent.com/vitorhora/pokemontrainer/main/ClassDiagram.png " alt="Class Diagram">

## Deployment

- Infrastructure Deployment View

&nbsp;
<p align="center">
  <img src="https://raw.githubusercontent.com/vitorhora/pokemontrainer/main/Deployment.drawio.png " alt="Deployment">


&nbsp;
# Installation
## Prerequisites

You need to have the following prerequisites installed on your system:

- [Java Development Kit (JDK 17)](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html)
- [Maven](https://maven.apache.org/download.cgi) installed and configured on your system.
- [MySQL 8](https://dev.mysql.com/downloads/mysql/) installed and configured on your system.

### Configuring the database

1. Open the MySQL administration tool and create a user with sufficient permissions to manage.
Example:
*username: user
password: 12345*
2. Create the database
```sql
CREATE DATABASE  IF NOT EXISTS `pokemontrainer`;
USE `pokemontrainer`;
```
3. Inform the credentials in the *application.properties* file of the ***pokemontrainer*** and ***pokemonfree*** project
```markdown
spring.datasource.username=user
spring.datasource.password=12345
```
4. Now you do not need to worry about the database anymore. The *pokemontrainer* project will generate all the tables when started.

### Starting the applications

#### 1. pokemontrainer

1.1 After unpacking the ZIP, navigate to the folder and run the following Maven command:
```shell
mvn clean install
```
1.2 Now let's start the application:
```shell
java -jar /pokemontrainer/target/pokemontrainer-0.0.1-SNAPSHOT.jar
```
1.3 The application is configured to respond on *port 8080*. To test if it started successfully, let's open the Swagger API documentation:
[http://localhost:8080/swagger-ui/index.html](http://localhost:8080/swagger-ui/index.html "http://localhost:8080/swagger-ui/index.html")
&nbsp;
![](https://raw.githubusercontent.com/vitorhora/pokemontrainer/main/swagger-pokemontrainer.png)
&nbsp;
*****************
**Be patient.** At this moment, we have only deployed the Pokémon management application. In the next steps, we will start the other two modules on different ports.
*****************

#### 2. pokemonoauth

2.1 After unpacking the ZIP, navigate to the folder and run the following Maven command:
```shell
mvn clean install
```
2.2 Now let's start the application:
```shell
java -jar /pokemonoauth/target/pokemonoauth-0.0.1-SNAPSHOT.jar
```
2.3 The application is configured to respond on *port 8081*. To test if it started successfully, let's open the Swagger API documentation:
[http://localhost:8081/swagger-ui/index.html](http://localhost:8081/swagger-ui/index.html "http://localhost:8081/swagger-ui/index.html")
&nbsp;
![](https://raw.githubusercontent.com/vitorhora/pokemontrainer/main/swagger-pokemonoauth.png)
&nbsp;

*****************
Now, you can already use the system, but the database **does not have any Pokémon**. 
There is an option to register trainers along with their Pokémon, but we **will automatically register** hundreds of Pokémon through the PokeAPI when we start the next step. 
It's the batch service that will ingest Pokémon data. 
The good news is that after the service finishes loading, there **will be hundreds of ownerless Pokémon to be adopted (associated) by trainers.**
*****************

#### 3. pokemonfree

3.1 After unpacking the ZIP, navigate to the folder and run the following Maven command:
```shell
mvn clean install
```
3.2 Now let's start the application:
```shell
java -jar /pokemonfree/target/pokemonfree-0.0.1-SNAPSHOT.jar
```
3.3 The application is configured to respond on *port 8083*.
&nbsp;
![](https://raw.githubusercontent.com/vitorhora/pokemontrainer/main/swagger-pokemonoauth.png)
&nbsp;
The service will take a **few minutes to load all the Pokémon (1293)**, but you can already use the APIs normally.
Pokémon are loaded into the database in batches of 100. 
To check the progress of the loading, perform a query in the database:
```sql
SELECT COUNT(*) FROM pokemontrainer.pokemon;
```

## Logging

The request monitoring logs are saved in a relative path in the **same root** where the projects were saved. 
They are available in a folder called *'log,'* and the file is *'request.log.'*

## Security

Public and private key approach was used, using .PEM files. 
&nbsp;
The OAuth server project (pokeoauth) generates the tokens and encrypts them with the private key. 
&nbsp;
The OAuth client project requires the token to expose the APIs (except the list of Trainers) and uses the public key to verify if it's a valid token. 
&nbsp;
In this approach, I used in-memory saved users both for token generation and validation.
&nbsp;
The certificates are located in the following folders:

`/pokemontrainer/src/main/resources/certs/public.pem`

`/pokemonoauth/src/main/resources/certs/private.pem`


