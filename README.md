#include <stdio.h>
#include <stdlib.h>
#include <limits.h>

#define V 6 // Number of vertices in the graph

// Function to find the vertex with the minimum distance value
int minDistance(int dist[], int visited[]) {
    int min = INT_MAX, min_index;
    for (int v = 0; v < V; v++) {
        if (visited[v] == 0 && dist[v] <= min) {
            min = dist[v];
            min_index = v;
        }
    }
    return min_index;
}

// Function to print the shortest path from source to the given vertex
void printPath(int parent[], int vertex) {
    if (parent[vertex] == -1) {
        printf("%d ", vertex);
        return;
    }
    printPath(parent, parent[vertex]);
    printf("%d ", vertex);
}

// Function to calculate and print the shortest paths using Dijkstra's Algorithm
void dijkstra(int graph[V][V], int source) {
    int dist[V]; // Array to store the distance from source to each vertex
    int visited[V]; // Array to keep track of visited vertices
    int parent[V]; // Array to store the parent of each vertex in the shortest path
  
    // Initialize distance, visited, and parent arrays
    for (int i = 0; i < V; i++) {
        dist[i] = INT_MAX;
        visited[i] = 0;
        parent[i] = -1;
    }
  
    // Distance of source vertex from itself is always 0
    dist[source] = 0;
  
    // Find shortest path for all vertices
    for (int count = 0; count < V - 1; count++) {
        int u = minDistance(dist, visited);
        visited[u] = 1;
  
        // Update dist value of the adjacent vertices of the chosen vertex
        for (int v = 0; v < V; v++) {
            if (!visited[v] && graph[u][v] && dist[u] != INT_MAX && dist[u] + graph[u][v] < dist[v]) {
                dist[v] = dist[u] + graph[u][v];
                parent[v] = u;
            }
        }
    }
  
    // Print the shortest paths from the source vertex to all other vertices
    for (int i = 0; i < V; i++) {
        printf("Shortest path from source %d to source %d: ", source, i);
        printPath(parent, i);
        printf("\n");
    }
}

// Main function
int main() {
    int graph[V][V] = {
        {0, 4, 0, 0, 0, 0},
        {4, 0, 8, 0, 0, 0},
        {0, 8, 0, 7, 0, 4},
        {0, 0, 7, 0, 9, 14},
        {0, 0, 0, 9, 0, 10},
        {0, 0, 4, 14, 10, 0}
    };

    int source = 0; // Source vertex
  
    dijkstra(graph, source);
  
    return 0;
}
