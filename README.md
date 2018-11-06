
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
->> | Change state operator | ->> $Working | [Change State](https://gist.github.com/frame-lang/ebec407ea6956e1d9dd7d0c3aa6a0df1)
-> | Transition operator | -> $Working | [Transition](https://gist.github.com/frame-lang/47cb1e87715c38861a5b0ebbda5e3bce)

## General Syntax

```
// this is a comment
```

## Controller

A controller is a Frame machine, or automaton, that contains blocks (or sections) that define the internal structure of the machine.  From an implementation standpoint, the reference architecture for implementing a Frame controller is defined as an object-oriented class, but this is for convieniece and not a requirement.

```
// FMN

#MyController

// Pseudocode implementation

class MyController {}
```

## Interface Block

```
// FMN

-interface-                             // interface block declaration
        m1                              // no parameters or return value
        m2[param1 param2]               // typeless parameters
        m3[param1:string param2:int]    // types parameters
        m4:boolean                      // typed return value
        m5[param4 param5:string]:int    // parameters and return value
        m6 @(|6m|)                      // message alias
        
// Pseudocode implementation

	func m1() { 
		var e = new FrameEvent("m1")
		_state(e) 
	}
  
	func m2(param1 param2) { 
		var params = { "param1":param1, "param2":param2 }
		var e = new FrameEvent("m2", params)
		_state(e) 
	}
 
   
	func m3(param1:string, param2:int) { 
		var params = { "param1":param1, "param2":param2 }
		var e = new FrameEvent("m3", params)
		_state(e) 
	}

  
	func m4():boolean { 
		var e = new FrameEvent("m2")
		_state(e) 
		return (boolean) e._return
	}

  
	func m5(param4, param5:string):int { 
		var params = { "param1":param1, "param2":param2 }
		var e = new FrameEvent("m2", params)
		_state(e) 
		return (int) e._return
	}

  
	func m6() { 
		var e = new FrameEvent("6m")
		_state(e) 
	}

  
```


