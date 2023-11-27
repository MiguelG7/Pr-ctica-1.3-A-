# Práctica 1.3 Aestrella
Búsqueda heurística sin adversarios

## Tabla de Contenidos

- [Instalación](#instalación)
- [Uso](#uso)
- [Preguntas a responder](#Preguntas-a-responder)
- [Licencia](#licencia)
- [Contacto](#contacto)

## Instalación

Clone el repositorio en su área de trabajo mediante el siguiente:
```bash
git clone https://github.com/MiguelG7/Practica-1.3-Aestrella.git
```

## Uso

Una vez ya haya clonado los archivos, descomprima 'java-algorithms-implementation.zip' y desde el directorio que contiene el archivo 'build.xml' ejecute el siguiente comando:
```
ant run_main
```

## Preguntas a responder

1. ¿Qué variable representa la lista ABIERTA?

La lista abierta se representa mediante la variable 'openSet'.

```
// código extraído de AStar.java

final List<Graph.Vertex<T>> openSet = new ArrayList<Graph.Vertex<T>>(size); // The set of tentative nodes to be evaluated, initially containing the start node
openSet.add(start);
```

2. ¿Qué variable representa la función g?

La función g es el coste real acumulado desde el nodo inicial hasta el nodo actual. La función g es representada por la variable 'gScore'.

```
// Código extraído de AStar.java

final Map<Graph.Vertex<T>,Integer> gScore = new HashMap<Graph.Vertex<T>,Integer>(); // Cost from start along best known path.
gScore.put(start, 0);
```

3. ¿Qué variable representa la función f ?

La función f es una estimación del coste total desde el nodo inicial hasta el nodo objetivo a través del nodo actual.La función f es representada por la variable 'fScore'.
```
// Código extraído de AStar.java

// Estimated total cost from start to goal through y.
final Map<Graph.Vertex<T>,Integer> fScore = new HashMap<Graph.Vertex<T>,Integer>();
for (Graph.Vertex<T> v : graph.getVertices())
  fScore.put(v, Integer.MAX_VALUE);
fScore.put(start, heuristicCostEstimate(start,goal));
```

4. ¿Qué método habría que modificar para que la heurística representara la distancia aérea entre vértices?

Para modificar la heurística para representar la distancia aérea entre vértices, necesitarías cambiar la implementación de 'heuristicCostEstimate'. En la implementación actual, esta función simplemente devuelve un valor constante de 1, así que puedes modificarla para calcular la distancia entre dos vértices.

```
// Código extraído de AStar.java

protected int heuristicCostEstimate(Graph.Vertex<T> start, Graph.Vertex<T> goal) {
  return 1;
}
```

5. ¿Realiza este método reevaluación de nudos cuando se encuentra una nueva ruta a un determinado vértice? Justifique la respuesta.

Cuando se encuentra una nueva ruta más corta a un nodo que ya ha sido evaluado, el algoritmo reevalúa ese nodo y actualiza la información si la nueva ruta es mejor que la ruta previamente almacenada. Lo podemos ver en el siguiente fragmento del código donde se comparan los costes estimados desde el nodo inicial hasta uno determinado y se actualizan las estructuras de datos 'cameFrom', 'gScore', y 'fScore' si se encuentra una ruta más eficaz a un nodo que ya hemos estudiado.

Conviene aclarar que 'cameFrom' se utiliza para almacenar información sobre el camino más corto conocido desde el nodo inicial hacia cada nodo en el grafo. 

```
// Código extraído de AStar.java

while (!openSet.isEmpty()) {
  final Graph.Vertex<T> current = openSet.get(0);
  if (current.equals(goal))
  return reconstructPath(cameFrom, goal);

  openSet.remove(0);
  closedSet.add(current);
  for (Graph.Edge<T> edge : current.getEdges()) {
    final Graph.Vertex<T> neighbor = edge.getToVertex();
    if (closedSet.contains(neighbor))
      continue; // Ignore the neighbor which is already evaluated.

    final int tenativeGScore = gScore.get(current) + distanceBetween(current,neighbor); // length of this path.
    if (!openSet.contains(neighbor))
      openSet.add(neighbor); // Discover a new node
    else if (tenativeGScore >= gScore.get(neighbor))
      continue;

    // This path is the best until now. Record it!
    cameFrom.put(neighbor, current);
    gScore.put(neighbor, tenativeGScore);
    final int estimatedFScore = gScore.get(neighbor) + heuristicCostEstimate(neighbor, goal);
    fScore.put(neighbor, estimatedFScore);

    // fScore has changed, re-sort the list
    Collections.sort(openSet,comparator);
  }
}
```

## Licencia
Este proyecto está bajo la Licencia MIT - vea el archivo [LICENSE](LICENSE) para más detalles.

## Contacto
Miguel Gamboa Sánchez
[Correo](mailto:miguel.gamboasanchez@usp.ceu.es)
[GitHub](https://github.com/MiguelG7)
