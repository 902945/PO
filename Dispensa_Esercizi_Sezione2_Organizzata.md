# Dispensa Esercizi Sezione 2 - Organizzata per Pattern Implementativi

**Documento generato il**: 2026-05-04

Questo documento contiene tutti gli esercizi della Sezione 2 dell'eserciziario PO2, organizzati per macro-gruppi basati sui pattern implementativi comuni. Ogni macro-gruppo include:
- Una soluzione blueprint che funge da template
- Tutti gli esercizi appartenenti al gruppo
- Le soluzioni complete e commentate

---

## Indice Generale

### 📋 Navigazione per Macro-Gruppi

1. [Iteratori Customizzati e Collection](#1-iteratori-customizzati-e-collection)
2. [Strutture Dati (Alberi, Grafi, Matrici)](#2-strutture-dati-alberi-grafi-matrici)
3. [Generics e Type System](#3-generics-e-type-system)
4. [Concorrenza e Threading](#4-concorrenza-e-threading)
5. [Programmazione Funzionale e HOF](#5-programmazione-funzionale-e-hof)
6. [Design Patterns e OOP](#6-design-patterns-e-oop)

### 🔍 Indice per Parole Chiave

**Iteratori e Iterazione**
- `Iterator` / `Iterable` → [Macro-gruppo 1](#1-iteratori-customizzati-e-collection)
- `hasNext()` / `next()` → Es. 2.1.7, 2.1.10, 2.1.13, 2.1.14, 2.1.16
- Iteratore lazy / caching → Es. 2.1.14 (Fibonacci), 2.1.7
- Iteratore con step → Es. 2.1.16 (FancyArrayList)
- Iteratore bidirezionale → Es. 2.1.16

**Strutture Dati**
- `BST` / Binary Search Tree → Es. 2.1.8
- `TreeNode` / Albero binario → Es. 2.1.9, 2.2.7
- DFS / Traversal → Es. 2.1.8, 2.1.9, 2.2.7
- Inserimento ricorsivo → Es. 2.1.8
- `Matrix` / Matrici → Es. 2.2.2
- Geometria / `Point` / `Line` / `Polygon` → Es. 2.1.3

**Generics e Type System**
- `<T extends Comparable<T>>` → Es. 2.1.1, 2.1.8, 2.1.17
- `Comparator<T>` → Es. 2.1.3, 2.1.8, 2.1.17
- Wildcards `? extends` / `? super` → Es. 2.1.1, 2.1.11
- PECS (Producer Extends Consumer Super) → Es. 2.1.11
- Template C++ → Es. 2.2.1, 2.2.3
- `value_type` / Type traits → Es. 2.2.3

**Concorrenza**
- `Thread` / `Runnable` → Es. 2.1.4, 2.1.5, 2.1.6
- `BlockingQueue` → Es. 2.1.2
- `acquire()` / `release()` → Es. 2.1.2
- `InterruptedException` → Es. 2.1.2, 2.1.5, 2.1.6
- Thread Pool → Es. 2.1.2
- Esecuzione parallela → Es. 2.1.5

**Programmazione Funzionale**
- `Function<A,B>` / `BiFunction` → Es. 2.1.4, 2.1.11, 2.1.13
- `map()` → Es. 2.1.4, 2.1.5, 2.2.4
- `fold()` / `reduce()` → Es. 2.1.11
- Lambda expressions → Es. 2.1.4, 2.1.7, 2.2.4, 2.2.5
- `Supplier<T>` → Es. 2.1.4, 2.1.6
- `std::function<>` (C++) → Es. 2.2.4, 2.2.6
- Higher-Order Functions → Es. 2.1.4, 2.1.5, 2.1.11

**Design Patterns**
- Singleton → Es. 2.1.15
- Factory Pattern → [Macro-gruppo 6](#6-design-patterns-e-oop)
- RAII → Es. 2.2.8
- Smart Pointer / Reference Counting → Es. 2.2.8
- Proxy Pattern → Es. 2.1.2 (Resource wrapper)
- Abstract Class / Inheritance → Es. 2.1.18

**Altri Concetti**
- `equals()` / `equalsTo()` → Es. 2.1.12
- `Predicate<T>` → Es. 2.1.10
- `Either` / Monad-like → Es. 2.1.10
- Pretty Printing → Es. 2.1.9
- Operator Overloading (C++) → Es. 2.2.1, 2.2.2, 2.2.8
- SFINAE / Template Constraints → Es. 2.2.3

---

## 1. Iteratori Customizzati e Collection

### 📘 Blueprint: Iteratore Customizzato Base

**Pattern comune**: Implementazione di un iteratore personalizzato che estende/implementa Iterator/Iterable.

#### Struttura Base Java

```java
import java.util.Iterator;
import java.util.NoSuchElementException;

public class CustomIterator<T> implements Iterator<T> {
    private int position = 0;
    private List<T> data;
    
    public CustomIterator(List<T> data) {
        this.data = data;
    }
    
    @Override
    public boolean hasNext() {
        return position < data.size();
    }
    
    @Override
    public T next() {
        if (!hasNext()) {
            throw new NoSuchElementException("No more elements");
        }
        return data.get(position++);
    }
}

// Classe Iterable
public class CustomCollection<T> implements Iterable<T> {
    private List<T> data = new ArrayList<>();
    
    public void add(T item) {
        data.add(item);
    }
    
    @Override
    public Iterator<T> iterator() {
        return new CustomIterator<>(data);
    }
}
```

#### Struttura Base C++

```cpp
template<typename T>
class custom_iterator {
    T* ptr;
    
public:
    using iterator_category = std::forward_iterator_tag;
    using value_type = T;
    using difference_type = std::ptrdiff_t;
    using pointer = T*;
    using reference = T&;
    
    custom_iterator(T* p = nullptr) : ptr(p) {}
    
    // Dereference
    T& operator*() { return *ptr; }
    T* operator->() { return ptr; }
    
    // Pre-increment
    custom_iterator& operator++() {
        ++ptr;
        return *this;
    }
    
    // Post-increment
    custom_iterator operator++(int) {
        custom_iterator tmp = *this;
        ++ptr;
        return tmp;
    }
    
    // Comparison
    bool operator==(const custom_iterator& other) const {
        return ptr == other.ptr;
    }
    
    bool operator!=(const custom_iterator& other) const {
        return ptr != other.ptr;
    }
};
```

#### Varianti Comuni

1. **Iteratore con caching**: Memorizza risultati calcolati (es. Fibonacci)
2. **Iteratore lazy**: Calcola elementi on-demand
3. **Iteratore con trasformazione**: Applica una funzione durante l'iterazione
4. **Iteratore con skip/step**: Salta elementi secondo un pattern
5. **Iteratore bidirezionale**: Supporta iterazione avanti e indietro

#### Concetti Chiave

- **Separazione tra container e iteratore**: L'iteratore mantiene lo stato dell'iterazione
- **Lazy evaluation**: Calcola solo quando richiesto
- **Type safety**: Uso di generics per evitare cast
- **Iterator invalidation**: Attenzione alle modifiche durante l'iterazione

---

