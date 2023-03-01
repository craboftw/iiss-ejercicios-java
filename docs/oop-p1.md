# Práctica 1: Herencia, composición y polimorfismo

## Ejercicios propuestos

### Ejercicio 1

Dados los siguientes fragmentos de código, responder a las siguientes preguntas:

#### `ElementsSet.java`

```java
public class ElementsSet<E> extends HashSet<E> {
    //Number of attempted elements insertions using the "add" method
    private int numberOfAddedElements = 0;

    public ElementsSet() {}

    @Override
    public boolean add(E element) {
        numberOfAddedElements++; //Counting the element added
        return super.add(element);
    }

    @Override
    public boolean addAll(Collection<? extends E> elements) {
        numberOfAddedElements += elements.size(); //Counting the elements added
        return super.addAll(elements);
    }

    public int getNumberOfAddedElements() {
        return numberOfAddedElements;
    }
}
```

#### `Main.java`

```java
    ...
    ElementsSet<String> set = new ElementsSet<String>();
    set.addAll(Arrays.asList("One", "Two", "Three"));
    System.out.println(set.getNumberOfAddedElements());
    ...
```

#### Preguntas propuestas

a) ¿Es el uso de la herencia adecuado para la implementación de la clase `ElementsSet`? ¿Qué salida muestra la función `System.out.println` al invocar el método `getNumberOfAddedElements`, 3 o 6?

El uso de las funciones proporcionadas por HashSet de Java de forma directa no es adecuado si se hace uso de la herencia.
Al llamarse a AddAll se llama a un recorrido de la lista donde todos los elementos de ella son pasados a una funcion Add con lo cual vuelve a ejecutarse la suma y resulta en el doble de elementos a los introducidos. Por tanto el resultado muestra 6.

b) En el caso de que haya algún problema en la implementación anterior, proponga una solución alternativa usando composición/delegación que resuelva el problema.

Con una composicion podriamos arreglar este problema de herencia

```java

public class ElementSet<T>{
    private HashSet<T> set;

    public ElementsSet()
    {
        set = new HashSet<T>();
    }

    public boolean add(T elemento)
    {
        return set.add(elemento);
    }

    public boolean addAll(Collection <? extends T> elementos)
    {
        return  set.addAll(elementos);
    }

    public int size()
    {
        return set.size();
    }
}
```

### Ejercicio 2

Dado los siguientes fragmentos de código responder a las siguientes preguntas:

#### `Animal.java`

```java
public abstract class Animal {
    //Number of legs the animal holds
    protected int numberOfLegs = 0;

    public abstract String speak();
    public abstract boolean eat(String typeOfFeed);
    public abstract int getNumberOfLegs();
}
```

#### `Cat.java`

```java
public class Cat extends Animal {
    @Override
    public String speak() {
        return "Meow";
    }

    @Override
    public boolean eat(String typeOfFeed) {
        if(typeOfFeed.equals("fish")) {
            return true;
        } else {
            return false;
        }
    }

    @Override
    public int getNumberOfLegs() {
        return super.numberOfLegs;
    }
}
```

#### `Dog.java`

```java
public class Dog extends Animal {
    @Override
    public String speak() {
        return "Woof";
    }

    @Override
    public boolean eat(String typeOfFeed) {
        if(typeOfFeed.equals("meat")) {
            return true;
        } else {
            return false;
        }
    }

    @Override
    public int getNumberOfLegs() {
        return super.numberOfLegs;
    }
}
```

#### `Main.java`

```java
    ...
    Animal cat = new Cat();
    Animal dog = new Dog();
    System.out.println(cat.speak());
    System.out.println(dog.speak());
    ...
```

#### Preguntas propuestas

a) ¿Es correcto el uso de herencia en la implementación de las clases `Cat` y `Dog`? ¿Qué beneficios se obtienen?

Ninguna de las dos clases tiene un constructor, en caso de que animal tuviera constructor, imagino que contienen alguna manera de indicar el numero de patas. La herencia permite crear un animal sin especializacion y debido a que los metodos son abstractos estos no hablan de manera incorrecta o comen algun tipo especifico de comida.
No entiendo porque numberOfLegs en la clase animal no imprime el numero de piernas, eso es un error de implementacion.

Una mejor implementacion de lo mismo seria una donde al constructor de Animal recibe lo que come, el numero de patas que tiene y la onomatopeya y asi podriamos crear animales genericos y ademas especializaciones de los mismos, pero requeriria de definir los metodos de Animales.

b) En el caso de que el uso de la herencia no sea correcto, proponga una solución alternativa. ¿Cuáles son los beneficios de la solución propuesta frente a la original?

```java
public abstract class Animal {
    //Number of legs the animal holds
    Animal (int NumberLegs,String Sonido,String eat)
    {
        numberOfLegs = NumberLegs;
        sound = Sonido;
        meal = eat;
    }
    Animal () {}
    protected int numberOfLegs = 0;
    protected String sound = "";
    protected String meal = "";

    public String speak() {return sound};
    public boolean eat(String typeOfFeed) {
            if(typeOfFeed.equals(meal)) {
            return true;
        } else {
            return false;
        }
    }
    public int getNumberOfLegs() {return numberOfLegs}
}

public class Dog extends Animal {
Dog (){
    super(4,"Hoof","Meat")
    }

...
}
public class Dog extends Animal {
Cat (){
    super(4,"Meow","Fish")
    }

```
