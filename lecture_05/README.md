# Advanced Graph Serch: Uniform Cost Search and A*

## [/Algos on Shortest Path with Dijkstra’s Algorithm](https://bradfieldcs.com/algos/graphs/dijkstras-algorithm/)

```
traceroute google.com
traceroute to google.com (172.217.5.110), 64 hops max, 52 byte packets
 1  testwifi.here (192.168.86.1)  2.355 ms  1.175 ms  1.259 ms
 2  192.168.0.1 (192.168.0.1)  1.647 ms  1.822 ms  3.391 ms
 3  * * *
 4  174.127.183.40 (174.127.183.40)  17.683 ms  14.209 ms  12.575 ms
 5  cr1-200p-b-be-5.as11404.net (192.175.29.48)  13.367 ms  18.483 ms  11.169 ms
 6  cr1-9greatoaks-hu-0-6-0-21-0.bb.as11404.net (192.175.28.120)  19.779 ms  18.156 ms  18.488 ms
 7  72.14.222.146 (72.14.222.146)  14.943 ms  23.223 ms  14.615 ms
 8  108.170.242.225 (108.170.242.225)  15.937 ms *  18.706 ms
 9  108.170.236.63 (108.170.236.63)  22.641 ms  18.721 ms  14.026 ms
10  sfo03s07-in-f110.1e100.net (172.217.5.110)  18.616 ms  19.223 ms  18.742 ms
```

Network of routers between you and network services like Google can be represented by a weighted graph.

### Dijkstra's Algorithm

Helps determine the shortest path from one particular starting node to all other nodes in the graph.

*Distances dictionary* used to keep track of the total cost from the start node.
* Initialized to zero for the start vertex and infinity for the other verticies.
* The algorithm will continue to update these values until there is a shortest path.
* At the end the distances directory will be returned.
* Algorithm iterates with each vertex, iterated via a priority queue.

```python
import heapq


def calculate_distances(graph, starting_vertex):
    distances = {vertex: float('infinity') for vertex in graph}
    distances[starting_vertex] = 0

    entry_lookup = {}
    pq = []

    for vertex, distance in distances.items():
        entry = [distance, vertex]
        entry_lookup[vertex] = entry
        heapq.heappush(pq, entry)

    while len(pq) > 0:
        current_distance, current_vertex = heapq.heappop(pq)

        for neighbor, neighbor_distance in graph[current_vertex].items():
            distance = distances[current_vertex] + neighbor_distance
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                entry_lookup[neighbor][0] = distance

    return distances


example_graph = {
    'U': {'V': 2, 'W': 5, 'X': 1},
    'V': {'U': 2, 'X': 2, 'W': 3},
    'W': {'V': 3, 'U': 5, 'X': 3, 'Y': 1, 'Z': 5},
    'X': {'U': 1, 'V': 2, 'W': 3, 'Y': 1},
    'Y': {'X': 1, 'W': 1, 'Z': 1},
    'Z': {'W': 5, 'Y': 1},
}
# calculate_distances(example_graph, 'X')
# => {'U': 1, 'W': 2, 'V': 2, 'Y': 1, 'X': 0, 'Z': 2}
```

### [Graph Data Structure 4. Dijkstra’s Shortest Path Algorithmj](https://www.youtube.com/watch?v=pVfj6mxhdMw)

Greedy algorithm: make locally optimal choices at each step in hopes that it helps us find the globally optimzed solution (not always efficient)

Algorithm
* Let distance of start vertex from start vertex = 0
Let distance of all other vertices from start = infinity
* Repeat
 * Visit the unvisited vertex with the smallest known distance from the start vertex
 * For the current vertex: examine its unvisited neighbors
 * For the current vertex: calculate distance of each neighbour from start vertex
 * If the calculated distance of a vertex is less than the known distance, update the shortest distance
 * Update the previous vertex for each of the updated distances
 * Add the current vertex to the list of veisited vertices
* Until all vertices visited

```
// pseudocode
Let distance of start vertex from start vertex = 0
Let distance of all other vertices from start = infinity

WHILE verticies remain unvisited
	Visit unviisted vertex with smallest known distance from start vertex / current vertex
	FOR each unvisited neighbor
		Calculate the distance from the start vertex
		IF the caculated distance of this certex is less than the known distance
			Update shortest distance to this vertex
			Update the previous vertex with the current vertex
		END
	NEXT unvisited neighborhor
	Add the current vertex to the list of visited vertices
END
```

### [Dijkstra's Algorithm on Youtube](https://www.youtube.com/watch?v=gdmfOwyQlcI)

Assign to every node a tentative distance value: set it to zero for our initial node and to infinity for all nodes.

Keep a set of visited nodes. The set starts with just the initial node.

For a current node, consider all of its unvisited neighbors and calculate (distance to the current node) + (distance from current node to the neighbor). If this is less than their current tentative distance, replace it with its new value.

### [Dijkstra's Algorithm - Computerphile](https://www.youtube.com/watch?v=GazC3A4OQTE)

