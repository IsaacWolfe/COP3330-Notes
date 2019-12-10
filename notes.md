CS Account is **wolfe**  
  
# 8/27/19  
  
[Website](www.cs.fsu.edu/~myers/cop3330)  
Recitation (Section 1) HCB219  
  
# 8/29/19  
Int types: char, short, int, long  
Double Data types: Long, Double, Long double  
  
Example class:  
```cpp
	class Circle {  
		public:  
			void SetCenter(double x, double);	//Setter  
			void SetRadius(double r);	//Setter  
			void Draw() const;	//Getter  
			double AreaOf() const;	//Getter  
  	
		private:  
			double radius;  
			double center_x;  
			double center_y;  
	};  
```  
  
Mutator- Function that changes the objects state  
Accessor- Read only function, looks at value but doesn't change it  
  
# 09/03/19  
  
Accessors should end with const after function call (like above)  
**Constructors** needed so that the private variables have data and aren't garbage data before the user starts using class, automatically happens upon call  
Constructor always has *same name as class* and *has no return type (**Can still have parameters that need a type**)*  
**Classes do not need default constructors**  
Circle c3() **will not call default constructor, instead is a function call so will not give an error**  
Writing Classes:  
* Data  
1. What Data concepts do I need to represent this thing?  
* Numerator, Denominator  
2. What types shall I choose for my data?  
* int numerator, int denominator  
3. What rules do I need to place on the data?  
* denominator != 0  
  
When calling class in a header file don't redeclare variable with data type  
# 09/03/19  
  
Use \" for around header files you made, <> for default packages like iostream  
Ex.   
```cpp
	#include "frac.h"  
```  
    
Full Class Example (circle):  
``` 
	int main() {  
		Circle c1;	// Runs default constructor  
		Circle c2(10);	//Runs constructor with parameter to set radius  
  	
		c1.SetCenter(5,10);  
		c1.SetRadius(10);  
		return 0;  
	}  
  	
	class Circle {  
		public:  
			Circle();	//Constructor with 0 parameters, default constructor  
			Circle (double r);	//Still a constructor  
		  	
			void SetCenter(double x, double y);  
			void SetRadius(double r);  
  	
			void Draw() const;  
			double GetArea() const;  
  	
		private:  
			double radius;  
			double center_x;  
			double center_y;  
	}  
  	
	Circle () {  
		radius = 1;  
		center_x = 0;  
		center_y = 0;  
	}  
  	
	Circle(double r) {  
		if (r > 0)  
			radius = r;  
		else  
			radius = 1;  
		center_x = 0;  
		center_y = 0;  
	}  
  	
	void SetCenter(double x, double y) {  
		c1.center_x = x;  
		c1.center_y = y;  
	}    
```  
	
# 09/10/19
**Compile vs linking**
Flags for g++ that are needed:
c- Checks compilation and creates .o files, checks syntax
o *Name for file to become*- Renames files to the name to be given, *ideal to do this as last argument so code isn't every overwritten*
.cpp -(-c, compile)-> .o -(linking)-> exec

Make files-

endl and flush will make sure the buffer is flushed and printed to the screen before CPU continues, \n does not flush the buffer so can be printed slower
cerr (in iostream) is printed instantly to wherever it is meant to go, not buffered
clog (in iostream) is not in the buffer

Keyword **Friend** grants full access to everything within the class, neither public nor private

```  
	class Fraction {
			friend bool Equals(Fraction x, Fraction y); // Must be in class block to access data
	}
```  

# 09/12/19
**Make Files**
Not ideal to #include but rather have a make file with it
For cpp and h files do g++ -c *file* to get .o file
.o files can be combined for an executable
Make File- script to compile programs
``` 
Filename: makefile
<target_name>: <dependency_list>
	<commands>
```
look into **GDB**
Conversion Constructor
Pass by reference must send data that is changeable, no data that is always a value aka number or operator, unless operator is assigned to a variable
  
# 09/17/19
Thing::Thing(parameters):CONSTVAR(value) {}  
static const int SIZE = 10; // Says that SIZE Can only exist once  
**Deconstructor**- ~ConstructorName();
Thing::~Thing(){
		cout << "Running deconstructor";
}
Scope != extent- extent is how long until the variable is deallocated, scope is how long it can be used (usually the end of function created in)
Precedence- which operators are used first
For **Operator Overloading** you can't change precedence (for built in operators), associativity, arity (the amount of options it takes, plus takes 2 numbers), can't create new operators (only changing existing operators)    

# 09/24/19
**Ostream** should always be pass by strict reference, **istream** as well  
```
	void Fraction::Show() { //Member function 
			cout << numerator << "/" << denominator;
	}  
	// OR
	ostream& operator<<(ostream& s, const Fraction& f) { //Friend of the class, not member function
			s << f.numerator << '/' << f.denominator;  
			return s;
	}
```
  
ofstream can go to any output such as files, cout can only print to screen    
istream& operator>>(istream& is, Fraction& f); //For input other than cin, can come from any input as opposed to just cin  
Pre increment (++count) will increment first since function returns the new value before line is completed, post increment returns the original value so that the value is not changed before being used  
```
	//Post increment has a extra int parmeter to differentiate the functions
	*Member Function Versions*
	++f  
	Fraction operator++();  pre-increment  
  	
	f++  
	Fraction operator++(int); post-increment      
  	
	*Friend function*
	++f  
	Fraction operator++(Fraction& f);  pre-increment  
  	
	f++  
	Fraction operator++(Fraction& f, int); post-increment    
```  
**This** is a pointer to the calling object (only usable from inside a member function of a class  
\*p dereferences pointer p  
return \*this; //Returns the calling object  
(\*this).denominator == this->denominator  
  
**Composition**    
Is the use of one class object to build another class object and so on, aka Has-a relationship  

# 09/26/19
Operator Overloading- operator(+,-,==,etc)  
Example of Overloading fully created (Member function)  
```cpp
	#include <iostream>
	#include <cstdlib>
	#include <cmath>
	using namespace std;
	
	//Class for amounts of money in U.S. currency.
	class Money
	{
	public:
    	Money( );
    	Money(double amount);
    	Money(int dollars, int cents);
    	Money(int dollars);
    	double getAmount( ) const;
    	int getDollars( ) const;
    	int getCents( ) const;
    	void input( ); //Reads the dollar sign as well as the amount number.
    	void output( ) const;
    	const Money operator +(const Money& amount2) const;
    	const Money operator -(const Money& amount2) const;
    	bool operator ==(const Money& amount2) const;
    	const Money operator -( ) const;
	private:
    	int dollars; //A negative amount is represented as negative dollars and
    	int cents; //negative cents. Negative $4.50 is represented as -4 and -50
	
    	int dollarsPart(double amount) const;
    	int centsPart(double amount) const;
    	int round(double number) const;
	};
	
	int main( )
	{
    	Money yourAmount, myAmount(10, 9);
    	cout << "Enter an amount of money: ";
    	yourAmount.input( );
	
    	cout << "Your amount is "; 
    	yourAmount.output( ); 
    	cout << endl;
    	cout << "My amount is "; 
    	myAmount.output( ); 
    	cout << endl;
	
    	if (yourAmount == myAmount)
        	cout << "We have the same amounts.\n";
    	else
        	cout << "One of us is richer.\n";
	
    	Money ourAmount = yourAmount + myAmount;
    	yourAmount.output( ); cout << " + "; myAmount.output( ); 
    	cout << " equals "; ourAmount.output( ); cout << endl;
	
    	Money diffAmount = yourAmount - myAmount;
    	yourAmount.output( ); cout << " - "; myAmount.output( ); 
    	cout << " equals "; diffAmount.output( ); cout << endl;
	
    	return 0;
	}
	
	const Money Money::operator +(const Money& secondOperand) const
	{
    	int allCents1 = cents + dollars*100;
    	int allCents2 = secondOperand.cents + secondOperand.dollars*100;
    	int sumAllCents = allCents1 + allCents2;
    	int absAllCents = abs(sumAllCents); //Money can be negative.
    	int finalDollars = absAllCents/100;
    	int finalCents = absAllCents%100;
	
    	if (sumAllCents < 0)
    	{
        	finalDollars = -finalDollars;
        	finalCents = -finalCents;
    	}
	
    	return Money(finalDollars, finalCents);
	}
	
	const Money Money::operator -(const Money& secondOperand) const
	{
    	int allCents1 = cents + dollars*100;
    	int allCents2 = secondOperand.cents 
                    	+ secondOperand.dollars*100;
    	int diffAllCents = allCents1 - allCents2;
    	int absAllCents = abs(diffAllCents); 
    	int finalDollars = absAllCents/100;
    	int finalCents = absAllCents%100;
	
    	if (diffAllCents < 0)
    	{
        	finalDollars = -finalDollars;
        	finalCents = -finalCents;
    	}
	
    	return Money(finalDollars, finalCents);
	}
	bool Money::operator ==(const Money& secondOperand) const
	{
    	return ((dollars == secondOperand.dollars)
            	&& (cents == secondOperand.cents));
	}
	
	const Money Money::operator -( ) const
	{
    	return Money(-dollars, -cents);
	}
	
	Money::Money( ): dollars(0), cents(0)
	{/*Body intentionally empty.*/}
	
	Money::Money(double amount)
              	: dollars(dollarsPart(amount)), cents(centsPart(amount))
	{/*Body intentionally empty*/}
	
	Money::Money(int theDollars)
              	: dollars(theDollars), cents(0)
	{/*Body intentionally empty*/}
	
	//Uses cstdlib:
	Money::Money(int theDollars, int theCents)
	{
    	if ((theDollars < 0 && theCents > 0) || (theDollars > 0 && theCents < 0))
    	{
        	cout << "Inconsistent money data.\n";
        	exit(1);
    	}
    	dollars = theDollars;
    	cents = theCents;
	}
	
	double Money::getAmount( ) const
	{
    	return (dollars + cents*0.01);
	}
	
	int Money::getDollars( ) const
	{
    	return dollars;
	}
	
	int Money::getCents( ) const
	{
    	return cents;
	}
	
	//Uses iostream and cstdlib:
	void Money::output( ) const
	{
    	int absDollars = abs(dollars);
    	int absCents = abs(cents);
    	if (dollars < 0 || cents < 0)//accounts for dollars == 0 or cents == 0
        	cout << "$-";
    	else
        	cout << '$';
    	cout << absDollars;
	
    	if (absCents >= 10)
        	cout << '.' << absCents;
    	else
        	cout << '.' << '0' << absCents;
	}
	
	//Uses iostream and cstdlib:
	void Money::input( )
	{
    	char dollarSign;
    	cin >> dollarSign; //hopefully
    	if (dollarSign != '$')
    	{
        	cout << "No dollar sign in Money input.\n";
        	exit(1);
    	}
	
    	double amountAsDouble;
    	cin >> amountAsDouble;
    	dollars = dollarsPart(amountAsDouble);
    	cents = centsPart(amountAsDouble);
	}
	
	int Money::dollarsPart(double amount) const
	{
    	return static_cast<int>(amount);
	}
	
	int Money::centsPart(double amount) const
	{
    	double doubleCents = amount*100;
    	int intCents = (round(fabs(doubleCents)))%100;//% can misbehave on negatives
    	if (amount < 0)
        	intCents = -intCents;
    	return intCents;
	}
	
	int Money::round(double number) const
	{
    	return static_cast<int>(floor(number + 0.5));
	}
```      
Operator Overload (ostream)        
  
```
	#include <iostream>  
	#include <cstdlib>
	#include <cmath>
	using namespace std;
	
	//Class for amounts of money in U.S. currency.
	class Money
	{
	public:
    	Money( );
    	Money(double amount);
    	Money(int theDollars, int theCents);
    	Money(int theDollars);
    	double getAmount( ) const;
    	int getDollars( ) const;
    	int getCents( ) const;
    	friend const Money operator +(const Money& amount1, const Money& amount2);
    	friend const Money operator -(const Money& amount1, const Money& amount2);
    	friend bool operator ==(const Money& amount1, const Money& amount2);
    	friend const Money operator -(const Money& amount);
    	friend ostream& operator <<(ostream& outputStream, const Money& amount);
    	friend istream& operator >>(istream& inputStream, Money& amount);
	private:
    	int dollars; //A negative amount is represented as negative dollars and
    	int cents; //negative cents. Negative $4.50 is represented as -4 and -50
	
    	int dollarsPart(double amount) const;
    	int centsPart(double amount) const;
    	int round(double number) const;
	};
	
	int main( )
	{
    	Money yourAmount, myAmount(10, 9);
    	cout << "Enter an amount of money: ";
    	cin >> yourAmount;
    	cout << "Your amount is " << yourAmount << endl;
    	cout << "My amount is " << myAmount << endl;
	
    	if (yourAmount == myAmount)
        	cout << "We have the same amounts.\n";
    	else
        	cout << "One of us is richer.\n";
	
    	Money ourAmount = yourAmount + myAmount;
    	cout << yourAmount << " + " << myAmount
         	<< " equals " << ourAmount << endl;
	
    	Money diffAmount = yourAmount - myAmount;
    	cout << yourAmount << " - " << myAmount
         	<< " equals " << diffAmount << endl;
	
    	return 0;
	}
	
	ostream& operator <<(ostream& outputStream, const Money& amount)
	{
    	int absDollars = abs(amount.dollars);
    	int absCents = abs(amount.cents);
    	if (amount.dollars < 0 || amount.cents < 0)
        	//accounts for dollars == 0 or cents == 0
        	outputStream << "$-";
    	else
        	outputStream << '$';
    	outputStream << absDollars;
	
    	if (absCents >= 10)
        	outputStream << '.' << absCents;
    	else
        	outputStream << '.' << '0' << absCents;
	
    	return outputStream;
	}
	
	//Uses iostream and cstdlib:
	istream& operator >>(istream& inputStream, Money& amount)
	{
    	char dollarSign;
    	inputStream >> dollarSign; //hopefully
    	if (dollarSign != '$')
    	{
        	cout << "No dollar sign in Money input.\n";
        	exit(1);
    	}
	
	
    	double amountAsDouble;
    	inputStream >> amountAsDouble;
    	amount.dollars = amount.dollarsPart(amountAsDouble);
    	amount.cents = amount.centsPart(amountAsDouble);
	
	
    	return inputStream;
	}
	
	const Money operator +(const Money& amount1, const Money& amount2)
	{
    	int allCents1 = amount1.cents + amount1.dollars*100;
    	int allCents2 = amount2.cents + amount2.dollars*100;
    	int sumAllCents = allCents1 + allCents2;
    	int absAllCents = abs(sumAllCents); //Money can be negative.
    	int finalDollars = absAllCents/100;
    	int finalCents = absAllCents%100;
	
    	if (sumAllCents < 0)
    	{
        	finalDollars = -finalDollars;
        	finalCents = -finalCents;
    	}
	
    	return Money(finalDollars, finalCents);
	}
	
	//Uses cstdlib:
	const Money operator -(const Money& amount1, const Money& amount2)
	{
    	int allCents1 = amount1.cents + amount1.dollars*100;
    	int allCents2 = amount2.cents + amount2.dollars*100;
    	int diffAllCents = allCents1 - allCents2;
    	int absAllCents = abs(diffAllCents); 
	
    	int finalDollars = absAllCents/100;
    	int finalCents = absAllCents%100;
	
    	if (diffAllCents < 0)
    	{
        	finalDollars = -finalDollars;
        	finalCents = -finalCents;
  	}
	
    	return Money(finalDollars, finalCents);
	}
	
	bool operator ==(const Money& amount1, const Money& amount2)
	{
    	return ((amount1.dollars == amount2.dollars)
           	&& (amount1.cents == amount2.cents));
	}
	
	const Money operator -(const Money& amount)
	{
    	return Money(-amount.dollars, -amount.cents);
	}
	
 	
	Money::Money( ): dollars(0), cents(0)
	{/*Body intentionally empty.*/}
	
	Money::Money(double amount)
              	: dollars(dollarsPart(amount)), cents(centsPart(amount))
	{/*Body intentionally empty*/}
	
	Money::Money(int theDollars)
              	: dollars(theDollars), cents(0)
	{/*Body intentionally empty*/}
	
	//Uses cstdlib:
	Money::Money(int theDollars, int theCents)
	{
    	if ((theDollars < 0 && theCents > 0) || (theDollars > 0 && theCents < 0))
    	{
        	cout << "Inconsistent money data.\n";
        	exit(1);
    	}
    	dollars = theDollars;
    	cents = theCents;
	}
	
	double Money::getAmount( ) const
	{
    	return (dollars + cents*0.01);
	}
	
	int Money::getDollars( ) const
	{
    	return dollars;
	}
	
	int Money::getCents( ) const
	{
    	return cents;
	}
	
	int Money::dollarsPart(double amount) const
	{
    	return static_cast<int>(amount);
	}
	
	int Money::centsPart(double amount) const
	{
    	double doubleCents = amount*100;
    	int intCents = (round(fabs(doubleCents)))%100;//% can misbehave on negatives
    	if (amount < 0)
        	intCents = -intCents;
    	return intCents;
	}
	
	int Money::round(double number) const
	{
    	return static_cast<int>(floor(number + 0.5));
	}
```  
Operator Overload (pre and Post increment)  
```
	#include <iostream>
	#include <cstdlib>
	using namespace std;
	
	class IntPair
	{
	public:
    	IntPair(int firstValue, int secondValue);
    	IntPair operator++( ); //Prefix version
    	IntPair operator++(int); //Postfix version
    	void setFirst(int newValue);
    	void setSecond(int newValue);
    	int getFirst( ) const;
    	int getSecond( ) const;
	private:
    	int first;
    	int second;
	};
	
	int main( )
	{
    	IntPair a(1,2);
    	cout << "Postfix a++: Start value of object a: ";
    	cout << a.getFirst( ) << " " << a.getSecond( ) << endl;
    	IntPair b = a++; 
    	cout << "Value returned: ";
    	cout << b.getFirst( ) << " " << b.getSecond( ) << endl;
    	cout << "Changed object: ";
    	cout << a.getFirst( ) << " " << a.getSecond( ) << endl;
	
    	a = IntPair(1, 2);
    	cout << "Prefix ++a: Start value of object a: ";
    	cout << a.getFirst( ) << " " << a.getSecond( ) << endl;
    	IntPair c = ++a;   
    	cout << "Value returned: ";
    	cout << c.getFirst( ) << " " << c.getSecond( ) << endl;
    	cout << "Changed object: ";
    	cout << a.getFirst( ) << " " << a.getSecond( ) << endl;
    	return 0;
	}
	
	IntPair::IntPair(int firstValue, int secondValue) 
                     	: first(firstValue), second(secondValue)
	{/*Body intentionally empty*/}
	
	IntPair IntPair::operator++(int ignoreMe) //postfix version
	{
    	int temp1 = first;
    	int temp2 = second;
    	first++;
    	second++;
    	return IntPair(temp1, temp2);
	}
	
	IntPair IntPair::operator++( ) //prefix version
	{
    	first++;
    	second++;
    	return IntPair(first, second);
	}
	
	void IntPair::setFirst(int newValue)
	{
    	first = newValue;
	}
	
	void IntPair::setSecond(int newValue)
	{
    	second = newValue;
	}
	
	int IntPair::getFirst( ) const
	{
    	return first;
	}
	
	int IntPair::getSecond( ) const
	{
    	return second;
	}
```  
  
Const Money& amount2- can read in literal values (due to the const) and variable names since it's a reference  
Conversion Constructor- Changes 1 parameter into new Class object, ex. int into dollars  
Operator keyword tells computer that we are overloading existing operator  
**Const** on far left- means return is const, before parameter data- the parameters are const variables, on far left of member functions- data in member function isn't being changed (getter function)  
*Read const right to left*  
  
Timer::Timer(): hours(24), minutes(60) // Initialize timer with first display setting hours and minutes to display constructor *then* to timer constructor, if 24 and 60 are empty then it calls default constructor  
Can chain dot operators but must be the right type of return on the left to be accepted and in the scope of object on right of dot operator  
*Array* collection of indexed data of all the same kind  

# 10/01/19
## Test 1 Review  
### Classes
C- data and functions are seperate    
Classes are either public or private, private holds variables that users shouldn't have direct access to (usually variables that are being used)   
Mutators- member functions that alter data, common example of a mutator is a setter function that sets a value to a variable in private 
Accesssors- member functions will *not* change the state of an object, usually defined with *const*, common example of accessor functions is a *getter* function  
Constructor should be in Public data, 
Constructor- upon calling is made to explicitly make sure that the object is in a valid semantic calling state to later be used, has **no return type** and **same name as the class**    
**Default constructor is not needed**, depends on the program  
DDU Design is Declare -> Define -> Use  

Reasons for Data Hiding with Private:    
* Makes interface easier  
* Principle of least privilege  
* Most secure  
* Class implementation easy to change without affecting other modules being used  

Principles of Good Design:     
* Decide on semantic meaning of objects of this class type  
* Goal is to ensure that an object is *always* in a valid semantic state, meaning that valid input cannot enter into a variable   
* Only put member functions needed to be called explicitly in the public section  
* Data should be in the private so that variable assignments are controlled to be valid information to be used  
  
Default Access Modifier is *private*  
:: is the Scope Resolution Operator  
  
### Compilation and Debugging  
Header Files contain declarations of the class, without the implementaion details, usually .h files  
Implementation File contains implementations of the class members, usually .cpp files  
  
During Compilation stage:  
* Syntax checked for correctness  
* Variables and function calls checked to insure the correct declarations (comiler doesn't need to match function definition to their calls)  
* Translations into *object code* (not an executable)  
  
Linking stage:  
* Links object code to executable  
* May involve one or more object code files  
* Function calls are matched with the definitions, comiler checks to ensures each function has one, and only one, definition for the called functions  
* End result is usually an executable   
  
**Combining multiple files**  
Can use #include "name.h" and then compile the main .cpp, generally *not a good idea*  
Rule of Thumb- Only #include header files, not the .cpp files  
  
### Types of Errors  
Compilation Errors- usually syntax errors, undeclared variables, and functions, improper function calls  
Linker Errors- usually involve undefined functions or multiply-defined functions or symbols  
Run-time Errors- Two varieties, only one that will compile still:  
1. Fatal- cause program to crash during execution  
2. Non-fatal (or logical)- don't crash but cause errorneous results  
Debugging Compile stage errors:  
1. Start at the top of the list of errors  
2. Debug one file at a time if there are multiple files  
3. When looking at line number also look around/above the line given  
4. Compile as you go  
  
Debugging Linking stage errors:  
1. Learn what causes linking errors  
2. Linker errors specify some kind of symbol which often resembles a function or variable name  
3. Learn about good compilation techniques and pre-processor directives  
  
Debugging Run-time errors:  
1. To catch fatal errors use prints to check where it crashes  
2. To catch logic errors use print statements to locate where it fails  
  
## More about Classes  
### Friends and Members  
If you were to create a bool Equals(Frac x, Frac y) function without it being a member function you can use *friend*  
**Friend** allows a class to grant full access to private data in the class  
Example:  
```  
	// Declared in class, normal function creation and call in .cpp
	friend bool Equals(Frac x, Fracy);    
```  
Member function instead of friend  
* When a function has two parameters usually is better to pass both variables as parameters and make it a friend   
* If it is a member function then one parameter needs to be the calling object Ex. `f1.equals(f2);`  
  
### Conversion Constructor  
Similar to automatic type conversion of double + int = double  
If you were to have `Fraction::Fraction(int n)` and you make that create and return a fraction of n/1 you can have:  
```  
	Fraction f1, f2;  
	f1 = Fraction(4);  
	f2 = 10;  
	f1 = add(f2, 5);  
```  
All the above are legal, f2 and 5 are both sent to the conversion constructor due to passing just an int  
One can also create a conversion constructor by having a default value Ex. `Fraction(int n, int d = 1)` creates a fraction even if only one parameter is passed and just defaults to 1  
If one needs a constructor with one parameter but it *shouldn't* be a conversion constructor one can use `explicit Fraction(double d)` which means the constructor can only be explicitly called  
  
### Const in classes  
The example using pass-by-value parameters:
`friend Fraction Add(Fraction f1, Fraction f2);`    

Since parameters in this should be const a better way is:    
`friend Fraction Add(const Fraction& f1, const Fraction& f2);`      
Since the parameters are const, r-values can be sent in (just like with pass-by-reference)  
### Const member functions  
If you were to use `void Func(int x) const;` means that the function **cannot change** the calling object itself  
* Can only be done to member functions of a class (not stand-alone functions)  
* Member function will not change the member data of the object    
When using this feature the word const must go on the declaration and on the definition (if the definition is seperate)  
**Should use const whenever possible**:  
* Constructors- initialize the object, so **not const**  
* Mutators- change member data, so **not const**  
* Accessors- any member function that will *not* change the member data (anything in the accessor category) **should be a const member function**  
Const variables **must be initialized on the same line**  
Can create const objects as long as a valid constructor is called with the creation of the const variable  
**A const object may only call const member functions** so that const object cannot be altered  
In a class in private you **cannot initialize a value** in the private section  
To create a const private variable you **must use** an *initialization list*  
Member function must look like this:  
```  
	returnType functionName(parameterList) : initializationList (LIMIT=10) {  
			// function body    
	}  
```  
### Destructors
Use tilde infront of the **default constructor** name ex. `~Fraction();`  
Destructors are called automatically and cannot have parameters  
Used to clean up allocated memory  
Put last in public after all the other constructors  
  
## Operator Overloading  
Done to create familiar operator notation on *programmer defined* types (or classes)  
* This cannot change the operators precedence  
* Cannot change its associativity  
* Cannot change its arity (number of operands or parameters, unary binary etc)  
* Cannot create new ones just change existing   
* Operator meaning on built in types cannot be changed  
With binary overloading first operand is the *calling object* and the second is the *parameter* if a member function **or** two parameters if just a function  
Unary operator has one operand and is either a stand alone function with one parameter **or** member function with one calling object  
Always in format of `returnType operator(symbol) (parameterList);`  
If being done as a member function best to look like `Fraction operator+(const Fraction& f) const;` since return value   
If overloading insertion (<<) and extraction (>>) operators, since we can't access ostream/istream library they are made **friend functions** with the **first parameter being ostream&** and the **second parameter being the new type**  
`friend ostream& operator<< (ostream& s, const Fraction& f);` or `friend istream& operator>> (istream& s, Fraction f);`  
If using ostream like a Show() function the key differences are:  
* there must be a return statement   
* must use s (or name of the ostream parameter), not cout `s << f.num << '/' << f.denom;`  
* Still a friend function so needs dot operators to access private data  
  
## Aggregation and Composition  
Relationship between objects, nesting classes within each other  
Often referred to as a "has-a" relation, a car "has-a" engine  
Must create the class in order, if second class has types of the first class it must follow that class  
When an object is created the constructor runs and if there is any embedded objects those are also called  
To set values to embedded values you need to use an **initialization list** `Timer::Timer() :hours(24), minutes(60) {}` sets hours to 24 and minutes to 60 by default if timer is created with no arguments to set hours and minutes in the Display class  
When accessing embedded data **pay attention to scopes for when a value must be accessed by a dot operator versus when you are already working in that class**  
  
## Arrays and Classes  
Arrays are handled the same as with built in data types, except when initializing you need to use curly braces and call the constructor you want to use  
`Fraction numList[4];` creates an array of 4 fractions all **using the default constructor**  
`Fraction numList[3] = {Fraction(2,4), Fraction(5), Fraction()}` creates arrays using the three varying constructors  
**Constructors must be explicitly called**  
To access member functions for a array one would need `arrayName[index].memberName`
Ex. `numList[2].Show();`  
  
## Output Formatting  
Only *width()* and *setw()* affect **only** the next items in the output stream  
**Everything else are pernament**  
[Link to notes](http://www.cs.fsu.edu/~myers/cop3330/notes/formatting_special.html)  
  
## Friend and Member Functions for Operator Overloading  
Don't use a friend for multiple classes due to how would it be used for multiple functions  
  
## Const locations    
If const is rightmost- const member function  
If const is leftmost- const return type  
Const int& x is **always better** than const int x, second makes an unnecessary copy  
Preincrement, << and >> all return a const reference value     

[Top of test 1 review](#test-1-review)  
  
# 10/10/19
## Pointers and C-Strings  
Pointers store addresses (use of \*)  
`int * p, j, l;` Declares 1 pointer variable and 2 int variables  
`int *p, *j, *l;` Declares 3 pointers  
Pointers don't always point to a valid target, memory location might not be assigned yet (good idea to help this is by using `int *p = null; or int *p = 0;` better to use keyword null) Cannot dereference null space  
int \* ptr;  
ptr = (can be NULL, address, other pointers, **Cannot give value**)  
Good Idea is to use:  
```
if (*p != 0)    
		cout << *p;  
```    
`ip = reinterpret<int *>(dp);` recasts int pointer to declared double pointer  
**& returns address** need to use `int *p = &num1;` to assign address of num1 to p  
& also used for pass-by-reference  
\* before a pointer dereferences it, returns address  
`cout << *p;` would return value of variable assigned to address
`cout << &p;` would return the *pointers address*    
`cout << p;` would return address of the variable assigned   
## Pass By Address    
Done by passing by parameter  
`void Square(int * p)` is function   
to pass into ^^ function you need to pass address of variable, best way to do this is: **`Square(&num1);`**  
Can return by address, needs to return a pointer  
## Pointers and Arrays  
```  
int list[10];  
int *p;  
p = list;  //Legal, names of arrays point to memery location of first array parameter
```  
`*p = 100;` will point to starting memory address of an array and assign 100  
`*(p + 2) = 10;` assigns array value at index [2] to 10, p is starting array location and 2 is 2 int lengths of memory added to the starting index and assign the value (Important for computer org)  
## Pass by Address with Arrays  
```  
void Swap(int * list, int a, int b) {
		int temp = list[a];  //or int temp = *(list + a);
		list[a] = list[b];  
		list[b] = temp;
}    
void Swap(int list[], int a, int b);
```  
Return by address:  
```    
int * ChooseList(int * list1, int * list2_ {
		if (list1[0] < list2[0])  
				return list1;  
		else  
				return list2;
}
```  
## Using Const with Pass by Address  
Can 1. Say the pointer only refers to one address, or 2. Pointer variable value is always the same value  
`const typeName * v;` says the value at v cannot change, pointer to constant data  
`int * const ptr = &x;` makes the address constant, must be initialized here  
`const int * const ptr = &x;` cannot change value of variable or memory address  
## Const Pointers and C-Style Strings  
`char *greeting = "Hello";` **cannot be changed**  so better to use `const char * greeting = "Hello";`  
## Dynamic Memory  
**New** is the keyword  
Need to always remember to deallocate  
To deallocate the keyword is **delete**
Can use:  
* new int; //Access it via pointers  
* new double;     
* new int[40];    
* new int[size];
```  
int * p;  
p = new int;  
```  
`delete ptr; // deletes the space at the pointer not the pointer`   
```  
delete [] arrayName; //Deletes array
list = 0; //Set pointer to null location  
```  
**Suggested for upcoming homeworks use linprog so memory isn't an issue**  
After new **always have a type not a variable name**  
Types of user defined types: enumeration, struct, class, typedef  
`cout << ptr[1];` is the same as `cout << *(ptr + 1);`  
`Fraction * ptr = new Fraction;` is a dynamic default constructor  
`Fraction * ptr2 = new Fraction(2,3);` is a dynamic constructor with values passed to fraction  
Order of Precedence- \. has higher precedence then \* so `*ptr.Show();` will not work either can use `(*ptr).Show();` or `ptr->Show();`  
Lifesaver for nested classes `displayPtr->timerPtr->ShowHours();`  
Only use arrow notation (`->`) in single dynamic class, in **dynamic arrays just use brackets with index followed by dot operator (`.`)**  
**For expanding an existing array**:    
```  
int * list;  
list = new int[size];  
  
int * temp = new int[size + 5];    
  
for (int i = 0; i < size; i++) // Can never run the loop past the size of the smaller array
		temp[i] = list[size];  
  
delete [] list; //if not immediately reassign memory should be set to NULL or 0

list = temp; //causes memory leak without deleting first
```  
Stack (function call stack) is where local variables are placed if they are static  
Heap is where the variables with new are created  
Because of the ^^ can't put dynamic variables inside of a static object, but can put **a named pointer inside the class to refer to a dynamic variable**  

# 10/15/19
When returning an array need `const char* GetElementOfArray() const;` end const is also for not changing calling object  
Get and Getline get the whole line including whitespace, third parameter is delimiter   
Grow() for increasing memory for an array needs to be in private data so users can't access and alter memory, and it should be set up to work with all arrays so a simple call can be used on each array to be expanded  
Need destructor for dynamic constructors, `delete [] arrayName`  
Delete on random memory location will usually crash, although delete on null pointer will **not** crash  
Example of Grow() func:  
```  
void Array::Grow() {
	maxSize = currentSize + 5;  
	Entry* newList = new Entry[maxSize];  
	for (int i = 0; i < currentSize; i++) {
			newList[i] = entryList[i];
	}  
	delete [] entryList;  
	entryList = newList;
}  
```  
For search functions of array returning int, if the value is not found -1 should be returned as -1 can never be an index  
Example of find func():  
```  
int Directory::FindName(char* aName) const {
	for (int i = 0; i < currentSize; i++) {
		if (strcmp(enrtyList[i].GetName(), aName) == 0)  
				return i;
	}  
	return -1;
}  
```  
Returns -1 if nothing is found, else returns index   
[Examples of Update and Remove functions as well](http://www.cs.fsu.edu/~myers/cop3330/examples/phonebook/directory.cpp)  
Make sure when setting up an array of self-created data make sure to have a default constructor so data isn't needed to be passed upon array creation  
Classes always have a constructor, destructor, assignment operator, copy constructor  
*Default Memberwise Copy* (aka Shallow copy)- Each member data goes to corresponding member data of the same class, **in the case of copying pointers the location points to the same object when using default memberwise copy, must overload assignment operator**  
**Deep Copy**-   
Copy constructor and Assignment operators are needed to be created when using pointers or reference variables where they are parameters of a class  
Copy constructor is when object is immediately created and assigned another value, `Fraction f5 = f1;` or `Fraction f5(f1);` *most important location of this is whith pass by value*
Assignment operator is `f1 = f2 + f3;` or `f1 = f2;`  

# 10/17/19
**GDB**- Important for debugging    
## C-Strings  
Character arrays ending in a null character, \0 is null  
**Cctype** helps to convert characters  
*Always need an extra space for cstring so that the null character is inserted*  
*Get* leaves delimiter in the stream    
`ch = cin.get(); // gets one character` 
`char* get(char cstringName[], int length, char delimiter='\n'); //Leaves delimiter in stream`  
`char* getline(char cstringName[], int length, char delimiter='\n'); //Removes delimiter`  
If you want operations for cstrings use *#include <cstring>*, gives you strlen, strcmp, strcpy, strcat    
**Example of Fraction copy constructor**:  
```    
//Prototype  
Directory(const Directory& d);  
  
//Function
Directory::Directory(const Directory& d) { //Can't use pass by value because it would make a copy BEFORE copy constructor
	maxsize = d.maxsize;
	currentsize = d.currentsize;  
	  
	entryList = new Entry[maxsize];  
	    
	for (int i = 0; i < currentsize; i++) {
		entryList[i] = d.entryList[i];
	}
}  
```  

# 10/24/19
Dynamic Memory Allocation  
`*ptr == ptr[]`    
```  
int &Array::operator[] (int subscript) {
	if (subscipt < 0 || subscript >= size) {
		cout << "Out of range" << endl;  
		exit(1);
	}  
	return ptr[subscript]; // Or *(ptr + subscript);
}  
```  

# 10/29/19
Make sure that functions are clearly agreeing on rules  
Keep memory management and algorithms separate   
**Declaring Derived Class**  
```  
class Sport {
}  
class Football : public Sport {
}  
```  
Football inherits attributes of Sport as well as it's own, Football inherits Sports data  
**Protected** is another data type that is similar to private *but* accessible by inherited classes only  

# 11/11/19
## Test 2 Review 
[Test 1 Review](#test-1-review)
**Arrays and Classes**  
Declaring needs the DataType nameOfArray[sizeOfArray]; so that program knows how many bits to set aside to store array size  
Each array position is a single object, Ex. Fraction rationals[20] is an array of 20 (0-19) fraction objects  
If initializing list of objects in array at values not in default constructor use Fraction numList[2] = {Fraction(2,4), Fraction(5)};  
Accessing data using dot operator, ArrayName[3].Show() calls show member function of class Fraction on ArrayName object at index of 3  
C-style arrays are unsafe naturally so if creating member data of a class by array, the creator needs to add in boundary checking and error-checking  
### Dynamic Memory  
Static memory is handled at compile time  
Dynamic memory is handled at run-time, can't have a name but uses a pointer to point to start of memory location  
Keyword *new* is always and only used with dynamic memory  
int\* ptr = new int; //Declaration of one dynamic int  
double\* nums = new double[size]; //Declaration of array of doubles dynamically allocated  
New returns address to be stored in pointer  
delete ptr; //Deallocates ptr of dynamic int  
delete [] nums; //Deallocates ptr named nums that points to an array  
### Using new with objects  
If parameters are given calls constructor matching parameters, without parameters calls default constructor  
To make a dynamic array of an object use `ptr = new Fraction[20];` creates an array of 20 fraction objects  
**Dot operator vs Arrow operator**    
Dot operator requires an object *name*.MemberName  
Arrow operator dereferences ptr so needs pointerToObject->MemberName  
If you don't use an arrow operator an equivalent is (\*ptr).Show();    
**(\*ptr).Show(); == ptr->Show();**  
When working with arrays of dynamic memory, flist[3]->Show() **will not work** (Due to flist[3] being an object to memory), flists[3].Show() would work  
To work with a dynamic array in an object or class one must include**only the pointer to the array** in the class and not the dynamic array itself  
*Always initialize pointers in constructor, best to initialize to null unless allocating right away*  
When working with dynamic array in a class one should include *delete* in the **destructor**  
*Tip:* separate memory management form functionality/algorithmic tasks  
Ex. Grow() and Shrink() functions  
### Automatics, Copy Constructors, and Assignment Operator  
The 4 automatics always created by the comiler for a class is the:  
1. Constructor (Automatic version is always default constructor)  
2. Destructor  
3. Copy Constructor  
4. Assignment Operator  
  
**Copy Constructor**  
*Is a constructor*, meaning no return type and same name  
**Copy constructors are called when:**   
1. An object is defined to have the value of another object of the same type    
* Ex. `Fraction f1, f2(3,4); Fraction f3 = f3;` (Uses copy constructor since initialization **AND** declaration are done at once, copy constructor is not used if `f1 = f2;` since declaration and initialization are separate)
2. An object is *passed by value* into a function  
3. An object is *returned (by value)* from a function  
Since Copy Constructors just copy the original to another object being created a copy constructor **always has 1 parameter which is the same type of the class itself, it is always passed by reference (pointer since a pass by value needs a copy constructor)**  
Format for copy constructor- `className(const className& c);`  
Const is not required but is usually a good idea   
*Shallow copy vs Deep copy*  
Default copy constructor created creates a shallow copy, meaning the object is copied exactly as is (sufficient for a lot but not all things)  
Deep copy is needed for when copying a memory address would point to the same object, usually in a dynamic array needed for a class that is not created within the class itself, just the pointer is in the class  
If a shallow copy is used for a class with dynamic array and a pointer within the class, when shallow copied both the original and copy would point to the same memory address of the dynamic array (meaning everything is a copy except the array which is the same as the original)  
Example Deep copy constructor:   
```  
Directory::Directory(const Directory& d) { // d is original 
		maxSize= d.maxSize;  
		currentSize = d.currentSize;    
		//Create new dynamic array for new object's pointer
		entryList = new Entry[d.maxSize];    
		// Creates loop to copy data from original to new
		for (int i = 0; i < currentSize; i++)   
				entryList[i] = d.entryList[i];
}  
```  
**Assignment Operator**  
Same as other operator overloads  
Ex. `Fraction f1, f2; f1 = f2;`  
Default assignment operator works like default copy constructor and makes a shallow copy  
*Differences of copy constructor and assignment operator:*  
1. Copy constructor initializes object as a copy of an existing one, while assignment operator sets an existing object to that of another existing object, in situations with dynamic allocation this means that old dynamic memory must be cleaned first before the copy is made  
2. An assignment operator also returns the value that was assigned (*copy constructor has no return*)   
* Ex. `a = b = c = 4;` c = 4 is first returning 4 to be assigned; then b = 4 returning 4 again to be assigned by a = 4  
**This keyword**  
From inside any member function the object has access to its own address by the keyword *this*, in an assignment operator you must return the object itself (this, which is a pointer itself)  
Like copy constructor assignment operators need one parameter, the parameter is the same as the copy constructor  
Declaration examples:   
* `Directory& operator= (const Directory&);`  
* `Fraction& operator= (const Fraction& f);  
Full example of assignment operator:  
```  
Directory& operator= (const Directory& d) {
		if (this != &d) { //Test to check if two objects are already equal   
				//Deallocate existing array of left object of equal sign  
				delete [] entryList;  
				//similar actions to Copy constructor   
				maxsize = d.maxsize;  
				currentsize = d.currentsize;  
				entrylist = new Entry[d.maxsize];  
				for (int i = 0; i < currentsize; i++) {
						entryList[i] = d.entryList[i];
				}  
		}  
		//Return object by reference
		return *this;  
}  
```  
### C-strings vs Strings as Objects  
C-Style Strings- an array of chars terminated by null space, need size of string + 1 for null character, character array is **only** a c-style string when ended with a null character  
<cstring> library includes:   
* strlen()  
* strcpy()  
* strcat()  
* strcmp()  
* strstr()  
* strtok()  
* strncpy()  
* strncat()  
* strncmp()    
Things about C-strings:  
* They are fast and efficient if user is going to enter a fixed size string  
* Person implementing c-strings must be aware of preventing them being called irresponsibly  
* String name is really just a pointer to start of the array  
* Must always be aware of the boundaries of the c-string, overflows are not automatically detected  
* Harder to work with, (Ex. `greeting = "Hellow World";` is not legal instead one needs `strcpy(greeting, "Hello World");`,  `if (str1 == str2)` actually **compares the pointers** so one needs `if (strcmp(str1, str2) == 0)`)  
* In order to use a more flexible c-style string one needs dynamic memory and using *new*, *delete*, and any resizing methods needed    
### Inheritance  
*Base class* is what a *derived class* is built off of, usually derived class **is a** base class    
Ex. A cricle "is a" geometric figure  
Base class of geometric\_figures with derived classes of circle and square, circle and square both can use base information from geometric\_figures but circle will have radius unique to it and square will have height and width unique to it  
To declare a derived class use initialization list:  
`class derivedClassName:public baseClassName`  
Public in the above causes the derivedClassName to be ublicly derived from baseClassName, meaning protection levels in the derived class are the same as the base class
Example:    
```  
class Vehicle{
...
};  
class Car:public Vehicle{
...  
}    
class Honda:public Car{
...
}
```  
In the above Honda inherits all of class Car which inherits all of Vehicle  
**Portection Levels**  
* Public: Any member that is public can be accesssed by name *anywhere*  
* Private: Any member that is private can be accessed directly *only* by the class in which it is declared  
* Protected: Any member that is protected can be accessed *directly by the class in which it is declared and by any classes that are derived from the class (but not from anywhere else)  
**Constructors in Derived Classes**  
**Order of Constructors Running with Inheritance**  
BaseClass -> Derived Class  
Ex. for Honda obj; going off above, Vehicle runs, then Car, then Honda  
When running destructors the reverse order is used, DerivedClass-> BaseClass (~Honda() -> ~Car() -> ~Vehicle)  
**Constructors with Parameters**  
`Honda h(2,3,4,"green","Accord");`  
To use the above with derived class:  
```    
Vehicle::Vehicle(int c, int id) {
		vehicleCategory = c;  
		idNumber = id;
}  
Car::Car(int c, int id, int nd, char* cc) : Vehicle(c, id) {
		numDoors = nd;  
		strcpy(carColor, cc);
}  
Honda::Honda(int c, int id, int nd, char*cc, char* mod) : Car(c, id, nd, cc) {
		strcpy(model, mod);
}
```    
In this case 2 and 3 could be sent as parameters to a 2 parameter constructor for Vehicle, followed by 4 and green for parameters in 2 parameter constructor for Car, lastly Accord would be sent to Honda constructor with one parameter  
**Function Overriding**  
Suppose we have the following base class:  
```  
class Student {
		public:  
			void GetReport();  
		... (other member data and functions)
};  
```  
The following are derived from Student:  
```  
class Grad:public Student //Student "is a" grad  
class Undergrad:public Student //Student "is a" undergrad    
```
Given the above one can create a student, grad, and undergrad object all with differnt uses:  
```  
Student s;  
Grad g;  
Undergrad u;  
s.GradeReport();  
g.GradeReport();  
u.GradeReport();  
```  
Each of the above can access GradeReport() but each call of the function can be written to act differently  
Due to inheritance one can have:  
```
void Student::GradeReport() {  
		... Processing done to get grade report ...
}  
void Grad::GradeReport() {
		Student::GradeReport();  
		... Other processing done to customize result to grad class approriate result ...
}  
```
This can be done to save on what needs to be written as Grad and Undergrad both need to access GradeReport() but both can be customize to each levels different grading scale  
**To call parent classes function one needs to use** `className::memberName`  
**Virtual Functions and Abstract Classes**  
A pointer to a base class type **can** be pointed at an object derived from that base class (a pointer in the base class can change and point to any of the derived classes types)    
```
Student s;  
Grad g;  
Undergrad u;  
Student *sp1, *sp2, *sp3; //All pointers of class type student  
  
sp1 = &s; //sp1 is now pointing at Student object  
sp2 = &g; //sp2 is now pointing at Grad object  
sp3 = &u; //sp3 is now pointing at Undergrad object  
```

All the above are type Student but sp1 points at the base class and the others point at the derived class  
**Heterogeneous List**  
An array of pointers pointing to a base class type, **but** can act more like pointers pointing at derived classes without breaking array type rules  
**Virtual Functions**  
*Binding*- As oppose to a function definition pointing only (staically) to a function, adding the keyword **virtual** can be made to where the function will determine the pointer type as an instance of the derived classes of the base class as long as the base class pointer is used and *virtual* is used, **use to override a base classes function when called through a pointer so that the call made by the pointer can change to call the needed function within a derived class of the base class**    

```
class Student{
		public:  
			virtual void GradeReport();  
		...
}

Student *sp1, *sp2, *sp3;    
Student s;  
Grad g;  
Undergrad u;  
sp1 = &s;  
sp2 = &g;  
sp3 = &u;  
sp1->GradeReport(); //Run Student's version  
sp2->GradeReport(); //Run Grad version  
sp3->GradeReport(); //Run Undergrad version  
  
for (int i = 0; i < size; i++) {
		list[i]->GradeReport();
}  
```

**Abstract Classes**    
An abstract class is one that has *at least one pure virtual function*   
A **pure virtual function** is a virtual function that will have no function declaration tied to the base class and will have no use of being called from the base class itself, **abstract classes cannot have an object built of that type**  
Syntax is: `virtual void GradeReport() = 0;` (only used if each GradeReport in following derived classes are fully created and GradeReport in the base class has no use)  
Pure Virtual Functions are mainly only used for inheritance hierarchies and are used to place data common to derived classes belonging to them, abstract classes can be used to build pointers which is useful when using virtual functions  
If you had a abstract class of Shape:  
`Shape s;` would be illegal since can't have an object of Shape since it is abstract  
`Shape *\sptr;` is *legal* since sptr is a pointer to a base class that **could** point to a derived class  
### Multiple Inheritance  
Java does not support multiple inheritance as well as other object oriented languages  
Example for Date:  
```
class Date {
		protected:  
			int day, month, year;
		public:   
			//Default parameter  
			Date() {day = 1; month = 1; year = 1900; }  
			//Constructor  
			Date (int d, int m, int y) {day = d; month = m; year = y; }  
			//Accessor   
			int GetDay() const {return day;}  
			int GetMonth() const {return month;}  
			int GetYear() const {return year;}  
};      
class Time {
		protected:  
			int hour, min, sec;  
		public:  
			//Default  
			Time() {hour = 0; min = 0; sec = 0;}  
			//Constructor  
			Time(int h, int m, int s) {hour = h; min = m; sec = s;}  
			//Accessor functions
			int GetHour() const {return hour;}
			int GetMin() const {return min;}
			int GetSec() const {return sec;}
};          
const int DT_SIZE = 20;
class DateTime : public Date, public Time {  
		protected:  
			char dateTimeString[DT_SIZE];  
		public:  
			//Default  
			DateTime();  
			//Constructor  
			DateTime(int, int, int, int, int, int);  
			//Accessor function  
			const char *GetDateTime() const {return dateTimeString;}
};  
//Default   
DateTime::DateTime() : Date(), Time() {
		strcpy(dateTimeString, "1/1/1900 0:0:0");
}  
//Constructor  
DateTime::DateTime(int dy, int mon, int yr, int hr, int mt, int sc) : Date(dy, mon, yr), Time(hr, mt, sc) {
   char temp[TEMP_SIZE]; // Temporary work area for itoa()

   // Store the date in dateTimeString, in the form MM/DD/YY
   strcpy(dateTimeString, itoa(getMonth(), temp, TEMP_SIZE));
   strcat(dateTimeString, "/");
   strcat(dateTimeString, itoa(getDay(), temp, TEMP_SIZE));
   strcat(dateTimeString, "/");
   strcat(dateTimeString, itoa(getYear(), temp, TEMP_SIZE));
   strcat(dateTimeString, " ");

   // Store the time in dateTimeString, in the form HH:MM:SS
   strcat(dateTimeString, itoa(getHour(), temp, TEMP_SIZE));
   strcat(dateTimeString, ":");
   strcat(dateTimeString, itoa(getMin(), temp, TEMP_SIZE));
   strcat(dateTimeString, ":");
   strcat(dateTimeString, itoa(getSec(), temp, TEMP_SIZE));
}
```
### Bitwise Operators  
Byte is 8 bits, smallest unit of **directly accessible** storage  
Bitwise operators are used to access and change the bits that make up a value  
&(and) has two parameters and returns a 1 if both bits are 1 and a 0 if otherwise  
|(or) has two parameters and returns a 1 if either value is a 1 or both are 1 and a 0 otherwise  
^(xor) has two parameters and returns a 1 if and only if there is only one 1 and a 0 otherwise    
<<(shift left) has two parameters and is a bitshift to the left an amount of bits  
\>\>(shift right) has two parameters and is a bitshift to the right an amount of bits  
~(complement) has one parameter and returns 1 if there is a 0 and a 0 when there is a 1  
Include Bitset library for flip() and set()  
flip takes in 1 parameter for index of bit to flip or no parameter for flip the whole bitset  
Set takes in up to two parameters, no parameters sets all bitset to 1s, first parameter is index of bit to set to 1, second defaults to 1 or true and is to set the value at the index to that value

[Top of test 2 review](#test-2-review)        

# 12/09/19
## Test 3 Review 
[Test 1 Review](#test-1-review)  
[Test 2 Review](#test-2-review)  
### Template Classes and Functions  
Used to capture multiple similar data types, often in functions to perform an specific algorithim on the data  
In an example of finding max of numbers instead of explicitly working with only int and similar, automatically converted datatypes   
Using `template < class nameOfType >` one can have functions return nameOfType like `nameOfType maximum (nameOfType v1, nameOfType v2, nameOfType v3) { ... function ... }`  
A function like this can be used to consistently print an array of varying data types (example can't work unless << is overloaded):   
```  
template< class T >  
void printArray (const T *array, const int count) {
		for (int i = 0, i < count; i++)   
				cout << array[i] << " ";  
		cout << endl;
}  
```  
For dealing with custom data types in classes in the header file one needs `typedef int myType;`, can then be used later in the class for member function return types  
*typedef* is used to be a synonym to replace int in the above example, one can use template to define a return type more like a function template  
`template< class T>` makes T a potential data type to return from member functions in header  
To use a class that has a custom data type defined by template to call the member function for definition one needs to create and call it like:  
`template< class T >`  
`List< T >::~List() { ... function ... }`  
**Template <class name>** needs to be used before any function in function definition and class declarations it is used in  
To use these data types in main one needs **ClassName< builtInDataType > variableName;**  
If template is used in the class but functions are defined in another cpp file the cpp file **must be #included** or **function definitions need to be written in header file**  
  
## Intro to Data Structures  
*Abstract Data Types*- many different ways to store data as opposed to just structs and arrays, these data types describe how the data is stored themselves as well as ways of accessing their data within consistently, examples are *stacks*, *queues*, *vectors*, *linked lists*, *trees*, *hash tables*, *sets*, and others  
### Stacks  
* First in last out (FILO), only removes and adds from the top position  
* Ex. stack of lunch trays, grab top one but put new ones on top of stack  
* Uses *push* (adds to top) and *pop* (removes top of the stack)  
* Used in compilers, operating systems, handling of program memory  
### Queues  
* First in first out (FIFO), inserts at the end and removes from the front  
* Ex. waiting in a line at a park  
* Uses *enqueue* (adds item into the queue) and *dequeue* (removes item at back of queue)  
* Used in print job scheduling, process scheduling  
### Vector  
* Stores item of a similar data type based on storage in an array  
* By encapsulating array into a class one can use:  
	* Dynamic allocation to allow internal array to be flexible in size  
	* Handle boundary issues (error-checking if index is known)  
* Pros: quick retrieval of data if index is known  
* Cons: Inserts and deletes are slow since many elements must shift  
### Linked List  
* Collection of data with pointers all pointing to the next item in the list  
* Usually data of the same types  
* Uses *nodes* that are **self-referential** (contains pointer to another item of the same data type)  
* Each node contains the *data it stores* and *a pointer to the next node*  
* Nodes can be anywhere in memory due to the pointers it stores pointing to following nodes  
* Can *generally* grow to any size   
* Alternative to array based storage  
* Pro: Insert and deletes fast, creates new node and changes pointers  
* Cons: No random access meaning locating an element requires stepping through each element  
  
**Vector and Linked Lists are opposites of each other**    
**Stacks and Queues can be implemented with Vectors and Linked List**  
**A stack can use a link list but this will limit push and pop usage**  

### Tree  
* Non-linear collection of data elements linked with pointers (like a linked list)  
* Has self-referential nodes, but each node points to two or more other nodes  
* Example: *binary tree*  
	* Each node contains data elements and two pointers pointing to more nodes  
	* Very useful for fast searching, assuming data is kept in some kind of order  
	* **Binary Search**- finds a path through the tree starting at the root (half-way point, tip of the tree) and searches left and right  
  
## Recursion  
*Recursive function*- calls itself  
Many functions can be done recursively or through iteration (looping through)  
When using iteration make sure there is a "way out", same when using recursion, ensure a way out (if infinite recursion occurs, activation records will eventually overrun the stack  
Examples with Factorials:  
Iterative:  
```  
unsigned long Factorial(int n) {
		unsigned long f = 1;  
		for (int i = n; i >= 1; i--)  
				for *= i;  
		return f;
}  
```  
Recursion:   
```  
unsigned long Factorial(int n) {
		if (n <= 1)  
				return 1;  
		else  
				return (n * Factorial(n - 1));
}  
```  
Recursion usually has extra overhead as when each function is called an activation record is created resulting in more memory used  
**Which is better?**  
In regards to Fibonnaci iteration is better despite Fibonnaci being recursive itself  
Iterative:   
```  
int Fib(int n) {
		int n1 = 1, n2 = 1, n3;  
		int i = 2;  
		while (i < n) {
				n3 = n1 + n2;  
				n1 = n2;  
				n2 = n3;  
				i++
		}  
		return n2;
}  
```  
Recursive:     
```
int Fib(int n) {
		if (n <= 0)			return 0;  
		else if (n == 1) 	return 1;  
		else   
				return Fib(n-1) + Fib(n - 2);
}  
```  
Recursive method creates many activation records with multiple calls, thus greatly slower with higher numbers  
Recursion is used with **Quicksort** and **Merge Sort**, usually these are considered the fastest  
  
### Exception Handling  
Exceptions- Raised on runtime, any error the program runs into to cause an abrupt stop  
**Exception Handler**- piece of code that resolves an exception situation  
Intended to be separate from main tasks  
It is needed due to many possible problems that are infrequent improving modifiability, improves fault tolerance, but helps makes programs hard to read or debug  
Shouldn't be used in **all** cases, traditional error checking is better sometimes, best for **infrequent errors**  
**User input should not use exception handling, but errors resulting in program termination can be**  
Good for use in teams or with multiple modules  
Uses reserved words of **try**, **throw**, and **catch**  
**Try**:  
* In curly braces is code that might cause an exception to be raised  
**Catch**  
* 1 or more catch blocks follow a try   
* Each block of catch is an **exception handler**  
* Catch block has a single parameter  
**Throw**    
Throw is used to identify exception to be caught
When an exception occurs it is referred to as the **throw point**    

If an exception occurs in a try block:  
1. Try block ends  
2. Program attempts to match exception to one of the catch handlers  
3. If a match is found the matching catch block is executed (**only one catch block can be executed**)  
4. program resumes after last catch block  
  
General library for exception handling is the exception library  
Example code:  
```  
int main() {
		int cookies, people;      
		double cpp;
  
		try {
				cout << "Enter number of people: ";  
				cin >> people;  
				cout << "Enter number of cookies: ";  
				cin >> cookies;  
				if (cookies == 0)  
						throw people;  
				else if (cookies < 0)  
						throw static_cast<double>(people);  
				cpp = cookies / static_cast<double>(people);  
				cout << cookies << " cookies.\n"  
						<< people << " people.\n"  
						<< "You have " << cpp << " cookies per person." << endl;
		}  
		catch (int e) {
				cout << e << " people, and no cookies!" << endl;
		}  
		catch (double t) {
				cout << "Second catch block activated." << endl;
		}  
		cout << "End of program" << endl;  
}
```  
In the above code if 0 i entered first catch is executed else if a negative number is entered the other catch is executed  
Try block is always executed and if a throw is detected catch is called, throw is used to look if a catch block matches, catch looks at if the data type is accurate and exception is raised  

[Top of test 3 review](#test-3-review)    
