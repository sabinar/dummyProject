# Spring MVC and Hibernate application for User, device & application model

##Link to application:
http://pure-wave-6204.herokuapp.com/people/

Please refer link for more details:
https://github.com/mobileiron/hiring/blob/master/senior-software-engineer-spring-mvc-java.md 


#Details of application


###Person:
####Model: Person.java
This model has name and email as mandatory fields. It has a one to many relationship with device model.
####Controller: PersonController.java
Contains get and post operations for model related to add, delete and list users. 
Also handles getting device by user. Handles redirects to various pages 
####Service: PersonService.java & PersonServiceImpl.java
Interface and implementation related to add, delete, get and list users. Also getting devices by userid
####View 
people.jsp: User listing page is the first page that the users views. The user also can add a new user.
error.jsp: In case user is being deleted and the user has devices associated with it
addDeviceToUser.jsp: Provides option to add device to this specified user
deviceList.jsp: Lists the devices owned by the selected user.
####Test: PersonServiceImplTest.java
Related test cases for service implementation 
  
###Device:
####Model: Device.java
This model has phoneNumber and Operating system as mandatory fields. The phone number should of 10 digits only. It has a many to one relationship with person model and many to many relationship with application model. Overriden equals() and hashCode() methods so as to display the devices in application page.
####Controller: DeviceController.java
Contains get and post operations for model related to add, delete and list devices. 
Also handles getting applications by device. Handles redirects to various pages 
####Service: DeviceService.java & DeviceServiceImpl.java
Interface and implementation related to add, delete, get and list devices. Also getting applications by device id.
####View 
device.jsp: Lists all devices in the DB along with user details. Provision to go to user page or application page. User can also view the list of applications for the selected device.
appListPerDevice.jsp: List of applications for the selected device.
####Test: DeviceServiceImplTest.java
Related test cases for service implementation 

###Application:
####Model: Application.java
This model has App name and description of which only name is mandatory. It has many to many relationship with device model. 
####Controller: ApplicationController.java
Contains get and post operations for model related to add, delete and list applications. Contains a binder method to bind the devices to a list so that it can be displayed and added to it. 
####Service: ApplicationService.java & ApplicationServiceImpl.java
Interface and implementation related to add, delete, get and list applications. Also saves changes made to application.
####View 
application.jsp: Lists all application. User can Application.
addDeviceToApp.jsp: Add device to app. Also lists the devices that are already assigned to app.
####Test: ApplicationServiceImplTest.java
Related test cases for service implementation

####DB structure
            Table "public.person"
 Column |          Type          | Modifiers
--------+------------------------+-----------
 userid | integer                | not null
 email  | character varying(255) | not null
 name   | character varying(255) | not null
Indexes:
    "person_pkey" PRIMARY KEY, btree (userid)
Referenced by:
    TABLE "device" CONSTRAINT "fk79d00a76cb95b051" FOREIGN KEY (userid) REFERENCES person(userid)
    
                Table "public.device"
     Column      |          Type          | Modifiers
-----------------+------------------------+-----------
 deviceid        | integer                | not null
 operatingsystem | character varying(255) | not null
 phonenumber     | character varying(255) | not null
 userid          | integer                |
Indexes:
    "device_pkey" PRIMARY KEY, btree (deviceid)
Foreign-key constraints:
    "fk79d00a76cb95b051" FOREIGN KEY (userid) REFERENCES person(userid)
Referenced by:
    TABLE "device_application" CONSTRAINT "fk1a42b0e72b0eef1d" FOREIGN KEY (deviceid) REFERENCES device(deviceid)    
    
              Table "public.application"
 Column  |          Type          | Modifiers
---------+------------------------+-----------
 appid   | integer                | not null
 appdesc | character varying(255) |
 appname | character varying(255) | not null
Indexes:
    "application_pkey" PRIMARY KEY, btree (appid)
Referenced by:
    TABLE "device_application" CONSTRAINT "fk1a42b0e7a1d2fb36" FOREIGN KEY (appid) REFERENCES application(appid)

Table "public.device_application"
  Column  |  Type   | Modifiers
----------+---------+-----------
 appid    | integer | not null
 deviceid | integer | not null
Foreign-key constraints:
    "fk1a42b0e72b0eef1d" FOREIGN KEY (deviceid) REFERENCES device(deviceid)
    "fk1a42b0e7a1d2fb36" FOREIGN KEY (appid) REFERENCES application(appid)