# Loopback sample app

## Run + init (not using volumes)

Terminal 1:
```
docker run -e MYSQL_ALLOW_EMPTY_PASSWORD=true -e MYSQL_USER= -p 3306:3306 -e MYSQL_DATABASE=sample mysql
```

Terminal 2:
```
CID=$(docker ps | grep mysql | cut -d " " -f1); docker exec ${CID} mysql -h127.0.0.1 -u root -D sample -e "CREATE TABLE Customer (id int NOT NULL AUTO_INCREMENT, Name varchar (255) NOT NULL, Country varchar (255) NOT NULL, PRIMARY KEY (id)) ENGINE=InnoDB DEFAULT CHARSET=latin1"
CID=$(docker ps | grep mysql | cut -d " " -f1); docker exec ${CID} mysql -h127.0.0.1 -u root -D sample -e "CREATE TABLE Appointment (id int NOT NULL AUTO_INCREMENT, CustomerID int NOT NULL, Date varchar (255) NOT NULL, PRIMARY KEY (id)) ENGINE=InnoDB DEFAULT CHARSET=latin1"

node . # run this in app dir
```

Then go to app api ui and make some entities (I made exactly 2 each)
- http://localhost:3000/explorer/#!/Customer/Customer_create
- http://localhost:3000/explorer/#!/Appointment/Appointment_create

e.g.
```
{
  "Name": "string",
  "Country": "string"
}
```
```
{
  "Name": "string2",
  "Country": "string2"
}
```
```
{
  "CustomerID": 1,
  "Date": "string"
}
```
```
{
  "CustomerID": 2,
  "Date": "string"
}
```

You can CTRL+C the node process at will without losing data, but closing mysql will lose the data

Note mysql may not shut down properly on exit, a quick careless way of cleaning up is to run `docker rm -f $(docker ps -aq)`

## How it was made

Resources:
- http://loopback.io/doc/en/lb3/Getting-started-with-LoopBack.html
- http://loopback.io/doc/en/lb3/Getting-started-part-II.html

```
lb
? What's the name of your application? loopback-sample
? Enter name of the directory to contain the project: loopback-sample
   create loopback-sample/
     info change the working directory to loopback-sample

? Which version of LoopBack would you like to use? 3.x (current)
? What kind of application do you have in mind? empty-server (An empty LoopB
ack API, without any configured models or datasources)
```

```
cd loopback-sample

```

```
lb datasource
? Enter the datasource name: mysqlDs
? Select the connector for mysqlDs: MySQL (su
pported by StrongLoop)
? Connection String url to override other set
tings (eg: mysql://user:pass@host/db):
? host: 127.0.0.1
? port: 3306
? user: root
? password:
? database: sample
? Install loopback-connector-mysql@^2.2 Yes
+ loopback-connector-mysql@2.4.1
added 10 packages in 5.658s
 lb model
? Enter the model name: Customer
? Select the datasource to attach Customer to
: mysqlDs (mysql)
? Select model's base class PersistedModel
? Expose Customer via the REST API? Yes
? Custom plural form (used to build REST URL)
:
? Common model or server only? common
Let's add some Customer properties now.

Enter an empty property name when done.
? Property name: Name
   invoke   loopback:property
? Property type: string
? Required? Yes
? Default value[leave blank for none]:

Let's add another Customer property.
Enter an empty property name when done.
? Property name: Country
   invoke   loopback:property
? Property type: string
? Required? Yes
? Default value[leave blank for none]:

Let's add another Customer property.
Enter an empty property name when done.
? Property name:
 lb model
? Enter the model name: Appointment
? Select the datasource to attach Appointment
 to: mysqlDs (mysql)
? Select model's base class PersistedModel
? Expose Appointment via the REST API? Yes
? Custom plural form (used to build REST URL)
:
? Common model or server only? common
Let's add some Appointment properties now.

Enter an empty property name when done.
? Property name: CustomerID
   invoke   loopback:property
? Property type: number
? Required? Yes
? Default value[leave blank for none]:

Let's add another Appointment property.
Enter an empty property name when done.
? Property name: Date
   invoke   loopback:property
? Property type: string
? Required? Yes
? Default value[leave blank for none]:

Let's add another Appointment property.
Enter an empty property name when done.
? Property name:
```
