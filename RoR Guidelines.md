#Guides 

Guia de estilo para desenvolvimento JAVA.


#What Makes Up a Good Name
Use full English descriptors1 that accurately describe the variable/field/class/... 
For example, use names like `firstName`, `grandTotal`, or `CorporateCustomer`

Use terminology applicable to the domain. If your users refer to their clients as customers, then use the term `Customer` for the class, not `Client`. Many developers will make the mistake of creating generic terms for concepts when perfectly good terms already exist in the industry/domain.


Avoid long names (< 15 characters is a good idea). Although the class name `PhysicalOrVirtualProductOrService`

#Good Documentation
Comments should add to the clarity of your code. 
Keep comments simple. Some of the best comments I have ever seen are simple, point-form notes. You do not have to write a book, you just have to provide enough information so that others can understand your code.

Write the documentation before you write the code. The best way to document code is to write the comments before you write the code.

```JAVA
/**
Customer – A customer is any
person or organization that we sell services and products to.
@author S.W. Ambler 
*/
```

#Standards For Member Functions


## Naming Conventions

* Names representing packages should be in all lower case. ` mypackage, com.company.application.ui`

* Variable names must be in mixed case starting with lower case. 

 `line, audioSystem `

* Names representing methods must be verbs and written in mixed case starting with lower ` case.getName(), computeTotalWidth()`

* The name of the object is implicit, and should be avoided in a method ` name.line.getLength();   // NOT: line.getLineLength();`


* The terms get/set must be used where an attribute is accessed directly

```JAVA
employee.getName();
employee.setName(name);

matrix.getElement(2, 4);
matrix.setElement(2, 4, value);
```

* is prefix should be used for boolean variables and methods.
`isSet, isVisible, isFinished, isFound, isOpen`

* The term find can be used in methods where something is looked up.
```JAVA
vertex.findNearestVertex();
matrix.findSmallestElement();
node.findShortestPath(Node destinationNode);
```

* The term initialize can be used where an object or a concept is established.
`printer.initializeFontSet();`

* Plural form should be used on names representing a collection of objects.
```JAVA
Collection<Point>  points;
int[]              values;
```

* n prefix should be used for variables representing a number of objects.
`nPoints, nLines`

* No suffix should be used for variables representing an entity number.
`tableNo, employeeNo`

* Iterator variables should be called i, j, k etc.
```JAVA
for (Iterator i = points.iterator(); i.hasNext(); ) {
        :
}
for (int i = 0; i < nTables; i++) {
   	:
}
```

* Abbreviations in names should be avoided.
```JAVA
computeAverage();               // NOT: compAvg();
ActionEvent event;              // NOT: ActionEvent e;
catch (Exception exception) {   // NOT: catch (Exception e) {
```
*  Exception classes should be suffixed with Exception.
```JAVA
class AccessException extends Exception
{
  	:
}
```

* Classes that creates instances on behalf of others (factories) can do so through method new[ClassName]

```JAVA
class PointFactory
{
  public Point newPoint(...)
  {
    ...
  }
}
```

##Statements
* The import statements must follow the package statement.


```JAVA
import java.io.IOException;
import java.net.URL;
import java.rmi.RmiServer;
import java.rmi.server.Server;
import javax.swing.JPanel;
import javax.swing.event.ActionEvent;
import org.linux.apache.server.SoapServer;
```

##Loops
* Only loop control statements must be included in the for() construction.


```JAVA
sum = 0;                       // NOT: for (i = 0, sum = 0; i < 100; i++)
for (i = 0; i < 100; i++)      //sum += value[i];
  sum += value[i];
```



##Naming Member Functions

Examples: 
```JAVA
openAccount()
printMailingLabel() save()
delete()
```

##Getters
Examples: 
```JAVA
getFirstName()
getAccountNumber() 
getLostEh() 
isPersistent() 
isAtEnd()
```

#Techniques for Writing Clean Code

##Document Your Code

##Paragraph/Indent Your Code

###Paragraph and Punctuate Multi-Line Statements
Example:
```JAVA
BankAccount newPersonalAccount = AccountFactory
				createBankAccountFor( currentCustomer, startDate,
				 initialDeposit, branch);
```

###Use Whitespace in Your Code
```JAVA
counter=1; grandTotal=invoice.total()+getAmountDue(); 
grandTotal=Discounter.discount(grandTotal,this);
```
vs￼
```JAVA
counter = 1;
grandTotal = invoice.total() + getAmountDue(); 
grandTotal = Discounter.discount(grandTotal, this);
```
#Standards for Fields (Attributes/Properties)

##Naming Fields
Examples: 
```JAVA
firstName
zipCode 
unitPrice 
discountRate 
orderItems 
sqlDatabase
```

##Naming Collections
Examples: 
```JAVA
customers
orderItems 
aliases
```

##Naming Local Variables

###Naming Streams
```JAVA
inputStream, outputStream, and ioStream instead of in, out, and inOut
```

###Naming Loop Counters
```JAVA
i, j, or k
```



