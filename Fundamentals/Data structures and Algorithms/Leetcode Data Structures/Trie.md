```cpp
class Trie {
public:
	Trie* next[26] = {};
    bool isword = false;
    
    Trie() {}
    
    void insert(string word) {
        Trie* node = this;
        for (auto c: word){
            c -= 'a';
            if (!node->next[c]){
                node->next[c] = new Trie();
            }
            node = node->next[c];
        }
        node->isword = true;
    }
    
    bool search(string word) {
        Trie* node = this;
        for (auto c: word){
            c -= 'a';
            if (!node->next[c]){
                return false;
            }
            node = node->next[c];
        }
        return node->isword;
    }
    
    bool startsWith(string prefix) {
        Trie* node = this;
        for (auto c: prefix){
            c -= 'a';
            if (!node->next[c]){
                return false;
            }
            node = node->next[c];
        }

        return true;
    }
};
```
