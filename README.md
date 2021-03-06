# USER FINDER SERVICE #

This service exposes a capability to search users that match a set of filters.

## What is this repository for? ##

* Implementation for business case [Filtering Matches](https://github.com/affinitas/coding_exercises_options/blob/master/filtering_matches/README.md)
* Contains source code that implements a user finder service and a user matches UI.
* /src Java code implementation is just a back end using java and spring-boot technology.
* /ui Angular code implementation is the User Interface to search users using a set of filters.

## How to execute this project ##

* Docker (17.09) and Docker-compose (1.16.0) is required.

* Clone the project:

```sh
git clone https://github.com/fernandoocampo/user-finder.git
cd ./user-finder
```

* Execute docker-compose and wait the project is built.

```sh
docker-compose up
```

* Open your Internet browser and go to: http://localhost

## Using service ##

### Parameters ###

The parameters of this service are.

| Parameter                 | Description                                                                  |
| ------------------------- | ---------------------------------------------------------------------------- |
| `hasphoto`                | true to indicate if the user must have a photo in its profile.		   |
| `incontact`               | true to indicate if the user is a contact of the user that is searching.     |
| `isfavourite`             | true to indicate if the user is a favourite one.				   |
| `mincompatibilityscore`   | it is an integer to indicate the minimum compatibility score expected. (1-99)|
| `maxcompatibilityscore`   | it is an integer to indicate the maximum compatibility score expected. (1-99)|
| `minage`                  | it is an integer to indicate minumum age expected. (18-95).		   |
| `maxage`                  | it is an integer to indicate maximum age expected. (18-95).		   |
| `minheight`               | it is an integer to indicate minimum height in cm expected (135-210).	   |
| `maxheight`               | it is an integer to indicate maximum height in cm expected (135-210).	   |
| `distanceinkm`            | it is an integer to indicate the distance in km expect to search a prospect. |
| `inquirerlongitude`       | it is the current longitude position of the inquirer user.		   |
| `inquirerlatitude`        | it is the current latitude position of the inquirer user.			   |
| `distancelowerbound`      | single selection: lower bound < distanceinkm, upper bound > distanceinkm     |


To check service health, please go to the following api.

```sh
curl -g 'http://localhost:8080/health'
```

### Usage examples ###

* Search users that has a photo in its profile.

```sh
curl -g 'http://localhost:8080/userfinder?hasphoto=true'
```

* Search users that has a photo in its profile.

```sh
curl -g 'http://localhost:8080/userfinder?hasphoto=true'
```

* Search users that are in contact with the inquirer user.

```sh
curl -g 'http://localhost:8080/userfinder?incontact=true'
```

* Search users that are in contact with the inquirer user and has a photo in its profile.

```sh
curl -g 'http://localhost:8080/userfinder?hasphoto=true&incontact=true'
```

* Search users that have a minimum and maximum compatibility score.

```sh
curl -g 'http://localhost:8080/userfinder?mincompatibilityscore=23&maxcompatibilityscore=76'
```

* Search users that are near to the inquirer user at a lower bound given 30 km of distance.

```sh
curl -g 'http://localhost:8080/userfinder?distanceinkm=30&inquirerlongitude=0.187&inquirerlatitude=2.345&distancelowerbound=true'
```

* Search users that are near to the inquirer user at a upper bound given 300 km of distance.

```sh
curl -g 'http://localhost:8080/userfinder?distanceinkm=300&inquirerlongitude=0.187&inquirerlatitude=2.345&distancelowerbound=false'
```

* To check if the service is up, we can use the /health endpoint.

```sh
curl -g 'http://localhost:8080/health'
```

### How do I get set up? ###

1. Download the source code from github.com at [here](https://github.com/fernandoocampo/user-finder)

2. For development process you will need:

**Backend**

* Java jdk1.8.0_111

* Maven [installer](https://maven.apache.org/download.cgi)

* Any IDE that can read maven projects. I used Netbeans 8.2.

**Frontend**

* Find it on ./ui/user-matches-app folder.

* Visual Studio Code 1.18.1.

* npm 3.10.10

* @angular/cli 1.5.4

* @angular/compiler-cli ^5.0.0

* if you wish to execute the frontend in development mode, you could do the following:

```sh
ng serve --open
```

* If you want to test the frontend, please execute the following.

```sh
ng test
```

3. To run the service and let it ready for consume, docker is required.

* In the project root folder executes the following.

```sh
docker-compose up
```

when finish please destroy the containers

```sh
docker-compose down
```


### Assumptions ###

* I am stronger on the backend side.

* All filters to search are not required.

* If client doesn't search any filter the system should return an empty array. 

* If client doesn't send minimum or maximum in the range filters, the system should have the following behavior.
    * if minimum value is set and maximum not, the service searches from minimum value until maximum configured default value.
    * if maximum value is set and minimum not, the service searches from the minimum configured default value until the given maximum value.

* The service is in charge to validate the input data. The search parameters will be validated in the service layer.

* The service with one node and just a node in the database are enough to support the current number of transactions.

* The database used in this service hypotetically is a robust.

* I assume that incontact filter means all those users that are contacts of the inquirer user. 
    * Then for this filter I will match all objects which contacts_exchanged is greater than 1.

* To be able to use mongodb geospatial queries, I have modified the provided matches.json adding an array for localization on city attribute and it has the form loc: [lon,lat].

* I assume that for distance filter if user doesn't give all the data required for localization, the service ignores this filter.

* I assume that the service is in a DMZ (DeMilitarized Zone) and it allows connection from any server.

* I assume that the client captures the user geo position to calculate the distance where the service must start to search.

* I assume that the service should not validate if the latitude and longitude are valid values.