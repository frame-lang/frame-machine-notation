# Frame Machine Notation (FMN) 0.1.0

Frame's mission is to be the simplest, easiest and most intuitive way to specify and implement Finite State Machines (FSMs) - aka "Automata" - in any object-oriented language.

### See [Medium for articles about Frame!](https://medium.com/@mark.truluck)


FMN Syntax | Definition | Example | Gist
:---: | :---: | :---: | :---:
<span>#</span> | Controller | #LoggedOut |
-block- | Block Section | -machine- |
$ | Frame State | $Begin | 
@ | Frame Event | @ | 
@&#124;message&#124; or &#124;message&#124; | Frame event message selector | &#124;buttonClick&#124; |
&#124;e1&#124;e2&#124;e3&#124; | Multi-messge selector | &#124;200&#124;201&#124;202&#124; |
@&#124;>>&#124; | Frame start message | @&#124;>>&#124; | 
@&#124;<<&#124; | Frame stop message | @&#124;<<&#124; | 
@&#124;>&#124; | Frame enter state message | @&#124;>&#124; |
@&#124;<&#124; | Frame exit state message | @&#124;<&#124; |
@[param] or [param] | Frame Event parameter attribute | @[firstName] |
@^ or ^(expr) | Frame Event return attribute | ^("YES!") | 
^ | Return statement | ^ |
->> | Change state operator | ->> $Working | [Change State](https://gist.github.com/frame-lang/ebec407ea6956e1d9dd7d0c3aa6a0df1)
-> | Transition operator | -> $Working | [Transition](https://gist.github.com/frame-lang/47cb1e87715c38861a5b0ebbda5e3bce)


## General Syntax

```
// this is a comment
```

## Frame Event

Frame Events are essential to the notation and the implementation.  The notation assumes three mandatory fields for a FrameEvent - **(message, parameters, return value)**.  Frame Events have a special type symbol `@` that refers to the current event in scope (there is almost always only one).

The pseudocode for the implementation is:

```
 class FrameEvent {
 
   var _msg:String
   var _params:Object
   var _return:Object
   
   FrameEvent(msg:String, params:Object = null) {
      _msg = msg
      _params = params
   }
}
```

## Frame Controller

A controller is a Frame machine, or automaton, that contains blocks (or sections) that define the internal structure of the machine.  From an implementation standpoint, the reference architecture for implementing a Frame controller is defined as an object-oriented class, but this is for convieniece and not a requirement.

```
// FMN

#MyController
	-interface-
	-machine-
	-actions-
	-domain-
##  // optional

// Pseudocode implementation

class MyController {

	// -interface-
	// -machine-
	// -actions-
	// -domain-
}
```

## Interface Block

```
// FMN

-interface-                             // interface block declaration
        m1                              // no parameters or return value
        m2[param1 param2]               // typeless parameters
        m3[param1:string param2:int]    // typed parameters
        m4:boolean                      // typed return value
        m5[param4 param5:string]:int    // mixed parameters and return value
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

## Machine Block

```
// FMN

-machine-                             		// machine block declaration
	$Begin 					// $    -- token indicates state definition
		|>>| 				// |>>| -- start event selector
			-> $Working 		// ->   -- transition token
                        ^			// ^    -- return token
                
        $Working => $Default 			// =>   -- Dispatch operator to send event to $Default
		|>| enterWorking() ^		// |>| 	-- enter event selector
		|<| exitWorking() ^		// |<|  -- exit event selector
             
        $End					// $End state has no event handlers. 
                
        $Default				// $Default parent state or "mode"
		|<<| -> $End ^			// |<<|  -- stop event selector token

// Pseudocode Implementation
 
    	// -machine-
    
   	var _state(e:FrameEvent) = Begin	// initialize start state
	
	func Begin(e:FrameEvent) {		// $Begin
		if (e._msg == ">>") {		// |>>|
		    _transition(Working)	// -> $Working
		    return			// ^
		}
	}
    
	func Working(e:FrameEvent) {		// $Working
		if (e._msg == ">") {		// |>|
		    enterWorking()		// enterWorking()
		    return			// ^
		}
		if (e._msg == "<") {		// |<|
		    exitWorking()		// exitWorking()
		    return			// ^
		}

		Default(e)			// $Working => $Default
	}  

	func End(e:FrameEvent) {		// $End
	}  

	func Default(e:FrameEvent) {		// $Default
		if (e._msg == "<<") {		// |<<|
		    _transition(End)		// -> $End
		    return			// ^
		}	
	}  
	
	func _transition(newState:FrameState) {
		_state(new FrameEvent("<"))	// send exit event
		_state = newState		// change state
		_state(new FrameEvent(">"))	// send enter event
	}


```

## States and Event Handlers

```
// FMN

	$Working 
		|>>| startMachine() ^		// start machine event
		|>| enterState() ^		// enter state event
		|e1| processE1(@[x] @[y]) ^	// packed param passing
		|e2|[x y] processE2(x y) ^	// unpacked param passing
		|<| exitState() ^		// exit state event
		|<<| stopMachine() ^		// stop machine event
		

// Pseudocode Implementation

	func Working(e:FrameEvent) {		// $Working
		if (e._msg == ">>") {		// |>|
		    	startMachine()		// startMachine()
		    	return			// ^
		}
		if (e._msg == ">") {		// |>|
		    	enterState()		// enterState()
		    	return			// ^
		}
		if (e._msg == "e1") {		// |e1|
			processE1( 		// processE1(@[x] @[y])
				e._params['x']
			)	e._params['y']
		    	return			// ^
		}
		if (e._msg == "e2") {		// |e1|
			var x = e._params['x']	// [x y]
			var y = e._params['y']
			processE2(x,y)		// processE2(x y)
		    	return			// ^
		}
		if (e._msg == "<") {		// |<|
		    	exitState()		// exitState()
		    	return			// ^
		}
		if (e._msg == "<<") {		// |<<|
		    	stopMachine()		// stopMachine()
		    	return			// ^
		}
	} 
```

## Control Flow

### Boolean if-then-else conditional

<image src="https://raw.githubusercontent.com/frame-lang/frame-machine-notation/master/Screen%20Shot%202018-11-10%20at%2010.36.51%20AM.png" width="600px"/>
