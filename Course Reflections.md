/*
** Algorithm Overview **

1. Kinds of Problems in Nature
*/

// Iteration Example: Factorial Calculation
long long factorial(int n) {
    long long result = 1;
    for (int i = 1; i <= n; ++i) {
        result *= i;
    }
    return result;
}

// Recursion Example: Fibonacci Series
int fibonacci(int n) {
    if (n <= 1) {
        return n;
    }
    return fibonacci(n - 1) + fibonacci(n - 2);
}

// Recursion Example: Tower of Hanoi
#include <stdio.h>
void towers(int n, char from, char to, char aux) {
    if (n == 1) {
        printf("Move disk 1 from %c to %c\n", from, to);
        return;
    }
    towers(n - 1, from, aux, to);
    printf("Move disk %d from %c to %c\n", n, from, to);
    towers(n - 1, aux, to, from);
}

int main() {
    int n;
    printf("Enter the number of Disks to be moved\n");
    scanf("%d", &n);
    towers(n, 'A', 'C', 'B');
    return 0;
}

// Backtracking Example: N-Queens Problem
#include <iostream>
using namespace std;

bool isSafe(int board[][10], int row, int col, int N) {
    for (int i = 0; i < row; i++) {
        if (board[i][col] == 1 || 
            (col - (row - i) >= 0 && board[i][col - (row - i)] == 1) || 
            (col + (row - i) < N && board[i][col + (row - i)] == 1)) {
            return false;
        }
    }
    return true;
}

bool solveNQueens(int board[][10], int row, int N) {
    if (row == N) {
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                cout << (board[i][j] ? "Q " : ". ");
            }
            cout << endl;
        }
        cout << endl;
        return true;
    }
    
    bool res = false;
    for (int col = 0; col < N; col++) {
        if (isSafe(board, row, col, N)) {
            board[row][col] = 1;
            res = solveNQueens(board, row + 1, N) || res;
            board[row][col] = 0;
        }
    }
    return res;
}

void solveNQueens(int N) {
    int board[10][10] = {0};
    if (!solveNQueens(board, 0, N)) {
        cout << "Solution does not exist!" << endl;
    }
}

int main() {
    int N;
    cout << "Enter the value of N: ";
    cin >> N;
    solveNQueens(N);
    return 0;
}

/*
** 2. Space and Time Efficiency **

- Space Efficiency: Recursive Fibonacci is memory-intensive due to stack calls.
- Time Efficiency: Use asymptotic analysis to optimize algorithm performance.

Classes of Problems:
- P: Solvable in polynomial time (e.g., sorting).
- NP: Solutions verifiable in polynomial time.
- NP-Hard/NP-Complete: No known polynomial-time solutions.
*/

// Example: Merge Sort Algorithm
void merge(int arr[], int l, int m, int r) {
    int n1 = m - l + 1;
    int n2 = r - m;

    int L[n1], R[n2];

    for (int i = 0; i < n1; i++)
        L[i] = arr[l + i];
    for (int j = 0; j < n2; j++)
        R[j] = arr[m + 1 + j];

    int i = 0, j = 0, k = l;
    while (i < n1 && j < n2) {
        arr[k++] = (L[i] <= R[j]) ? L[i++] : R[j++];
    }

    while (i < n1)
        arr[k++] = L[i++];
    while (j < n2)
        arr[k++] = R[j++];
}

void mergeSort(int arr[], int l, int r) {
    if (l < r) {
        int m = l + (r - l) / 2;

        mergeSort(arr, l, m);
        mergeSort(arr, m + 1, r);
        merge(arr, l, m, r);
    }
}

/*
** 3. Graph Algorithms **
*/

// Dijkstra's Algorithm
#include <iostream>
#include <vector>
#include <climits>
using namespace std;

void dijkstra(vector<vector<int>>& graph, int src) {
    int V = graph.size();
    vector<int> dist(V, INT_MAX);
    vector<bool> sptSet(V, false);

    dist[src] = 0;
    for (int count = 0; count < V - 1; count++) {
        int u = -1;

        for (int i = 0; i < V; i++)
            if (!sptSet[i] && (u == -1 || dist[i] < dist[u]))
                u = i;

        sptSet[u] = true;

        for (int v = 0; v < V; v++)
            if (!sptSet[v] && graph[u][v] && dist[u] != INT_MAX && dist[u] + graph[u][v] < dist[v])
                dist[v] = dist[u] + graph[u][v];
    }

    for (int i = 0; i < V; i++)
        cout << "Vertex " << i << " -> Distance from source: " << dist[i] << endl;
}

int main() {
    vector<vector<int>> graph = {
        {0, 10, 0, 0, 0},
        {0, 0, 20, 5, 0},
        {0, 0, 0, 0, 1},
        {0, 0, 0, 0, 2},
        {0, 0, 0, 0, 0}
    };

    dijkstra(graph, 0);
    return 0;
}
