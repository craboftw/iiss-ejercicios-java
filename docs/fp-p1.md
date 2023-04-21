# Práctica 1: Anónimos & Cierres

## Ejercicios propuestos

### Ejercicio 1

Dado los siguientes fragmentos de código que implementan un API dado por la interfaz `DataOperations`, responder a las siguientes preguntas:

#### `DataOperations.java`

```java
public interface DataOperations {
    public void print(int[] data);
    public int[] filterPairs(int[] data);
}
```

#### `DataOperationsImpl.java`

```java
public class DataOperationsImpl implements DataOperations {
    @Override
    public void print(int[] data) {
        for(int element: data) {
            System.out.print(element + ", ");
        }
        System.out.println();
    }

    @Override
    public int[] filterPairs(int[] data) {
        int index = 0;
        int[] dataAux = new int[data.length];
        for(int element: data) {
            if((element % 2) != 0) {
                dataAux[index] = element;
                index++;
            }
        }
        return dataAux;
    }
}
```

#### `Main.java`

```java
import java.util.Arrays;

public class Main {
    public static void main(String args[]) {
        int[] data = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
        System.out.println("data = " + Arrays.toString(data));
        DataOperations operations = new DataOperationsImpl();
        operations.print(data);
        data = operations.filterPairs(data);
        operations.print(data);
    }
}
```

#### Preguntas propuestas

1. Utilice expresiones *lambda* y el API de *streams* de Java para cambiar la implementación de las operaciones de la interfaz `DataOperations` usando los mecanismos de la programación funcional.

El uso de las funciones proporcionadas por HashSet de Java de forma directa no es adecuado si se hace uso de la herencia. La superclase HashSet ya registra el número de elementos almacenados en el conjunto, lo que significa que no es necesario tener un atributo adicional para este propósito. Además, debido a la adición duplicada de datos, el valor devuelto por el método "getNumberOfAddedElements" será 6.

2. Además, haciendo uso de expresiones *labmda* y del API de *streams*, añada a la interfaz de `DataOperations` las siguientes operaciones y su implementación:


- Operación que devuelva la lista de números ordenada descendentemente.
- Operación que multiplique todos los números de la lista por 10 e imprima el resultado.
- Operación que devuelva el resultado de la suma de todos los números de la lista.

### Respuesta

#### Pregunta 1

Para utilizar expresiones lambda y el API de streams de Java, se pueden modificar los métodos `print` y `filterPairs` de la siguiente manera:

```java
public interface DataOperations {
    public void print(int[] data);
    public int[] filterPairs(int[] data);
    
    default void printWithStream(int[] data) {
        Arrays.stream(data).forEach(e -> System.out.print(e + ", "));
        System.out.println();
    }
    
    default int[] filterPairsWithStream(int[] data) {
        return Arrays.stream(data).filter(e -> e % 2 != 0).toArray();
    }
}
```

En el código anterior, se han agregado dos métodos nuevos en la interfaz `DataOperations`. Ambos métodos tienen la palabra clave `default`, lo que significa que se les está proporcionando una implementación por defecto. La implementación por defecto utiliza expresiones lambda y el API de streams de Java para proporcionar la funcionalidad requerida.

En el método `printWithStream`, se utiliza el método `forEach` de la clase `IntStream` para imprimir cada elemento del arreglo `data`. El operador `->` se utiliza para definir una expresión lambda que toma un argumento de tipo `int` y lo imprime en la salida estándar.

En el método `filterPairsWithStream`, se utiliza el método `filter` de la clase `IntStream` para filtrar los elementos pares del arreglo `data`. El operador `%` se utiliza para calcular el resto de la división de cada elemento entre 2, y así determinar si el elemento es par o impar. El método `toArray` se utiliza para convertir el `IntStream` resultante en un arreglo de `int`.


#### Pregunta 2

Para agregar las nuevas operaciones a la interfaz `DataOperations` y su implementación, se pueden hacer los siguientes cambios:

```java
public interface DataOperations {
    public void print(int[] data);
    public int[] filterPairs(int[] data);
    public int[] sortDescending(int[] data);
    public void printTimesTen(int[] data);
    public int sum(int[] data);
    
    default void printWithStream(int[] data) {
        Arrays.stream(data).forEach(e -> System.out.print(e + ", "));
        System.out.println();
    }
    
    default int[] filterPairsWithStream(int[] data) {
        return Arrays.stream(data).filter(e -> e % 2 != 0).toArray();
    }
    
    default int[] sortDescendingWithStream(int[] data) {
        return Arrays.stream(data).boxed()
            .sorted(Collections.reverseOrder())
            .mapToInt(Integer::intValue)
            .toArray();
    }
    
    default void printTimesTenWithStream(int[] data) {
        Arrays.stream(data).map(e -> e * 10).forEach(e -> System.out.print(e + ", "));
        System.out.println();
    }
    
    default int sumWithStream(int[] data) {
        return Arrays.stream(data).sum();
    }
}
```

En el código anterior, se han agregado cinco nuevos métodos a la interfaz `DataOperations`. Los métodos `sortDescending`, `printTimesTen` y `sum` realizan las operaciones especificadas, mientras que los métodos `sortDescendingWithStream`, `printTimesTenWithStream` y `sumWithStream` proporcionan la implementación de dichas operaciones utilizando expresiones lambda y el API de streams de Java.

En el método `sortDescendingWithStream`, se utiliza el método `boxed` de la clase `IntStream` para convertir el stream de `int` a un stream de `Integer` y así poder utilizar el método `sorted` de la clase Stream que devuelve un stream de elementos ordenados. Luego se utiliza el método `mapToInt` de la clase Stream para convertir el stream de `Integer` a un stream de `int` y se utiliza el método `toArray` de la clase IntStream para obtener un arreglo de `int` con los elementos ordenados de forma descendente.

En el método multiplyByTenWithStream, se utiliza el método `map` de la clase Stream para multiplicar cada elemento de la lista por 10 y se utiliza el método `forEach` de la clase Stream para imprimir el resultado.

En el método sumWithStream, se utiliza el método `sum` de la clase IntStream para obtener la suma de todos los elementos de la lista.

A continuación se muestra la nueva versión de la interfaz `DataOperations` con las operaciones agregadas:

```java
import java.util.List;

public interface DataOperations {
    void print(int[] data);
    int[] filterPairs(int[] data);
    int[] sortDescendingWithStream(int[] data);
    void multiplyByTenWithStream(int[] data);
    int sumWithStream(int[] data);
}
```

Y a continuación se muestra la implementación de estas operaciones en la clase `DataOperationsImpl` usando expresiones lambda y el API de streams:

```java
import java.util.Arrays;

public class DataOperationsImpl implements DataOperations {
    @Override
    public void print(int[] data) {
        for(int element: data) {
            System.out.print(element + ", ");
        }
        System.out.println();
    }

    @Override
    public int[] filterPairs(int[] data) {
        return Arrays.stream(data)
                .filter(element -> (element % 2) != 0)
                .toArray();
    }

    @Override
    public int[] sortDescendingWithStream(int[] data) {
        return Arrays.stream(data)
                .boxed()
                .sorted((a, b) -> Integer.compare(b, a))
                .mapToInt(Integer::intValue)
                .toArray();
    }

    @Override
    public void multiplyByTenWithStream(int[] data) {
        Arrays.stream(data)
                .map(element -> element * 10)
                .forEach(System.out::println);
    }

    @Override
    public int sumWithStream(int[] data) {
        return Arrays.stream(data)
                .sum();
    }
}
```

Con estas implementaciones, se pueden utilizar todas las operaciones de la interfaz `DataOperations` de manera funcional usando expresiones lambda y el API de streams.

### Ejercicio 2

Dado los siguientes fragmentos de código que implementan un API cuya interfaz viene dada por `DataSorter`, responder a las siguientes preguntas.

#### `DataSorter.java`

```java
public interface DataSorter {
    public String[] sort(String[] data);
}
```

#### `DataSorterAsc.java`

```java
import java.util.Arrays;

public class DataSorterAsc implements DataSorter {
    public String[] sort(String[] data) {
        Arrays.sort(data);
        return data;
    }
}
```

#### `DataSorterDesc.java`

```java
import java.util.Arrays;
import java.util.Collections;

public class DataSorterDesc implements DataSorter {
    public String[] sort(String[] data) {
        Arrays.sort(data, Collections.reverseOrder());
        return data;
    }
}

```

#### `Main.java`

```java
import java.util.Arrays;

public class Main {
    public static void main(String args[]) {
        String [] data = {"H", "S", "I", "V", "E", "W", "M", "P", "L",  "C", "N", "K",
                 "O", "A", "Q", "R", "J", "D", "G", "T", "U", "X", "B", "Y", "Z", "F"};
        System.out.println("data = " + Arrays.toString(data));
        DataSorter dataSorter = new DataSorterDesc();
        dataSorter.sort(data);
        System.out.println("data (desc) = " + Arrays.toString(data));
        dataSorter = new DataSorterAsc();
        dataSorter.sort(data);
        System.out.println("data (asc) = " + Arrays.toString(data));
    }
}
```

#### Preguntas propuestas

1. Utilice cierres (*closures*) para cambiar la implementación de las clases `DataSorterAsc` y `DataSorterDesc` usando los mecanismos de la programación funcional.

2. Añada un tercer cambio haciendo uso de cierres (*closures*) para realizar la ordenación aleatoria de los elementos, siguiendo el mismo enfoque aplicado con las clases `DataSorterAsc` y `DataSorterDesc` en el apartado anterior.

## Referencias

[Java 8 Stream Tutorial]: https://winterbe.com/posts/2014/07/31/java8-stream-tutorial-examples/
[[1] Blog: Java 8 Stream Tutorial.][Java 8 Stream Tutorial]
[API Java Streams]: https://www.oracle.com/technetwork/es/articles/java/procesamiento-streams-java-se-8-2763402-esa.html
[[2] Documentación Oficial Java: Procesamiento de datos con streams de Java SE 8.][API Java Streams]
[API Java Funciones Lambda + Streams]: https://www.oracle.com/technetwork/es/articles/java/expresiones-lambda-api-stream-java-2737544-esa.html
[[3] Documentación Oficial Java: Introducción Expresiones Lambda y API Stream en Java SE 8.][API Java Funciones Lambda + Streams]
[Closures in Java]: https://medium.com/@rmanavalan/5-ways-to-implement-closures-in-java-8-590790659ac5
[[4] Blog: 5 ways to implement Closures in Java 8.][Closures in Java]
