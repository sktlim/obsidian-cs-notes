- Busy waiting introduces multiple problems
- One of the problems, a variant of the **priority inversion problem**, is as follows:
	- 2 processes, _H_ (High priority), _L_ (low priority)
	- Scheduling rules: _H_ is run whenever ready
	- Scenario: 
		- _L_ is in critical region, _H_ becomes ready
		- _H_ begins busy waiting, but _L_ is never scheduled when _H_ is ready
		- _L_ never leaves critical region
		- _H_ busy waits forever

**Solution:** Use blocking instead of busy waiting

