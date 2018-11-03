
# Frame Machine Notation (FMN) 2.0


FMN Syntax | Definition | Example 
:---: | :---: | :---:
<span>#</span> | Controller | #LoggedOut 
-block- | Block Section | -machine-
$ | Frame State | $Begin
@ | Frame Event | @
@&#124;message&#124; or &#124;message&#124; | Event Message Selector | &#124;buttonClick&#124;
@[param] or [param] | Event parameter attribute | @[firstName]
@^ or ^(expr) | Event return attribute | ^("YES!")
->> | Change state operator | ->> $Working
-> | Transition operator | -> $Working





