Resource Acquisition is Initialization
- Binds <u>lifecycle of a resource</u> that must be acquired before use to <u>lifetime of object</u>
- Every resource requiring cleanup should be given to an object's constructor

## Example

```cpp
class FileHandler{
public:
	FileHandler(const std::string filename): file(filename){}

	~FileHandler(){
		if (file.is_open()){
			file.close();
		}
	}
	
private:
	std::ofstream file;
};

class SomethingUsingFileHandler{
public:
	// do stuff
private:
	FileHandler _file; // Member allocated but destructor never explicitly called; This is being handled by RAII
};

```