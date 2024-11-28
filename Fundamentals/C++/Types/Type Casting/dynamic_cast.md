> Mainly for converting <u>base class</u> to <u>derived class</u> pointers (downcasting)

## Usage
- check for `nullptr` result before use to ensure cast succeed

## Note
- Can also be done with `static_cast`, main diff is that `static_cast` does no runtime check to ensure that cast makes sense
- virtual functions should be preferred over downcasting, but downcasting makes sense when:
	- cannot change base class
	- need access to something derived class specific