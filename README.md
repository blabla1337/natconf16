This library contains a number of proof of concepts for rexx.

It shows a way for rexx to respond to the operator stop command in
POCREXXS.

The POCREXXS contains optional 'debug' code (it will display the
eyecatcher of some of the control blocks it traverses). This shows a
method to optionally include (debug) code using the %INCLUDE compiler
directive. This will of course only work if the code is compiled.
To enable this the member POCDBGA should contain */ and the
POCDBGB member should contain /*.

The xxxxxA members should always be empty or contain */
The xxxxxB members should always be empty or contain /*

The principle is:
/*                     << starts comment section
/*%INCLUDE POCDBGA */  << include POCDBGA
                       << if empty the comment continues
*/                     OR if */ the comment ends and code becomes active
some code              << some debug code
IF 0 = 1 THEN          << 'some code' will replace the normal code
/*%INCLUDE POCDBGB */  << include POCDBGB
                       << should be empty if POCDBGA is empty
/*                     OR should contain '/*' if POCDBGA has '*/'
*/                     << ends comment section
This way different types of debug code can be enabled or disabled
easily by having 2 %INCLUDE members that surround the code. This is
of course not limited to debug code, it could be used in any situation
where we want to have an easy option to include or exclude parts of the
(compiled) code.
The same principle applies to the CPU measurement code (POCCPUA and
POCCPUB).

!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
Make sure you keep the compile listing around, otherwise you probably
will have a hard time figuring out what line the SIGL will point at
if something goes wrong.
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

For the other code (the DB2 and Socket code) we use it the other way
around. It is included by default, but surrounded by an %INCLUDE so that
if we add */ to the POCDB2A or POCSOCA member and /* to the POCDB2B or
POCSOCB member we can remove that code from the compiled rexx.

The DB2 Purexml code uses the example EMP and EMPPROJACT tables. It
shows a way to use a correlated subquery to generate an xml which shows
all the employees from the EMP table and per employee all the projects
from the EMPPROJACT table that they are working on.

The socket code can be used to send the XML to a webserver. It is setup
as a POST request against httpbin.org/post which will return the posted
data embedded in a json format.
Alternatively, you can send it to a netcat running somewhere:
nc -l 7756
where server would be the ip address of the server you are running the
netcat on and port would be 7756 or whatever value you specify after
the -l.
