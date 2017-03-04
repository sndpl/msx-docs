# Pascal Course (2)

 Let's start programming. This course will handle
 variables. One of the main differences between BASIC
 and PASCAL is that in PASCAL you need to declare every
 variable you use before using it. This is to tell the
 compiler that you are going to use this variable AND to
 tell the compiler how you are going to use it. That's right,
 you need not only tell the compiler that you are going to use
 a variable but also what kind of variable it is (integer,
 real, etc). An example (the variable declaration is started
 using the reserved word VAR):

   VAR
     counter : integer;
     value   : real;
     status  : boolean;

 First use the reserved word VAR. Then on the next lines state
 the name of the variable followed by a colon and it's type,
 follow by a semi colon. The only restriction for variable
 names is that they may not be a reserved word (like BEGIN,
 END, VAR, etc).

 These are the standard types:

 - boolean (can be TRUE or FALSE)
 - byte (no need to explain)
 - integer (16 bit value, also called word)
 - longint (32 bit value, also called longword)
 - real (a not integer value)
 - char (an ascii character)
 - string (multiple characters)
 - text (a file containing characters)

 These are all the standard types included in the modern
 PASCAL versions. The last two (string and text) where not
 included in the early versions of PASCAL, so check the
 documentation of your compiler before using them. You can
 also make your own variable types, but I will come to that in
 a later course.

 How to use the variables? Here are some examples:

   value:=2;
   character:='A';
   counter:=counter+1;

 Assigning a value to a variable is very simple. First name
 the variable you want to assign the value to, then place the
 'equals' sign (:=) and then name the value to assign. The
 last works just as in basic, the only restriction is that the
 type may not change, e.g. don't assign a real to an integer.

 Besides the normal operators (+, -, *, (), AND, OR, XOR,
 NOT), there are also 'functions' like CHR, ORD, ROUND, TRUNC,
 ABS, ARCTAN, COS, EXP, FRAC, INT, LN, SIN, SQR, SQRT, etc.
 Most of them you already know if you program in BASIC. If you
 don't know them, then read a book about PASCAL. All of these
 functions can be used, but again, DON'T CHANGE THE TYPE.

 Good to remember is that, when using integers, there are two
 special functions you can use when dividing. Use DIV to
 divide, and use MOD to get what's left after the division.

 That's it for this course. I will leave you with an
 assignment. I will always try to leave you with an
 assignment, because the only way of learning a computer
 language, is by programming in the language.

 Assignment:
 Write a program that asks for three prices of products. The
 program must then calculate the total price with tax, the
 total price without tax, the tax of each product and the
 total tax.

 Use Readln(variable) to read a variable from the keyboard,
 and use Writeln(variable) to write a variable to screen.

 Have fun and if you send your solution to me, I will ensure
 that your solution is published next time.

Jeroen 'Just program it' Smael