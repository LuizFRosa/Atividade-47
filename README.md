# Atividade-47

#include <stdio.h>
#include <stdlib.h>


typedef struct {
    int origem, destino, peso;
} Aresta;


typedef struct {
    int V, E;  
    Aresta* aresta;
} Grafo;


typedef struct {
    int pai;
    int rank;
} Subconjunto;


Grafo* criarGrafo(int V, int E) {
    Grafo* grafo = (Grafo*) malloc(sizeof(Grafo));
    grafo->V = V;
    grafo->E = E;
    grafo->aresta = (Aresta*) malloc(E * sizeof(Aresta));
    return grafo;
}


int find(Subconjunto subconjuntos[], int i) {
    if (subconjuntos[i].pai != i)
        subconjuntos[i].pai = find(subconjuntos, subconjuntos[i].pai);
    return subconjuntos[i].pai;
}


void unionSubconjuntos(Subconjunto subconjuntos[], int x, int y) {
    int raizX = find(subconjuntos, x);
    int raizY = find(subconjuntos, y);

    if (subconjuntos[raizX].rank < subconjuntos[raizY].rank)
        subconjuntos[raizX].pai = raizY;
    else if (subconjuntos[raizX].rank > subconjuntos[raizY].rank)
        subconjuntos[raizY].pai = raizX;
    else {
        subconjuntos[raizY].pai = raizX;
        subconjuntos[raizX].rank++;
    }
}


int compararArestas(const void* a, const void* b) {
    Aresta* a1 = (Aresta*) a;
    Aresta* a2 = (Aresta*) b;
    return a1->peso > a2->peso;
}


void kruskalMST(Grafo* grafo) {
    int V = grafo->V;
    Aresta resultado[V]; 
    int e = 0;  
    int i = 0;  

   
    qsort(grafo->aresta, grafo->E, sizeof(grafo->aresta[0]), compararArestas);

   o)
    Subconjunto* subconjuntos = (Subconjunto*) malloc(V * sizeof(Subconjunto));

    
    for (int v = 0; v < V; v++) {
        subconjuntos[v].pai = v;
        subconjuntos[v].rank = 0;
    }

    
    while (e < V - 1 && i < grafo->E) {
        
        Aresta proximaAresta = grafo->aresta[i++];

        int x = find(subconjuntos, proximaAresta.origem);
        int y = find(subconjuntos, proximaAresta.destino);

       
        if (x != y) {
            resultado[e++] = proximaAresta;
            unionSubconjuntos(subconjuntos, x, y);
        }
      
    }

    printf("As arestas na MST são:\n");
    for (i = 0; i < e; i++)
        printf("%d -- %d == %d\n", resultado[i].origem, resultado[i].destino, resultado[i].peso);

    free(subconjuntos);
}

int main() {
   

    int V = 4;  
    int E = 5;  
    Grafo* grafo = criarGrafo(V, E);

   
    grafo->aresta[0].origem = 0;
    grafo->aresta[0].destino = 1;
    grafo->aresta[0].peso = 10;

    
    grafo->aresta[1].origem = 0;
    grafo->aresta[1].destino = 2;
    grafo->aresta[1].peso = 6;

    
    grafo->aresta[2].origem = 0;
    grafo->aresta[2].destino = 3;
    grafo->aresta[2].peso = 5;

    
    grafo->aresta[3].origem = 1;
    grafo->aresta[3].destino = 3;
    grafo->aresta[3].peso = 15;

    
    grafo->aresta[4].origem = 2;
    grafo->aresta[4].destino = 3;
    grafo->aresta[4].peso = 4;

    kruskalMST(grafo);

    free(grafo->aresta);
    free(grafo);

    return 0;
}
