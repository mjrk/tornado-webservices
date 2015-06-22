This document describes how to run the demos web services.

Running the DemoService.py example:
-----------------------------------

Running:

     $ python DemoServices.py

The example display four separate services.

EchoService (http://localhost:8080/EchoService?wsdl):

For an input string responds with an echo message.

CountService (http://localhost:8080/CountService?wsdl):

The operation receives as input an python list an returns the numbers of 
elements in the list.

This service used _params=xmltypes.Array(xmltypes.String) like input, 
what mean his input parameter is a array of string elements.
    
     xmltypes.Array(xmltypes.String) create an xml element with maxOccurs
     property with the value "unbounded" for wsdl.

DivService (http://localhost:8080/DivService?wsdl):

The operation receives as input two float numbers and returns the result 
of the deivision of this numbers.

Finally, FibonacciService (http://localhost:8080/FibonacciService?wsdl):

Returns a list with the fibonacci's numbers.

This services return an _return=xmltypes.Array(xmltypes.Integer) what
mean the operation of service return an list of integer elements.

     xmltypes.Array(xmltypes.Integer), create an xml element with maxOccurs
     property with the value "unbounded" for wsdl.

You can probe this services with SoapUI or creating a web services client with 
your favorite programming language.

Running the DemoServiceHostname.py example:
-------------------------------------------

The results are the same as above showed, except that in this demo you can
set the hostname to be answered in the WSDL. It's useful when you need to 
run the services in distributed servers architecture. It's gonna be useful
operating in a round-robin or reverse proxy configuration.

Running the DemoService2.py example:
------------------------------------

Running:

     $ python DemoServices2.py

The example display the same four separate services of DemoService.py.
The differences are in the types. Here we used python types (int, str, 
float, etc.). See the code.

Running the ProducService.py example:
-------------------------------------

Running:

     $ python ProductService.py

ProductService (http://localhost:8080/ProductService?wsdl):

This web service uses complextypes by transforming a python class in an
xml schema within the definition of the schemas of WSDL.

For example:
        
     from tornadows import complextypes

     class Product(complextypes.ComplexType):
          id = complextypes.IntegerPropert()
          name = complextypes.StringProperty()
          price = complextypes.FloatProperty()
          stock = complextypes.IntegerProperty()

The class is tranformed into the following xml schema.

     <xsd:complexType name="Product" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
          <xsd:sequence>
                <xsd:element name="id"    type="xsd:integer"/>
                <xsd:element name="name"  type="xsd:string"/>
                <xsd:element name="price" type="xsd:float"/>
                <xsd:element name="stock" type="xsd:integer"/>
          </xsd:sequence>
     </xsd:complexType>

In the WSDL, xml schema is seen as follows.

     <wsdl:types>
          <xsd:schema targetNamespace="http://127.0.1.1:8080/ProductService/getProduct">
               <xsd:complexType name="Product">
                     <xsd:sequence>
                          <xsd:element name="id"    type="xsd:integer"/>
                          <xsd:element name="name"  type="xsd:string"/>
                          <xsd:element name="price" type="xsd:float"/>
                          <xsd:element name="stock" type="xsd:integer"/>
                     </xsd:sequence>
               </xsd:complexType>
               <xsd:element name="Product" type="tns:Product"/>
          </xsd:schema>
     </wsdl:types>

In the example, the SOAP envelope is seen as follows.

The request is:

     <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:get="http://127.0.1.1:8080/ProductService/getProduct">
          <soapenv:Header/>
               <soapenv:Body>
                    <get:Input>
                         <idProduct>1</idProduct>
                    </get:Input>
               </soapenv:Body>
     </soapenv:Envelope>
     
The response of the web service is:

     <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
          <soapenv:Header/>
               <soapenv:Body>
                    <Product>
                         <id>1</id>
                         <name>COMPUTER</name>
                         <price>1000.5</price>
                         <stock>100</stock>
                    </Product>
          </soapenv:Body>
     </soapenv:Envelope>
    
You can use the SoapUI for testing. See the code in the ProductService.py

Running the ProducListService.py example:
-----------------------------------------

Running:

     $ python ProductListService.py

ProductListService (http://localhost:8080/ProductListService?wsdl):

The web service implements an operation to retrieve a list of products.
Python types are used to define the attributes of the class; and python
list to store a set of Product.

Input is a class that represents the input parameter (request) for the
web service:

     idList  : is a int python type.

Product is a class that represents Product data structure.

     id      : Is a int python type. Is the product id
     name    : Is a str python type. Is the name of product. 
     price   : Is a float python type. Is the price of product.
     stock   : Is a int python type. Is the stock of product.

List is a class that represents  the response of the web services.

     idList  : IS a int python type. Is a id for the list.
     product : Is a python list with a set of product (Product class).

Running the ProducListService2.py example:
------------------------------------------

Running:

     $ python ProductListService2.py

ProductListService (http://localhost:8080/ProductListService2?wsdl):

The web service implements an operation to retrieve a list of products.
Python types are used to define the attributes of the class; and python 
list to store a set of Product.

Product is a class that represents Product data structure.

     id     : Is a int python type. Is the product id
     name   : Is a str python type. Is the name of product. 
     price  : Is a float python type. Is the price of product.
     stock  : Is a int python type. Is the stock of product.

List is a class that represents the response of the web services.
This is a list of Product.

     product : Is a python list with a set of product (Product class).

The operation have not input parameters.

Running the UserRolesService.py example:
----------------------------------------

Running:

     $ python UserRolesService.py

UserRolesService (http://localhost:8080/UserRolesService?wsdl):

This web service uses python list into xml complextypes. This allows create data structures more 
complex.

The service receive a idlist (integer) and returns a list of users with username (str) and another
python list with roles.

For example:
        
     from tornadows import complextypes
    
     class User(complextypes.ComplexType):
          username = str
          roles = [str]

     class ListOfUser(complextypes.ComplexType):
          idlist = int
          list = [User]

The classes are tranformed into the following xml schema.

     <xsd:schema targetNamespace="http://127.0.1.1:8080/UserRolesService/getUsers">
          <xsd:complexType name="paramsTypes">
               <xsd:sequence>
                     <xsd:element name="idlist" type="xsd:integer"/>
               </xsd:sequence>
          </xsd:complexType>
          <xsd:element name="params" type="tns:paramsTypes"/>
          <xsd:complexType name="ListOfUser">
           <xsd:sequence>
                    <xsd:element name="idlist" type="xsd:integer"/>
                    <xsd:element maxOccurs="unbounded" name="list" type="tns:User"/>
               </xsd:sequence>
          </xsd:complexType>

          <xsd:complexType name="User">
               <xsd:sequence>
                    <xsd:element maxOccurs="unbounded" name="roles" type="xsd:string"/>
                    <xsd:element name="username" type="xsd:string"/>
               </xsd:sequence>
          </xsd:complexType>

          <xsd:element name="ListOfUser" type="tns:ListOfUser"/>
     </xsd:schema>
    
Python lists are represented like xml elements with the maxOccurs attribute unbounded.    
    
The soap request is:

     <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:get="http://127.0.1.1:8080/UserRolesService/getUsers">
          <soapenv:Header/>
               <soapenv:Body>
                    <get:params>
                         <idlist>9010</idlist>
                    </get:params>
               </soapenv:Body>
          </soapenv:Envelope>

The soap response is:

     <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
          <soapenv:Header/>
          <soapenv:Body>
               <ListOfUser>
                    <idlist>9010</idlist>
                    <list>
                         <roles>ceo</roles>
                         <roles>admin</roles>
                         <username>steve</username>
                    </list>
                    <list>
                         <roles>developer</roles>
                         <username>billy</username>
                    </list>
               </ListOfUser>
          </soapenv:Body>
     </soapenv:Envelope>

Running the CertService.py example:
----------------------------------------

Running:

     $ python CertService.py

CertService (http://localhost:8080/CertService?wsdl):

This web service returns an certificate of birth, this service uses the module datetime 
of python.

The service receives an idperson parameter like input request, and returns an xml with
the information of the "certificate":

     <CertificateResponse>
          <birthday>1973-12-11</birthday>
          <datetimecert>2011-10-04T00:25:55</datetimecert>
          <idperson>1</idperson>
          <isvalid>true</isvalid>
          <nameperson>Steve J</nameperson>
          <numcert>1</numcert>
     </CertificateResponse>

See the code of CertService.py for more details.

Running the RegisterService.py example:
----------------------------------------

Running:

    $ python RegisterService.py

    1.- RegisterService (http://localhost:8080/RegisterService?wsdl):

    This example is similar to CertService.py.

    The example uses a RegisterRequest like input to the web service.
    The operation register() of the web service simulates the service of register.
    The operation response a RegisterResponse with information of the register.

    See the code of RegisterService.py for more details.


Running the CurrentTempService.py example:
------------------------------------------

Running:

     $ python CurrentTempService.py

CurrentTempService (http://localhost:8080/CurrentTempService?wsdl):

This example simulates a service that gives the current temperature (in a unknown location).
The operation or method getTemperature() does not receive any parameters.
The operation ever return the same temperature (29).

See the code of CurrentTempService.py for more details.

Running the HelloWorldService.py example:
-----------------------------------------

Running:

     $ python HelloWorldService.py

HelloWorldService (http://localhost:8080/HelloWorldService?wsdl):

This service is the classic hello world!!!. The operation sayHello() does not receive any parameters
this method just return a string with the classic "Hello World!!!".

See the code of HelloWorldService.py for more details.

Running the HelloWorldService2.py example:
------------------------------------------

Running:

     $ python HelloWorldService2.py

HelloWorldService2 (http://localhost:8080/HelloWorldService2?wsdl):

This service is the classic hello world. The operation sayHello() does not receive any parameters
this method just return a string with the classic hello world in a array or list in python
['Hello','World'].

See the code of HelloWorldService2.py for more details.

Running the MathService.py example (a new feature):
---------------------------------------------------

Running:

     $ python MathService.py

MathService (http://localhost:8080/MathService?wsdl):

This service displays four math operations in a single service (python class): 
addition (add), subtraction (sub), multiplication (mult) and division (div).

See the code of MathService.py for more details.

This example made use of a new feature of the API, allow multiple operations
on the service, throught the use of @webservice decorator.

Running the RepositoryService.py example (a new feature):
---------------------------------------------------------

Running:

     $ python RepositoryService.py

RepositoryService (http://localhost:8080/RepositoryService?wsdl):

This service display two operations to save a document in a repository and
find a document from the same repository.

See the code of RepositoryService.py for more details.

The service uses the ComplexTypes to define the documents stored in the
repository.

We define two subclass of ComplexTypes:

    Document class represents a document.
    Message class defines message and transports documents.

To store the documents, the service uses python dictionaries.
