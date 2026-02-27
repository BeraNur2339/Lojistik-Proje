#include <stdio.h>
#include <stdlib.h>
#include <limits.h>

#define V 7 // A:0, B:1, C:2, D:3, E:4, F:5, G:6

// --- BÖLÜM 4: HASH MALİYETLERİ ---
int hashCosts[] = {1, 3, 2, 4, 2, 1, 4}; // A, B, C, D, E, F, G

struct AdjNode {
    int dest, weight;
    struct AdjNode* next;
};

struct Graph {
    struct AdjNode* head[V];
};

void addEdge(struct Graph* g, int u, int v, int w) {
    struct AdjNode* node = (struct AdjNode*)malloc(sizeof(struct AdjNode));
    node->dest = v; node->weight = w;
    node->next = g->head[u]; g->head[u] = node;
}

void dijkstraAnaliz(struct Graph* g, int start, int target, char targetChar) {
    int dist[V], parent[V], visited[V] = {0};
    int opCount = 0;

    for(int i=0; i<V; i++) { dist[i] = INT_MAX; parent[i] = -1; }
    dist[start] = 0;

    for(int i=0; i<V; i++) {
        int u = -1, min = INT_MAX;
        for(int v=0; v<V; v++) {
            opCount++;
            if(!visited[v] && dist[v] < min) { min = dist[v]; u = v; }
        }
        if(u == -1 || u == target) break;
        visited[u] = 1;

        struct AdjNode* tmp = g->head[u];
        while(tmp) {
            opCount++;
            if(!visited[tmp->dest] && dist[u] != INT_MAX && dist[u] + tmp->weight < dist[tmp->dest]) {
                dist[tmp->dest] = dist[u] + tmp->weight;
                parent[tmp->dest] = u;
            }
            tmp = tmp->next;
        }
    }

    printf("\n=== %c DUGUMU ANALIZI ===\n", targetChar);
    printf("[BOLUM 3] Yol Maliyeti: %d\n", dist[target]);

    // Rota yazdırma
    int path[V], pathIdx = 0;
    int curr = target;
    int totalHash = 0;
    while(curr != -1) {
        path[pathIdx++] = curr;
        totalHash += hashCosts[curr];
        curr = parent[curr];
    }

    printf("Rota: ");
    for(int i = pathIdx - 1; i >= 0; i--) {
        printf("%c%s", "ABCDEFG"[path[i]], i == 0 ? "" : " -> ");
    }
    
    printf("\n[BOLUM 4] Hash Maliyeti: %d\n", totalHash);
    printf("TOPLAM SISTEM MALIYETI: %d\n", dist[target] + totalHash);
    printf("[BOLUM 5] Islem Sayisi: %d\n", opCount);
}

int main() {
    struct Graph* g = (struct Graph*)malloc(sizeof(struct Graph));
    for(int i=0; i<V; i++) g->head[i] = NULL;

    // --- KAĞIDINDAKİ BAĞLANTILAR VE MALİYETLER ---
    // A'dan çıkanlar
    addEdge(g, 0, 1, 4); // A-B: 4
    addEdge(g, 0, 2, 2); // A-C: 2

    // B'den çıkanlar
    addEdge(g, 1, 2, 1); // B-C: 1
    addEdge(g, 1, 3, 4); // B-D: 4 (Kağıttaki güncel 2x değer)
    addEdge(g, 1, 6, 6); // B-G: 6 (Öğrenci no sonu)

    // C'den çıkanlar
    addEdge(g, 2, 3, 5); // C-D: 5
    addEdge(g, 2, 4, 10);// C-E: 10

    // D'den çıkanlar
    addEdge(g, 3, 4, 3); // D-E: 3
    addEdge(g, 3, 5, 6); // D-F: 6
    addEdge(g, 3, 6, 1); // D-G: 1 (Öğrenci no başı)

    // E'den çıkanlar
    addEdge(g, 4, 5, 1); // E-F: 1

    // ANALİZLER
    printf("PROJE: DINAMIK LOJISTIK AG ANALIZI\n");
    
    // 3. Bölüm:(Hedef G)
    // Beklenen Rota: A -> C -> D -> G | Maliyet: 2+5+1 = 8
    dijkstraAnaliz(g, 0, 6, 'G');

    // 4. Bölüm:(Hedef F)
    // Beklenen Rota: A -> C -> D -> F | Maliyet: 2+5+6 = 13
    dijkstraAnaliz(g, 0, 5, 'F');

    return 0;
}
