# Backoffice Demo

Backoffice Demo represents mini-system incorporation ETL module, web-service REST api module and frontend module
## Global Preconditions
The following resources need be installed:
1) java 1.8+
2) maven 4

## ETL Module
etl/
Represent Backoffice initialization tool which extracts, transforms and loads db structure and data.

### Usage
From command line run packaging proccess
```bash
<path_to_etl> mvnw package
```
Then run etl tool :
```bash
java -jar <path_to_etl>/targer/BackofficeInitializer-0.0.1-SNAPSHOT.jar -Dspring.datasource.url=<db_connection_string> -Dspring.datasource.username=<username>  -Dspring.datasource.password=<password> -Dbrands=<brands_tsv_file_name> -Dquantities=<brands_tsv_file_name>
```

Example:
```bash
BackofficeInitializer-0.0.1-SNAPSHOT.jar -Dspring.datasource.url=jdbc:mysql://localhost:3306/backoffice10 -Dspring.datasource.username=backoffice  -Dspring.datasource.password=backoffice -Dbrands=brands.tsv -Dquantities=quantities.tsv
```

Application creates db schema and populates it with data.

### Known issues and further improvements
1) Better logging of db populating proccess
2) Test coverage improvent: data validation, resulting file assertion (using hamcreast), full integration test
3) Escapings processing

## Backend Module
backoffice/

Web-service exposing REST api for CRUD operations for Brand and Inventory Sum.

### Usage
Prerequisite: tomcat 9.0

From command line run packaging proccess
```bash
<path_to_backoffice> mvnw package
```
Then run web service :
```bash
java -jar <path_to_backoffice>/targer/backoffice-0.0.1-SNAPSHOT.jar
```
Endpoints can be used either from Postman or Swagger-ui.
For Postman:
1) http://localhost:8080/zappos/brands - CRUD for Brand
2) http://localhost:8080/inventory/search/sum - total inventory sum

Incorporated swagger-ui cannot reflect model properly however it can be still used to test the endpoins:
http://localhost:8080/swagger-ui.html#/  - CRUD for Brands

#### CRUD Brand details
1) GET zappos/brands
Returns brand with total quantity over all added quantities.
Example:
```json
[{
  id: 12345,
  name: "name",
  totalQuantity: 45
}]
```
2) POST zappos/brands
It is possible to add simple brand with name only
```json
{
  name: "name for new brand"
}
```
as well as with initial quantity(ies)
```json
{
  name: "new brand",
  [
     {
       "quantity": 10,
       "receivedTime": 155445817
     }
  ]
}
```

To work with xml set header Accept="application/xml", to work with json set header Accept="application/json"

### Known issues and further improvements
1) Swagger-ui does not reflect Brand Model correctly (known issue of springfox, PR is still open) and does not reflect custum endpoint for http://localhost:8080/inventory/search/sum
2) Test coverage improvent: integration test for RestControllers

## Frontend Module
frontoffice/

### Usage
Prerequisite: npm 6.9.0+

From command line in "frontoffice" dir
1) Intall
```bash
npm install
```
2) In src/config.js specify URL to valid REST api
3) And then start as
```
npm start
```
In browser: http//localhost:<port_service _started>
# zps
