## DESCRIPTION AND CODE IMPLEMENTATIONS OF ALGORITHMS USED

## Graph Implementation for Crop Tracking
## Description::
### 1. Graph Representation
- Use an adjacency list to represent locations (nodes) and routes (edges).

### . Dijkstra's Algorithm
- To find the shortest route for efficient transportation.

### . DFS (Depth-First Search)
- To explore all possible routes and ensure coverage.
- import heapq

      class Graph:
      def __init__(self):
        self.graph = {}  # Adjacency list representation
    
        def add_location(self, location):
        if location not in self.graph:
            self.graph[location] = []
    
      def add_route(self, from_location, to_location, distance):
        self.graph[from_location].append((to_location, distance))
        self.graph[to_location].append((from_location, distance))  # Assuming bidirectional routes
    
        def dijkstra(self, start, end):
        # Min-heap for finding the shortest path
        min_heap = [(0, start)]  # (distance, node)
        distances = {node: float('inf') for node in self.graph}
        distances[start] = 0
        previous_nodes = {node: None for node in self.graph}
        
        while min_heap:
            current_distance, current_node = heapq.heappop(min_heap)
            
            # Skip processing if we've already found a shorter path
            if current_distance > distances[current_node]:
                continue
            
            for neighbor, weight in self.graph[current_node]:
                distance = current_distance + weight
                
                # Update the shortest distance if a better path is found
                if distance < distances[neighbor]:
                    distances[neighbor] = distance
                    previous_nodes[neighbor] = current_node
                    heapq.heappush(min_heap, (distance, neighbor))
        
        # Reconstruct the shortest path
        path, current = [], end
        while current:
            path.append(current)
            current = previous_nodes[current]
        
        return path[::-1], distances[end]
    
       def dfs(self, start, visited=None):
        if visited is None:
            visited = set()
        visited.add(start)
        print(f"Visited: {start}")
        for neighbor, _ in self.graph[start]:
            if neighbor not in visited:
                self.dfs(neighbor, visited)

       # Example Usage
      crop_graph = Graph()

      # Add locations
      locations = ['Farm', 'Warehouse', 'Inspection Center', 'Market']
      for loc in locations:
      crop_graph.add_location(loc)

      # Add routes with distances
      crop_graph.add_route('Farm', 'Warehouse', 10)
      crop_graph.add_route('Warehouse', 'Inspection Center', 5)
      crop_graph.add_route('Inspection Center', 'Market', 8)
      crop_graph.add_route('Warehouse', 'Market', 15)

      # Shortest path using Dijkstra's Algorithm
      print("\nDijkstra's Algorithm:")
      path, total_distance = crop_graph.dijkstra('Farm', 'Market')
      print(f"Shortest path: {path}, Total distance: {total_distance} km")

      # Explore all routes using DFS
      print("\nDFS Traversal:")
      crop_graph.dfs('Farm')
 ## Output for the Given Graph

### Dijkstra's Algorithm

**Input**: Start = Farm, End = Market.

**Output**:
- **Shortest Path**: Farm -> StorageB -> Market
- **Total Cost**: 10 (Farm to StorageB) + 5 (StorageB to Market) = 15
## DFS Traversal

**Input**: Start = Farm.

**Output**:
```plaintext
Visited: Farm
Visited: Warehouse
Visited: Inspection Center
Visited: Market



