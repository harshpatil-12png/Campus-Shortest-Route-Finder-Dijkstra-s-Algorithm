# Campus-Shortest-Route-Finder-Dijkstra-s-Algorithm
#include <stdio.h>
#include <string.h>

#define MAX 20
#define INF 99999

int graph[MAX][MAX];
char location[MAX][50];
int n = 0;
int source = -1;

int distance[MAX];
int visited[MAX];
int parent[MAX];

void enterGraph();
void displayGraph();
void selectSource();
void dijkstra();
void displayPaths();
void displayDistances();
int findMinDistance();
void printPath(int v);

int main()
{
    int choice;
    do
    {
        printf("\n========================================\n");
        printf("\n-----CAMPUS SHORTEST ROUTE FINDER-----\n");
        printf("\n-----------DIJKSTRA'S ALGORITHM-----------\n");
        printf("========================================\n");
        printf("1. Enter Campus Graph\n");
        printf("2. Display Adjacency Matrix\n");
        printf("3. Select Source Location\n");
        printf("4. Find Shortest Distance\n");
        printf("5. Display Shortest Paths\n");
        printf("6. Display Distance from Source\n");
        printf("7. Exit\n");
        printf("----------------------------------------\n");

        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch(choice)
        {
            case 1:
                enterGraph();
                break;

            case 2:
                displayGraph();
                break;

            case 3:
                selectSource();
                break;

            case 4:
                dijkstra();
                break;

            case 5:
                displayPaths();
                break;

            case 6:
                displayDistances();
                break;

            case 7:
                printf("\nProgram exited successfully.\n");
                break;

            default:
                printf("\nInvalid choice! Please enter 1 to 7.\n");
        }
    }
 while(choice != 7);
    return 0;
}

/* Enter campus graph */
void enterGraph()
{
    int  i, j, distanceValue;
    printf("\nEnter number of locations: ");
    scanf("%d", &n);
    if(n <= 0 || n > MAX)
    {
        printf("Invalid number of locations!\n");
        n = 0;
        return;
    }
    printf("\nEnter location names:\n");
    for(i = 0; i < n; i++)
    {
        printf("Location %d: ", i + 1);
        scanf(" %[^\n]", location[i]);
    }

    /*
       The campus roads are treated as undirected.
       Enter the distance only once for each pair.
    */

    for(i = 0; i < n; i++)
    {
        graph[i][i] = 0;
        for(j = i + 1; j < n; j++)
        {
            printf("\nDistance between %s and %s: ",
                   location[i], location[j]);

            scanf("%d", &distanceValue);
            if(distanceValue < 0)
            {
                printf("Distance cannot be negative. Try again.\n");
                j--;
            }
            else if(distanceValue == 0)
            {
                graph[i][j] = INF;
                graph[j][i] = INF;
            }
            else
            {
                graph[i][j] = distanceValue;
                graph[j][i] = distanceValue;
            }
        }
    }
    source = -1;
    printf("\nCampus graph entered successfully!\n");
}

/* Display adjacency matrix */
void displayGraph()
{
    int i, j
    if(n == 0)
    {
        printf("\nPlease enter the campus graph first.\n");
        return;
    }
    printf("\n============= ADJACENCY MATRIX =============\n\n");
    printf("%-22s", "");
    for(i = 0; i < n; i++)
    {
        printf("%-18s", location[i]);
    }
    printf("\n");
    for(i = 0; i < n; i++)
    {
        printf("%-22s", location[i]);
        for(j = 0; j < n; j++)
        {
            if(graph[i][j] == INF)
                printf("%-18s", "INF");
            else
                printf("%-18d", graph[i][j]);
        }
        printf("\n");
    }
}

/* Select source location */
void selectSource()
{
    int i, choice;
    if(n == 0)
    {
        printf("\nPlease enter the campus graph first.\n");
        return;
    }
    printf("\n============= LOCATIONS =============\n");

    for(i = 0; i < n; i++)
    {
        printf("%d. %s\n", i + 1, location[i]);
    }
    printf("\nEnter source location number: ");
    scanf("%d", &choice);
    if(choice < 1 || choice > n)
    {
        printf("Invalid location!\n");
        source = -1;
        return;
    }
    source = choice - 1;
    printf("\nSource selected: %s\n", location[source]);
}

/* Find vertex having minimum distance */
int findMinDistance()
{
    int min = INF;
    int index = -1;
    int i;
    for(i = 0; i < n; i++)
    {
        if(visited[i] == 0 && distance[i] < min)
        {
            min = distance[i];
            index = i;
        }
    }
    return index;
}

/* Dijkstra's Algorithm */
void dijkstra()
{
    int i, j, u;
    if(n == 0)
    {
        printf("\nPlease enter the campus graph first.\n");
        return;
    }
    if(source == -1)
    {
        printf("\nPlease select a source location first.\n");
        return;
    }

    /* Initialize arrays */

    for(i = 0; i < n; i++)
    {
        distance[i] = INF;
        visited[i] = 0;
        parent[i] = -1;
    }

    distance[source] = 0;

    /* Find shortest distances */
    for(i = 0; i < n - 1; i++)
    {
        u = findMinDistance();
        if(u == -1)
            break;
        visited[u] = 1;

        for(j = 0; j < n; j++)
        {
            if(visited[j] == 0 &&
               graph[u][j] != INF &&
               distance[u] != INF &&
               distance[u] + graph[u][j] < distance[j])
            {
                distance[j] = distance[u] + graph[u][j];
                parent[j] = u;
            }
        }
    }
    printf("\nShortest distances calculated successfully!\n");
}

/* Print shortest path */
void printPath(int v)
{
    if(parent[v] == -1)
    {
        printf("%s", location[v]);
        return;
    }
    printPath(parent[v]);
    printf(" -> %s", location[v]);
}

/* Display shortest paths */
void displayPaths()
{
    int i;
    if(n == 0)
    {
        printf("\nPlease enter the campus graph first.\n");
        return;
    }
    if(source == -1)
    {
        printf("\nPlease select source and find shortest distance first.\n");
        return;
    }
    printf("\n============= SHORTEST PATHS =============\n");
    printf("Source: %s\n\n", location[source]);
    for(i = 0; i < n; i++)
    {
        printf("Destination: %s\n", location[i]);
        if(distance[i] == INF)
        {
            printf("Path: Not reachable\n");
        }
        else
        {
            printf("Path: ");
            printPath(i);
            printf("\nDistance: %d\n", distance[i]);
        }
        printf("\n");
    }
}

/* Display shortest distances */
void displayDistances()
{
    int i;
    if(n == 0)
    {
        printf("\nPlease enter the campus graph first.\n");
        return;
    }
    if(source == -1)
    {
        printf("\nPlease select source and find shortest distance first.\n");
        return;
    }
    printf("\n========== DISTANCES FROM SOURCE ==========\n");
    printf("Source: %s\n\n", location[source]);
    printf("%-25s %s\n",
           "Location", "Shortest Distance");

    printf("-------------------------------------------\n");

    for(i = 0; i < n; i++)
    {
        printf("%-25s ", location[i]);
        if(distance[i] == INF)
            printf("Not reachable\n");
        else
            printf("%d\n", distance[i]);
    }
}
