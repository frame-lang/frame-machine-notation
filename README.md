
# Frame Machine Notation (FMN) 0.1


FMN Syntax | Definition | Example | Gist
:---: | :---: | :---: | :---:
<span>#</span> | Controller | #LoggedOut |
-block- | Block Section | -machine- |
$ | Frame State | $Begin | 
@ | Frame Event | @ | 
@&#124;message&#124; or &#124;message&#124; | Frame Event Message Selector | &#124;buttonClick&#124;
@[param] or [param] | Frame Event parameter attribute | @[firstName] |
@^ or ^(expr) | Frame Event return attribute | ^("YES!") | 
^ | Return statement | ^ |
->> | Change state operator | ->> $Working |
-> | Transition operator | -> $Working | [Transition](https://gist.github.com/frame-lang/47cb1e87715c38861a5b0ebbda5e3bce)







