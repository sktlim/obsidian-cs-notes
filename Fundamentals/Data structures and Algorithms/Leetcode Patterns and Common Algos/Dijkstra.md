```cpp
int n = numNodes;
vector<vector<pair<int,int>>> graph; // [startNode, endNode, weight]

priority_queue<pair<int,int>, vector<pair<int,int>>, greater()>;
vector<int> dist(n, INT_MAX);

pq.emplace(0, 0); // push distance 0, node 0 into pq

while (!pq.empty()){
	auto [d, u] = pq.top();
	pq.pop();

	if (d > dist[u]) continue;
	// if (u == endPoint) return; alt condition for early return

	for (auto [v, weight]: graph[u]){
		if (dist[u] + weight < dist[v]){
			dist[v] = dist[u] + weight;
			pq.emplace(dist[v], v);
		}
	}
}

return dist;
```
