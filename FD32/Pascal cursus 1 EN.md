# Pascal (1)

Koen asked me to write a course on PASCAL. And because I
think PASCAL is a well structured and nice language I
agreed.

 Before I begin the real work (the course), I will do a short
 introduction. PASCAL (the language) is given the name of
 Blaise Pascal the mathematician  who lived from 1623 till
 1662. He was one of the great mathematicians that lived. He
 laid down the basics for calculating changes. One of his
 'hobbies' was trying to create machines that did mathematics.
 He created a machine that could do various calculations (not
 only adding and subtracting) that was entirely mechanical
 (try to imagine this).

 In about 1970 The name PASCAL (in capitals) was
 'reintroduced'. Professor Niklaus Wirth though it nice to
 name his new programming language to the great mathematician.
 I think most people have heard or read about PASCAL. PASCAL
 has a lot of advantages compared to BASIC. PASCAL is almost
 self documenting because of its structure.

 And now for the real thing. Lets begin with the structure of
 a PASCAL program:

 PROGRAM program_name (INPUT,OUTPUT);

   CONST
     {Constant definition}

   TYPE
     {Type definition}

   VAR
     {Variable declaration}

   {Procedure and function definition}

   BEGIN
     {Main program}
   END.

 All words in capitals are reserved words. This means that
 this name cannot be used for something else (a variable for
 instance), they have a special function in a program. PROGRAM
 indicates that where about to make a PASCAL program. It is
 followed by the name of the program and the words INPUT and
 OUTPUT between brackets. The words INPUT and OUTPUT tell the
 compiler that the program uses standard in- and output
 (keyboard, screen). When working with files we will include
 more between the brackets.

 After the program heading a block with constant, type and
 variable declaration follows. PASCAL forces you to declare
 all variables that you use in the program in advance. This
 not as in BASIC where you can declare a variable when you
 need it.

 After this the procedure and function declarations follow.
 These declarations are equal to the 'program' declaration.
 In a later course we will handle this.

 The last thing is a PASCAL program is the main program. The
 main program is enclosed by BEGIN and END.

 That's it for the first course. I will leave you with a small
 and simple PASCAL program. Try to compile and run it and see
 if you can understand it.

```
 PROGRAM simple (INPUT,OUTPUT);

   VAR
     Value_1 : integer;
     Value_2 : integer;
     Result  : integer;

   PROCEDURE input_value(VAR value:integer);

     BEGIN          (* input_value *)
       Write('Input a value : ');
       Readln(value);
       Writeln;
     END;           (* input_value *)

   BEGIN            (* mainprogram *)
     Writeln('Example program for PASCAL-course (1)');
     Writeln('FutureDisk');
     Writeln;
     Writeln('Made by :');
     Writeln('Jeroen Smael');
     Writeln;
     Writeln;
     input_value(Value_1);
     input_value(Value_2);
     Result:=Value_1+Value_2;
     Writeln(Value_1:1,' + ',Value_2:1,' = ',Result:1);
     Writeln;
   END.             (* mainprogram *)
```

                                  Jeroen 'PASCAL is fun' Smael