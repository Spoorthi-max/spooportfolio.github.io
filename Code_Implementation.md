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
````
## 2. Linked List Implementation

### Description
- **Dynamic Stages**: Add or remove stages (nodes) as crops progress.
- **Traversal**: Track the entire journey.
- **Reverse Lookup**: Audit trails for backward tracking.
````
class Node:
       def __init__(self, location, timestamp, condition):
        self.location = location
        self.timestamp = timestamp
        self.condition = condition
        self.next = None

     class LinkedList:
        def __init__(self):
        self.head = None
        self.tail = None

       def add_stage(self, location, timestamp, condition):
        """Add a new stage to the linked list."""
        new_node = Node(location, timestamp, condition)
        if not self.head:
            self.head = self.tail = new_node
        else:
            self.tail.next = new_node
            self.tail = new_node

        def display_journey(self):
          """Display the entire journey."""
           current = self.head
           print("Crop Journey:")
            while current:
             print(f"Location: {current.location}, Time: {current.timestamp}, Condition: {current.condition}")
              current = current.next
  
         def remove_stage(self, location):
          """Remove a specific stage from the journey."""
          current = self.head
           prev = None
           while current:
             if current.location == location:
                 if prev:
                     prev.next = current.next
                 else:
                     self.head = current.next
                 if current == self.tail:
                     self.tail = prev
                  print(f"Removed stage: {location}")
                  return
              prev = current
             current = current.next
             print(f"Stage not found: {location}")
 
        def reverse_journey(self):
          """Reverse the linked list to show the journey in reverse order."""
         prev = None
          current = self.head
           while current:
              next_node = current.next
              current.next = prev
              prev = current
              current = next_node
            self.head = prev

          ## Example Usage
         journey = LinkedList()
 
        ## Add stages
       journey.add_stage("Farm", "2024-12-30 08:00", "Fresh")
       journey.add_stage("Warehouse", "2024-12-30 12:00", "Good")
        journey.add_stage("Inspection Center", "2024-12-30 14:00", "Checked")
         journey.add_stage("Market", "2024-12-30 18:00", "Delivered")

        ## Display the journey
       journey.display_journey()

        ## Remove a stage
       journey.remove_stage("Inspection Center")
 
       ## Display the journey after removal
       journey.display_journey()
 
        ## Reverse and display the journey
        journey.reverse_journey()
        journey.display_journey()
````
## Output

###  Display Journey
```
plaintext
Crop Journey:
Location: Farm, Time: 2024-12-30 08:00, Condition: Fresh
Location: Warehouse, Time: 2024-12-30 12:00, Condition: Good
Location: Inspection Center, Time: 2024-12-30 14:00, Condition: Checked
Location: Market, Time: 2024-12-30 18:00, Condition: Delivered
````
## Tree Algorithm Implementation

### Key Features

#### Hierarchical Representation
- Parent nodes represent sources (e.g., farms).
- Child nodes represent destinations (e.g., warehouses, markets).

#### Traversal
- **Depth-First Search (DFS)** for complete path exploration.
- **Breadth-First Search (BFS)** for level-by-level tracking.

#### Dynamic Updates
- Add or remove locations (nodes) dynamically.
  ````
  class TreeNode:
    def __init__(self, location, timestamp, condition):
        self.location = location
        self.timestamp = timestamp
        self.condition = condition
        self.children = []

    def add_child(self, child_node):
        self.children.append(child_node)

    def remove_child(self, location):
        self.children = [child for child in self.children if child.location != location]

  class CropTree:
    def __init__(self, root):
        self.root = root

    def dfs(self, node, path=[]):
        """Depth-First Search Traversal."""
        if not node:
            return
        path.append((node.location, node.timestamp, node.condition))
        for child in node.children:
            self.dfs(child, path)
        return path

    def bfs(self):
        """Breadth-First Search Traversal."""
        queue = [self.root]
        path = []
        while queue:
            current = queue.pop(0)
            path.append((current.location, current.timestamp, current.condition))
            queue.extend(current.children)
        return path

   # Example Usage
   # Create the root node
   farm = TreeNode("Farm", "2024-12-30 08:00", "Fresh")

   # Create the tree
   crop_tree = CropTree(farm)

   # Add child nodes
   warehouse = TreeNode("Warehouse", "2024-12-30 12:00", "Good")
   inspection_center = TreeNode("Inspection Center", "2024-12-30 14:00", "Checked")
   market1 = TreeNode("Market A", "2024-12-30 18:00", "Delivered")
   market2 = TreeNode("Market B", "2024-12-30 20:00", "Delivered")

  # Build the tree structure
   farm.add_child(warehouse)
   warehouse.add_child(inspection_center)
   inspection_center.add_child(market1)
   inspection_center.add_child(market2)

   # Perform DFS traversal
   print("DFS Traversal:")
  dfs_path = crop_tree.dfs(crop_tree.root)
   for location, timestamp, condition in dfs_path:
    print(f"Location: {location}, Time: {timestamp}, Condition: {condition}")

   # Perform BFS traversal
   print("\nBFS Traversal:")
   bfs_path = crop_tree.bfs()
   for location, timestamp, condition in bfs_path:
    print(f"Location: {location}, Time: {timestamp}, Condition: {condition}")
  ````
## Output
## DFS Traversal

```plaintext
Location: Farm, Time: 2024-12-30 08:00, Condition: Fresh
Location: Warehouse, Time: 2024-12-30 12:00, Condition: Good
Location: Inspection Center, Time: 2024-12-30 14:00, Condition: Checked
Location: Market A, Time: 2024-12-30 18:00, Condition: Delivered
Location: Market B, Time: 2024-12-30 20:00, Condition: Delivered
````
## BFS Traversal

```plaintext
Location: Farm, Time: 2024-12-30 08:00, Condition: Fresh
Location: Warehouse, Time: 2024-12-30 12:00, Condition: Good
Location: Inspection Center, Time: 2024-12-30 14:00, Condition: Checked
Location: Market A, Time: 2024-12-30 18:00, Condition: Delivered
Location: Market B, Time: 2024-12-30 20:00, Condition: Delivered
````

# Sorting Algorithms
## Bubble Sort
Sort the movement data by timestamp in ascending order.
```
def bubble_sort(crop_data):
    n = len(crop_data)
    for i in range(n):
        for j in range(0, n - i - 1):
            if crop_data[j]["timestamp"] > crop_data[j + 1]["timestamp"]:
                crop_data[j], crop_data[j + 1] = crop_data[j + 1], crop_data[j]

# Example Usage
crop_data = [
    {"location": "Market A", "timestamp": "2024-12-30 18:00", "condition": "Delivered"},
    {"location": "Farm", "timestamp": "2024-12-30 08:00", "condition": "Fresh"},
    {"location": "Warehouse", "timestamp": "2024-12-30 12:00", "condition": "Good"},
    {"location": "Inspection Center", "timestamp": "2024-12-30 14:00", "condition": "Checked"},
]

bubble_sort(crop_data)
````
## Output for Bubble Sort (by timestamp):
```plaintext
{'location': 'Farm', 'timestamp': '2024-12-30 08:00', 'condition': 'Fresh'}
{'location': 'Warehouse', 'timestamp': '2024-12-30 12:00', 'condition': 'Good'}
{'location': 'Inspection Center', 'timestamp': '2024-12-30 14:00', 'condition': 'Checked'}
{'location': 'Market A', 'timestamp': '2024-12-30 18:00', 'condition': 'Delivered'}

````
## Queue Management

- **manage_shipment_queue Function**: 
  - Sorts the shipments and adds them to a queue (deque) for processing.
  - The queue ensures first-in, first-out (FIFO) processing, ideal for handling shipments efficiently.

## Processing Shipments

      - **Main Function**: 
        - Demonstrates dequeuing shipments and processing them in the correct order.
       - This approach ensures that crops are tracked and moved systematically from farm to market.
       Added to queue: {'crop_name': 'Wheat', 'quantity': 500, 'farm_location': 'Farm A', 'destination': 'Market X'}
     Added to queue: {'crop_name': 'Corn', 'quantity': 300, 'farm_location': 'Farm B', 'destination': 'Market Y'}
    Added to queue: {'crop_name': 'Rice', 'quantity': 800, 'farm_location': 'Farm C', 'destination': 'Market Z'}
     Current queue:
    1. {'crop_name': 'Wheat', 'quantity': 500, 'farm_location': 'Farm A', 'destination': 'Market X'}
    2. {'crop_name': 'Corn', 'quantity': 300, 'farm_location': 'Farm B', 'destination': 'Market Y'}
      3. {'crop_name': 'Rice', 'quantity': 800, 'farm_location': 'Farm C', 'destination': 'Market Z'}
      Processed crop: {'crop_name': 'Wheat', 'quantity': 500, 'farm_location': 'Farm A', 'destination': 'Market X'}
      Processed crop: {'crop_name': 'Corn', 'quantity': 300, 'farm_location': 'Farm B', 'destination': 'Market Y'}
    Current queue:
    1. {'crop_name': 'Rice', 'quantity': 800, 'farm_location': 'Farm C', 'destination': 'Market Z'}

## Output:
    Added to queue: {'crop_name': 'Wheat', 'quantity': 500, 'farm_location': 'Farm A', 'destination': 'Market X'}
    Added to queue: {'crop_name': 'Corn', 'quantity': 300, 'farm_location': 'Farm B', 'destination': 'Market Y'}
    Added to queue: {'crop_name': 'Rice', 'quantity': 800, 'farm_location': 'Farm C', 'destination': 'Market Z'}

    Current queue:
     1. {'crop_name': 'Wheat', 'quantity': 500, 'farm_location': 'Farm A', 'destination': 'Market X'}
    2. {'crop_name': 'Corn', 'quantity': 300, 'farm_location': 'Farm B', 'destination': 'Market Y'}
    3. {'crop_name': 'Rice', 'quantity': 800, 'farm_location': 'Farm C', 'destination': 'Market Z'}

    Processed crop: {'crop_name': 'Wheat', 'quantity': 500, 'farm_location': 'Farm A', 'destination': 'Market X'}
     Processed crop: {'crop_name': 'Corn', 'quantity': 300, 'farm_location': 'Farm B', 'destination': 'Market Y'}

    Current queue:
    1. {'crop_name': 'Rice', 'quantity': 800, 'farm_location': 'Farm C', 'destination': 'Market Z'}







