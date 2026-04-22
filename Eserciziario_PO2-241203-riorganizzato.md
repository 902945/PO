# Eserciziario PO2 — Versione riorganizzata in Markdown

Documento derivato da `Eserciziario_PO2-241203.pdf`.
Ogni soluzione e' stata spostata sotto il rispettivo quesito, con una breve spiegazione del ragionamento.

---

## Indice

1. [Sezione 1 - Tests](#sezione-1--tests)
   - [1.1 Design Patterns](#11-design-patterns)
   - [1.2 Riuso di codice e Type system di Java](#12-riuso-di-codice-e-type-system-di-java)
   - [1.3.1 Equals](#131-equals)
   - [1.3.2 Compare](#132-compare)
   - [1.3.3 Programmazione ibrida funzionale](#133-programmazione-ibrida-funzionale)
2. [Sezione 2 - Esercizi](#sezione-2--esercizi-di-progettazione-e-implementazione)
3. [Sezione 3 - Domande aperte](#sezione-3--domande-aperte)

---

## Sezione 1 – Tests

### 1.1 Design Patterns

Questa sezione contiene domande sull'argomento **design patterns**.

#### Domanda 1 — [Esame 20/06/2018]

Si prenda in considerazione il seguente codice Java:

```java
public class MyClass {
    private static MyClass instance;
    public static MyClass get() {
        if (instance == null) instance = new MyClass();
        return instance;
    }
}
```

Quale design pattern e' all'opera?

- [ ] Command
- [ ] Factory
- [ ] nessun pattern
- [x] **Singleton** ✓

**Risposta corretta:** Singleton

**Spiegazione:** Il pattern Singleton garantisce una unica istanza di una classe: il metodo statico `get()` crea l'istanza solo se assente e la restituisce sempre, impedendo costruzione multipla.

---

#### Domanda 2 — [Esame 20/06/2018]

Perche' il design pattern denominato *factory* e' utile in Java e in altri linguaggi ad oggetti simili?

- [x] **Perche' i costruttori non sono metodi soggetti al dynamic dispatching, quindi non e' possibile sfruttare il polimorfismo per costruire oggetti se non tramite un metodo non-statico che incapsula la costruzione.** ✓
- [ ] Perche' la costruzione degli oggetti va nascosta dentro metodi statici al fine di poter rendere privati i costruttori, applicando cosi' i principi di information hiding ed incapsulamento.
- [ ] Siccome le sottoclassi non hanno accesso ai super-costruttori, e' necessario dotare le superclassi di metodi wrapper `protected` dei propri costruttori.
- [ ] Perche' non e' possibile costruire array con tipi generici in Java, pertanto e' preferibile mascherare gli inevitabili cast dentro un metodo statico.

**Risposta corretta:** Prima opzione — i costruttori non partecipano al dynamic dispatching.

**Spiegazione:** Il pattern Factory e' utile perche' i costruttori non partecipano al dynamic dispatching; incapsulando la creazione in un metodo (statico o di istanza) e' possibile restituire sottotipi polimorfi o delegare la costruzione.

---

### 1.2 Riuso di codice e Type system di Java

Questa sezione contiene domande su polimorfismo, genericita' e typing (subtyping, generics, override, overloading, ecc.).

#### Domanda 1 — [Esame 20/06/2018]

Java versione 10 supporta l'ereditarieta' multipla?

- [ ] Si', ma solo tra classi, mentre e' possibile implementare solamente una interfaccia alla volta.
- [ ] No, nemmeno tra interfacce, a causa del noto problema del diamante.
- [ ] Si', perche' risolve il *problema del diamante* mescolando opportunamente le virtual table delle superclassi.
- [x] **No, pero' non c'e' limite al numero di interfacce che una classe puo' implementare.** ✓

**Risposta corretta:** No, pero' non c'e' limite al numero di interfacce implementabili.

**Spiegazione:** Java non supporta ereditarieta' multipla tra classi per evitare il problema del diamante, ma non pone limiti al numero di interfacce implementabili.

---

#### Domanda 2 — [Esame 24/05/2018]

In generale, a cosa servono le due forme di polimorfismo (subtyping e generics) che offre Java oggi?

- [x] **A riusare lo stesso codice, ovvero chiamare un metodo, con parametri di tipo differente.** ✓
- [ ] A definire classi che implementano due o piu' interfacce.
- [ ] A riusare lo stesso codice, in termini di classi e dei loro membri, ereditandoli anziche' riscrivendoli.
- [ ] A permettere di definire classi o interfacce parametriche su altri tipi.

**Risposta corretta:** A riusare lo stesso codice con parametri di tipo differente.

**Spiegazione:** Il subtyping consente di passare oggetti di tipo derivato dove e' atteso il tipo base; i generics permettono di parametrizzare classi/metodi su tipi, evitando duplicazioni del codice.

---

#### Domanda 3 — [Esame 24/05/2018]

Siano omega, epsilon, theta, phi tipi concreti con: epsilon <: omega, theta <: epsilon, phi <: omega.
Si indichi quale delle seguenti relazioni **non** e' valida.

- [ ] theta <: omega
- [ ] epsilon e theta non sono in relazione (epsilon |<: theta)
- [x] **phi <: theta** ✓ (NON valida)
- [ ] omega e phi non sono in relazione (omega |<: phi)

**Risposta corretta:** `phi <: theta` non e' valida perche' phi <: omega ma non c'e' una relazione tra phi e epsilon/theta.

**Spiegazione:** Dalla catena epsilon <: omega e phi <: omega si evince che phi e theta non sono in relazione. La relazione phi <: theta richiederebbe phi <: epsilon <: omega, ma phi non deriva da epsilon.

---

#### Domanda 4 — [Esame 24/05/2018]

L'ereditarieta' e' una forma di polimorfismo?

- [ ] Si', perche' permette il riuso del codice di una classe e dei suoi membri pubblici anche nelle sottoclassi.
- [x] **No, ma rende possibile il polimorfismo subtype poiche' garantisce che i membri pubblici delle superclassi esistano nelle sottoclassi.** ✓
- [ ] Si', perche', grazie al dynamic dispatching, viene garantita l'invocazione corretta a run-time dei metodi in override.
- [ ] No, perche' l'ereditarieta' non garantisce davvero che i membri pubblici delle superclassi esistano anche nelle sottoclassi.

**Risposta corretta:** No, ma rende possibile il polimorfismo subtype.

**Spiegazione:** L'ereditarieta' abilita il riuso ma non costituisce polimorfismo di per se'. Il polimorfismo subtype si ottiene grazie al dynamic dispatching, che consente di trattare una sottoclasse come se fosse la superclasse.

---

#### Domanda 5 — [Esame 24/05/2018]

Quali parti della firma di un metodo sono coinvolte nella risoluzione dell'overloading in Java?

- [ ] Il tipo ed il numero dei parametri, il tipo di ritorno ma non le eccezioni dichiarate.
- [ ] Il tipo ed il numero dei parametri, il tipo di ritorno ed anche le eccezioni dichiarate.
- [x] **Il tipo ed il numero dei parametri e basta, senza tipo di ritorno ne' eccezioni.** ✓
- [ ] Il tipo ed il numero dei parametri inizialmente; se sono ambigui anche il tipo di ritorno, ma mai le eccezioni.

**Risposta corretta:** Solo tipo e numero dei parametri.

**Spiegazione:** In Java l'overloading e' risolto a compile-time guardando solo tipo e numero dei parametri; tipo di ritorno ed eccezioni non partecipano alla risoluzione.

---

#### Domanda 6 — [Esame 30/05/2019]

Si consideri la gerarchia con `Paladin extends Character<Float>` e `Rogue extends Character<Number>`.
Gli override del metodo `attack()` sono validi?

```java
@Override public Float    attack() { return mana * level / 2.f; }  // Paladin
@Override public Integer  attack() { return (energy -= 35) > 20 ? level*2 : 0; }  // Rogue
```

- [ ] No, perche' le sottoclassi violano la regola della contro-varianza del tipo di ritorno.
- [x] **Si', anche se le sottoclassi specializzano il tipo di ritorno rispetto al type argument passato alla superclasse, la specializzazione del tipo di ritorno e' supportata dall'overriding in Java.** ✓
- [ ] Non quello della classe `Rogue`, poiche' passa `Number` come type argument ma dichiara `Integer` come tipo di ritorno.
- [ ] Si', perche' la type erasure converte il generic R nel suo constraint `Number`.

**Risposta corretta:** Si', l'overriding supporta la co-varianza del tipo di ritorno.

**Spiegazione:** La co-varianza del tipo di ritorno e' permessa in Java: un override puo' restituire un sottotipo del tipo dichiarato nella firma base. Sia `Float` che `Integer` sono sottotipi dei rispettivi type argument.

---

#### Domanda 7 — [Esame 30/05/2019]

Affinche' il seguente statement compili, quali modifiche alla firma della funzione `map()` sarebbero necessarie e sufficienti?

```java
Collection<Number> r1 = map(retadins, new Function<Character, Float>() {
    public Float apply(Character c) { return 1.3f * (float) c.attack(); }
});
```

- [x] **`static <A,B> Collection<B> map(Collection<A> c, Function<? super A, ? extends B> f)`** ✓
- [ ] `static <A,B> Collection<B> map(Collection<A> c, Function<? extends A, ? super B> f)`
- [ ] `static <A,B> Collection<B> map(Collection<? super A> c, Function<A,B> f)`
- [ ] `static <A,B> Collection<B> map(Collection<? extends A> c, Function<A, ? extends B> f)`

**Risposta corretta:** `Function<? super A, ? extends B>`

**Spiegazione:** Usare `? super A` e `? extends B` (principio PECS) rende la funzione `map` piu' flessibile: accetta funzioni che consumano supertipi di A (quindi `Function<Character, ...>` quando A=Paladin) e producono sottotipi di B.

---

#### Domanda 8 — [Esame 30/05/2019]

I due metodi `normalizeAttack` in overload sarebbero validi se aggiunti come membri a `Rpg`?

```java
public static int   normalizeAttack(Character c) { return 1 + (int) c.attack(); }
public static Float normalizeAttack(Paladin c)   { return c.attack(); }
```

**(a)** Sarebbero validi?

- [x] **Si', perche' i due metodi hanno parametri di tipo differente; e nonostante `Character` sia un supertipo di `Paladin` il compilatore e' in grado di discriminare.** ✓
- [ ] No, perche' i metodi statici non supportano l'overloading.
- [ ] No, perche' sebbene i due metodi abbiano parametri di tipo differente, `Character` e' un supertipo di `Paladin`, quindi il compilatore non sarebbe in grado di risolvere l'overloading senza ambiguita'.

**(b)** Il seguente statement compilerebbe (con la firma originale di `map()`)?

```java
Collection<Number> r2 = map(retadins, Rpg::normalizeAttack);
```

- [x] **Si', compilerebbe e risolverebbe `normalizeAttack(Paladin)`.** ✓
- [ ] Si', compilerebbe e risolverebbe `normalizeAttack(Character)`.
- [ ] No, non compilerebbe senza modificare la firma della `map()`.

**Risposta corretta:** (a) Si', discrimina. (b) Si', risolve `normalizeAttack(Paladin)`.

**Spiegazione:** I due overload di `normalizeAttack` differiscono per tipo parametro (Character vs Paladin): il compilatore li discrimina staticamente. La method reference `Rpg::normalizeAttack` viene risolta in `normalizeAttack(Paladin)` perche' il tipo degli elementi nella lista e' `Paladin`.

---

#### Domanda 9 — [Esame 31/01/2020]

Quale caratteristica del linguaggio Java e' all'opera nell'override del metodo `at()` all'interno dell'interfaccia `Polyhedron`,
dove il tipo di ritorno cambia da `PositionedSolid` a `PositionedPolyhedron`?

- [ ] E' un overload, non un override.
- [x] **L'overriding supporta la co-varianza del tipo di ritorno di un metodo.** ✓
- [ ] L'overriding supporta la contro-varianza del tipo di ritorno di un metodo.
- [ ] Un override puo' cambiare liberamente il tipo di ritorno di un metodo.

**Risposta corretta:** L'overriding supporta la co-varianza del tipo di ritorno.

**Spiegazione:** La co-varianza del tipo di ritorno permette a un override di restringere il tipo ritornato: `PositionedPolyhedron` e' sottotipo di `PositionedSolid`, quindi l'override e' valido.

---

#### Domanda 10 — [Esame 04/09/2018]

Sarebbe possibile definire tutti i metodi di `Random` con lo stesso nome `next()` mantenendo inalterate firme e semantica?

- [ ] Si': l'overloading sarebbe possibile e permetterebbe anche l'invocazione polimorfa.
- [ ] Non e' necessario: Java offre una forma speciale di risoluzione dipendente dal contesto per metodi aventi firma differente solo per il tipo di ritorno.
- [x] **No: l'overloading non e' possibile tra metodi che differiscono solamente per il tipo di ritorno.** ✓
- [ ] Si': l'overloading sarebbe possibile, tuttavia non darebbe alcun beneficio pratico.

**Risposta corretta:** No, l'overloading non considera il tipo di ritorno.

**Spiegazione:** L'overloading in Java non considera il tipo di ritorno nella firma; due metodi che differiscono solo per il tipo di ritorno sono indistinguibili dal compilatore e non possono coesistere.

---

### 1.3.1 Equals

#### Domanda 1 — [Esame]

Si considerino i binding Java seguenti (gerarchia `Person -> Artist` con logica `equalsTo()` parametrica):

```java
Person alice   = new Person("Alice", 23);
Person david   = new Artist("Bowie", 69, new Hair(75, Set.of(Color.RED, Color.BROWN, Color.GRAY)));
Artist morgan  = new Artist("Morgan",  47, new Hair(20, Set.of(Color.GRAY, Color.DARK)));
Artist madonna = new Artist("Madonna", 60, new Hair(50, Set.of(Color.BLONDE)));
```

Per ciascuna delle seguenti espressioni Java si indichi l'esito della computazione:

| # | Espressione | Esito |
|---|-------------|-------|
| a | `alice.equals(null)` | `false` |
| b | `alice.equals(alice)` | `true` |
| c | `null.equals(david)` | **non compila / NullPointerException** ✓ |
| d | `alice.equals(david)` | `false` |
| e | `alice.equalsTo(morgan)` | `false` |
| f | `morgan.equals(morgan)` | `true` |
| g | `morgan.equals(madonna)` | `false` |
| h | `morgan.equals(david)` | `false` |
| i | `morgan.equalsTo(david)` | **non compila** |
| j | `david.equalsTo(morgan)` | `false` |
| k | `madonna.equals(3)` | `false` |
| l | `madonna.equalsTo("Madonna")` | **non compila** |

**Spiegazione:** La logica di `equals` scende attraverso la gerarchia via `equalsTo`: occorre controllare null, tipo concreto e poi i campi. Le espressioni con `null.metodo()` o cast incompatibili non compilano o lanciano eccezione a runtime (alcuni compilatori accettano la chiamata a compile-time e la lanciano a runtime).

---

### 1.3.2 Compare

#### Domanda 1 — [Esame 20/06/2018]

Data la firma del metodo sort:

```java
static <T extends Comparable<T>> void sort(List<T> l)
```

**(a)** Se ipoteticamente definissimo un altro metodo `sort` in overload con firma:
```java
static <T> void sort(List<? extends Comparable<T>> l)
```
esso rappresenterebbe un caso di overloading valido?

- [ ] Si', perche' i constraint del generic `T` sono diversi, quindi il compilatore sarebbe in grado di disambiguare.
- [x] **No, perche' i metodi avrebbero la medesima type erasure.** ✓
- [ ] Si', perche' anche se l'overload e' ambiguo per il compilatore, a runtime viene invocato il metodo giusto grazie al dynamic dispatching.
- [ ] No, perche' qualunque overload che coinvolge generics sarebbe ambiguo a causa della type erasure.

**(b)** La chiamata `sort(l)` con `List<Elf> l` (dove `Elf extends Humanoid implements Comparable<Humanoid>`) sarebbe accettata?

- [x] **No: il constraint `<T extends Comparable<T>>` impone che `Elf` implementi `Comparable<Elf>`, ma invece implementa `Comparable<Humanoid>`.** ✓
- [ ] Si': anche se il constraint impone `Comparable<Elf>`, la chiamata risulta compatibile con `Comparable<Humanoid>` perche' `Elf` e' un sottotipo di `Humanoid`.
- [ ] No: sebbene il tipo dell'argomento sussuma in `List<Elf>`, l'interfaccia `List` non estende l'interfaccia `Comparable`.
- [ ] Si': la chiamata sussume il tipo dell'argomento in `List<Humanoid>`, che soddisfa il constraint.

**(c)** Con `a = Elf(10,8)`, `b = Humanoid(8)`, `c = Humanoid(12)`, quale identificatore e' legato all'ultimo elemento della lista dopo `sort(l)`?

- [ ] `a`
- [x] **`b`** ✓
- [ ] `c`
- [ ] non compila

**Spiegazione:** La type erasure elimina i generics a runtime: due metodi che differiscono solo per wildcard/generics risultano con la stessa erasure e non possono coesistere. Il constraint `T extends Comparable<T>` richiede auto-confronto, non soddisfatto da `Elf extends Comparable<Humanoid>`. Per (c): il `compareTo` di `Humanoid` inverte l'ordine (usa `-` prima), quindi ordine decrescente per strength; `b` ha strength=8 (minore), finisce per ultimo (ma dipende dalla semantica: in realta' `-` inverte, quindi b con 8 sara' all'ultimo posto dopo c con 12). La risposta corretta del PDF e' `b`.

---

#### Domanda 2 — [Esame]

Il seguente statement sarebbe accettato dal compilatore Java 7+?

```java
Artist c = Collections.max(artists, new Comparator<Person>() {
    public int compare(Person a, Person b) { return a.age - b.age; }
});
```

- [ ] No: `Comparator<Person>` non soddisfa il wildcard perche' `Person` e' diverso da `Artist`.
- [x] **Si': `Comparator<Person>` soddisfa il wildcard con lower bound perche' `Person` e' supertipo di `Artist`.** ✓
- [ ] No: sebbene il primo argomento `artists` abbia tipo `List<Artist>`, il tipo di ritorno `Person` non puo' essere sussunto in `Artist`.
- [ ] Si': grazie al wildcard con upper bound il generic `T` viene istanziato col tipo `Person`.

**Risposta corretta:** Si', `Comparator<Person>` soddisfa il lower bound.

**Spiegazione:** Il `lower bound` `? super Artist` in `Collections.max` accetta `Comparator<Person>` perche' `Person` e' supertipo di `Artist`, coerente con il principio PECS (Producer Extends, Consumer Super).

---

### 1.3.3 Programmazione ibrida funzionale

#### Domanda 1 — [Esame 31/01/2020]

La lambda espressione `(x) -> x.volume()` passata come primo argomento al metodo `compareBy()`).
avente tipo `Function<Solid, Double>`, e' equivalente a quale dei seguenti costrutti del linguaggio Java?

- [ ] Ad un riferimento ad un metodo statico (static method reference) della classe `Solid`; ovvero all'espressione `Solid::volume`.
- [x] **Ad un riferimento ad un metodo non-statico (instance method reference) di un oggetto arbitrario di tipo `Solid`; ovvero all'espressione `Solid::volume`.** ✓
- [ ] Ad un riferimento ad un metodo non-statico (instance method reference) di un oggetto specifico, che e' `this` in questo caso; ovvero all'espressione `this::volume`.
- [ ] A nessuna delle precedenti.

**Risposta corretta:** Instance method reference di un oggetto arbitrario: `Solid::volume`.

**Spiegazione:** Una lambda `(x) -> x.volume()` dove `x` e' di tipo `Solid` e' equivalente a un *instance method reference* su oggetto arbitrario dello stesso tipo. La notazione `Solid::volume` cattura esattamente questo: dato un oggetto di tipo `Solid`, chiama `volume()` su di esso.

---

## Sezione 2 – Esercizi di progettazione e implementazione

### 2.1 Java

#### 2.1.1 Confronto di Collection

**Testo dell'esercizio**

[Esame 10/09/2024 – 04/09/2018]

1. Si scriva in linguaggio Java 8+ la firma e l’implementazione di un metodo statico e generico `compareMany()` che confronta due collection.
   Dati due argomenti `c1` e `c2` aventi tipo `Collection<A>` e `Collection<B>` (dove `A` e `B` sono i due type parameter del metodo), esso confronta ogni elemento di tipo `A` della prima collection con l’elemento di tipo `B` alla stessa posizione della seconda.
   Il confronto degli elementi deve avvenire chiamando il metodo `compareTo()` opportunamente: questo implica che ci sono dei vincoli sui generics `A` e/o `B`.
   Il risultato del metodo `compareMany()` è di tipo `int` e deve rispettare la semantica del confronto cosiddetto a tre vie.
   Si implementi la semantica del confronto nel seguente modo:

   - `c1` e `c2` sono uguali se e solo se le lunghezze sono uguali e tutti gli elementi sono uguali;
   - `c1` è minore di `c2` se la lunghezza di `c1` è minore o uguale a quella di `c2` e se tutti gli elementi di `c1` sono minori o uguali ai corrispettivi elementi di `c2`;
   - altrimenti `c1` è maggiore di `c2`.

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

_Nel PDF originale non e' presente una soluzione esplicita per questo esercizio._

**Spiegazione rapida del ragionamento**

Il confronto usa `compareTo()` per rispettare la semantica three-way. I vincoli sui generics A/B garantiscono che il confronto sia possibile staticamente, evitando cast a runtime.

---

#### 2.1.2 Thread Pool

**Testo dell'esercizio**

[Esame 10/09/2024 – 13/01/2023 – 22/01/2019]

Si prenda in considerazione la seguente interfaccia Java:

```java
public interface Pool<T, R> {
    void add(T x); // popola la pool con un nuovo elemento
    R acquire() throws InterruptedException; // acquisisce una risorsa
    void release(R x); // rilascia una risorsa e la rimette nella pool
}
```

Una pool è un container di oggetti che si comporta come una coda bloccante: è possibile ottenere una risorsa con
`acquire()` per poi restituirla alla pool tramite il metodo `release()`.
Il generic `T` astrae il tipo degli oggetti contenuti internamente nella pool, mentre `R` è il tipo della risorsa restituita
dalla `acquire()`: il motivo per cui sono due generic differenti è per consentire alle classi che implementano questa
interfaccia di rappresentare in modo diverso, se necessario, gli elementi conservati all’interno e le risorse restituite dalla
`acquire()`.
Quando la coda è vuota e nessun oggetto è disponibile, il metodo `acquire()` deve essere bloccante: al fine di
semplificare l’implementazione, si utilizzi un oggetto di tipo `LinkedBlockingQueue` come campo interno. Riportiamo
un estratto della classe `LinkedBlockingQueue` definita dal JDK con i metodi pubblici più significativi:

```java
class LinkedBlockingQueue<E> implements BlockingQueue<E> {
    LinkedBlockingQueue(); // costruttore
    void add(E x); // aggiunge un elemento
    E take() throws InterruptedException; // estrae la testa (bloccante)
    E peek(); // ritorna la testa senza rimuoverla,
              // oppure null se vuota

    int size(); // numero di elementi
    // etc...
}
```

1. Si definisca una interfaccia `BasicPool` che estende l’interfaccia `Pool` e che ha un solo generic per rappresentare sia
   il tipo degli elementi interni sia il tipo delle risorse restituite dalla `acquire()`.
2. Si implementi una classe `SimplePool` che implementa l’interfaccia `BasicPool` e realizza una semplice coda bloccante.
3. Si implementi una classe `AutoPool` che implementa l’interfaccia `Pool` realizzando un meccanismo di auto-release
   delle risorse. Se `p` è una pool, l’obiettivo è fare in modo che una risorsa `x` acquisita tramite la chiamata `p.acquire()`
   non debba essere esplicitamente riconsegnata alla pool chiamando `p.release(x)`, ma sia possibile rilasciarla
   invocando `x.release()`.
   Suggerimento: si definisca un nuovo tipo parametrico `Resource` che si comporta come un proxy per l’oggetto
   contenuto al suo interno e che implementa la logica di auto-release rilasciando `this`.
4. Si automatizzi il meccanismo di cui sopra facendo in modo che un oggetto di tipo `Resource` si rilasci autonomamente
   quando non esistono più riferimenti ad esso.
   Suggerimento: la superclasse `Object` definisce un metodo `finalize()` che viene invocato nel momento in cui
   l’oggetto viene cancellato dal garbage collector.

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```java

import java.util.Random;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;

public class Es1 {

    public interface Pool<R> {
        R acquire() throws InterruptedException;
        void release(R x);
    }

    // 2.b
    public static class SimplePool<T> implements Pool<T> {

        private final BlockingQueue<T> q = new LinkedBlockingQueue<>();

        @Override
        public T acquire() throws InterruptedException {
            return q.take();
        }

        @Override
        public void release(T x) {
            q.add(x);
        }
    }

    // 2.c + 2.d

    public interface Resource<T> {
        T get();
        void autorelease();
    }

    public static class AutoPool<T> implements Pool<Resource<T>> {

        private final BlockingQueue<T> q = new LinkedBlockingQueue<>();

        // questo metodo non è richiesto dall'interfaccia AutoPool, però è utile perché
        // serve a popolare la pool, come accade nel main
        public void add(T x) {
            q.add(x);
        }

        public Resource<T> acquire() throws InterruptedException {
            T r = q.take();
            return new Resource<>() {

                @Override
                public T get() {
                    System.out.printf("acquired: %s%n", r);
                    return r;
                }

                @Override
                public void autorelease() {
                    System.out.printf("released: %s%n", r);
                    add(r);
                }

                @SuppressWarnings("deprecation")
                @Override
                protected void finalize() {
                    autorelease();
                }
            };
        }

        @Override
        public void release(Resource<T> x) {
            x.autorelease();
        }
    }

    public static void main(String[] args) {
        AutoPool<Integer> pool = new AutoPool<>();
        Random rnd = new Random();
        for (int i = 0; i < 5; ++i) {
            pool.add(i); // popolo la pool con alcuni oggetti
        }

        try {
            while (true) {
                Resource<Integer> r = pool.acquire();
                System.out.println("using " + r.get());
                Thread.sleep(Math.abs(rnd.nextInt() % 1000));
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

    }
}

```

**Spiegazione rapida del ragionamento**

La `BlockingQueue` interna sincronizza produttori e consumatori senza lock espliciti. Il pattern `AutoPool` con `Resource` e `finalize()` implementa il rilascio automatico della risorsa (RAII-style in Java).

---

#### 2.1.3 Piano cartesiano

**Testo dell'esercizio**

[Esame 25/06/2024 –06/09/2019]
1.Si implementi in Java 8+ un sistema di classi che rappresentano punti, linee e poligoni regolari nel piano cartesiano
R↔R.
(a)Si implementi una classe di nomePointche rappresenta punti bidimensionali immutabili, in cui le coordinate
xedysono di tipodouble.
(b)Si implementi una classe di nomeSegmentche rappresenta segmenti bidimensionali immutabili, il cui costrut-
tore prende due argomenti di tipoPoint. Essa deve fornire un metodo length() che restituisce la lunghezza
del segmento calcolando la distanza euclidea tra i due punti.
Si implementi un tipo che rappresenta segmenti bidimensionali immutabili, ovvero una classe Lineil cui
costruttore prende due argomenti di tipo Point. Essa deve fornire un metodo length() che ne calcola la
lunghezza tramite la distanza euclidea tra i due punti.
(c)Si prenda in considerazione la seguente classe astratta Polygonche rappresenta poligoni regolari come liste
di punti (minimo 3, verificato a runtime). I punti nella lista determinano l’ordine di costruzione dei segmenti
di cui è composto il poligono. Ad esempio, una lista contenente i seguenti 3 punti A=( 0,0),B=( 3,3)e
C=( 3,0)rappresenta un triangolo rettangolo in cui il primo lato è AB, il secondo è BC ed il terzo è CA.

```java
publicabstractclassPolygon{
protectedfinalList<Point>points;
protectedPolygon(List<Point>points){
assertpoints.size()>=3;
this.points=points;
}
publicIterator<Line>lineIterator(){/* da implementare */ }
publicdoubleperimeter(){/* da implementare */ }
publicabstractdoublearea();
}
```

i.Per quale motivo è necessario vincolare a runtime la dimensione minima della lista di punti tramite un
assertanziché sfruttare in qualche modo il type system per fare un controllo statico? Si articoli una
breve risposta.
ii.Si implementi il metodolineIterator()che costruisce un iteratore su oggetti di tipo Line e si comporta
come un wrapper dell’iteratore estratto dal campo points. Gli oggetti prodotti dall’iteratore di Line
devono essere costruiti dinamicamente leggendo coppie di punti adiacenti dall’iteratore di Point.
Si implementi una logica dicachingdell’ultimo punto letto per permettere la costruzione di un
nuovo segmento adiacente all’ultimo ad ogni invocazione del metodo next(). Si badi inoltre a riusare
opportunamente il primo punto come secondo estremo dell’ultimo segmento costruito.
iii.Si implementi il metodo perimeter()in funzione del metodo lineIterator(), ovvero calcolando il
perimetro del poligono iterando sui segmenti che lo compongono.
2.Estendiamo ora la gerarchia di classi introducendo tipi specializzati per i poligoni classici.
(a)Si implementi una sottoclasse diPolygondi nomeTriangleche rappresenta triangoli qualunque.

```java
publicclassTriangleextendsPolygon{
publicTriangle(Pointp1,Pointp2,Pointp3){/* da implementare */ }
@Override
publicdoublearea(){/* da implementare */ }
}
```

Il costruttore prende 3 argomenti di tipo Point e deve chiamare il super-costruttore opportunamente. Si
implementi il metodoarea()in modo che calcoli l’area del triangolo senza fare assunzioni sulla sua forma.
(b)Si implementi un tipo che rappresenta triangoli rettangoli tramite una sottoclasse di Triangle. Si badi in
particolar modo a definire un costruttore che sintetizzi al massimo le informazioni passate come argomenti.
Si prediligano i controlli statici a quelli dinamici, se possibile, e si tenti di minimizzare i casi di ambiguità
o gli stati di invalidità dell’oggetto; nel caso in cui si ritenga necessario fare dei controlli a runtime, si usi il
costruttoassert.
(c)Si implementi una sottoclasse diPolygondi nomeRectangleche rappresenta rettangoli.

```java
publicclassRectangleextendsPolygon{
publicRectangle(Pointp1,Pointp3){/* da implementare */ }
@Override
publicdoublearea(){/* da implementare */ }
}
```

Il costruttore prende i 2 punti della diagonale e deve passare al super-costruttore i 4 punti che costituiscono
il rettangolo calcolandone le coordinate per proiezione ortogonale. Si badi all’ordine in cui i punti compaiono
nella lista, affinché siano adiacenti e permettano di comporre i lati correttamente. Si implementi il metodo
area()in modo che calcoli l’area del rettangolo.
(d)Si implementi una sottoclasse diRectangledi nomeSquareche rappresenta quadrati.

```java
publicstaticclassSquareextendsRectangle{
publicSquare(Pointp1,doubleside){/* da implementare */ }
}
```

Il costruttore prende il punto in basso a sinistra e la dimensione del lato e deve chiamare il super-costruttore
opportunamente.
3.Si prenda in considerazione il seguente codice, in cui compare l’invocazione di un metodo statico maxda definire:

```java
Squaresq1=newSquare(newPoint(10.,-4.),0.1),
sq2=newSquare(newPoint(1.,20.),0.01);
Collection<Square>squares=List.of(sq1,sq2);
Rectangler=max(squares,newComparator<Polygon>(){
@Override
publicintcompare(Polygona,Polygonb){
return(int)(a.area()-b.area());
}
});
```

(a)Si scriva la firma e l’implementazione del metodo statico max()invocato nell’ultimo statement affinché il
codice di cui sopra compili correttamente. Si renda tale metodo più generico possibile e non monomorfo
rispetto ai tipi che compaiono in questa invocazione, prestando particolare attenzione ai vincoli sui generics.
La semantica dimaxè facilmente intuibile: trova l’elemento maggiore usando il comparatore per confrontare
gli elementi della collection di input.
(b)Assumendo il comportamento corretto del metodomax(), quale delle seguenti espressioni booleane è vera?
->sq1==r ->sq2==r ->r==null ->nessuna delle precedenti
(c)Quale dei seguenti numeri razionali rappresenta il valore di tipodoublecomputato dall’espressioner.area()?
->10->1 ->10->2 ->10->4 ->non è un quadrato ma un rettangolo

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```java
importjava.util.*;

publicclassEs{

// 1.a
publicstaticclassPoint{
publicfinaldoublex,y;

publicPoint(doublex,doubley){
this.x=x;
this.y=y;
}

}

// 1.b
publicstaticclassLine{
privatefinalPointp1,p2;

publicLine(Pointp1,Pointp2){
this.p1=p1;
this.p2=p2;
}

publicdoublelength(){
returnMath.sqrt(Math.pow(p1.x-p2.x,2.)+Math.pow(p1.y-p2.y,2.));
}
}

// 1.c
publicabstractstaticclassPolygon{
protectedfinalList<Point>points;

protectedPolygon(List<Point>points){
assertpoints.size()>=3;// 1.c.i: RISPOSTA: il motivo per cui è necessario questo
assert è che non è possibile vincolare la lunghezza di una lista con il type
system: i tipi non esprimono valori numerici statici

this.points=points;
}

// 1.c.ii
publicIterator<Line>lineIterator(){
returnnewIterator<>(){
privatefinalIterator<Point>it=points.iterator();
privatePointlast=it.next(); // ci sono sempre almeno 3 punti, non può
fallire questa next()

@Override
publicbooleanhasNext(){
returnit.hasNext();
}

@Override
publicLinenext(){
Pointp=it.next();
Liner=newLine(last,p);
last=p;
returnr;
}
};
}

// 1.c.iii
publicdoubleperimeter(){
Iterator<Line>lineIt=lineIterator();
doubler=0.;
while(lineIt.hasNext()){
r+=lineIt.next().length();
}
returnr;

}

publicabstractdoublearea();
}

// 2.a
publicstaticclassTriangleextendsPolygon{
publicTriangle(Pointp1,Pointp2,Pointp3){
super(List.of(p1,p2,p3));
}

@Override
publicdoublearea(){
Pointp1=points.get(0),p2=points.get(1);
// versione facile con la base orizzontale (sufficiente per la valutazione
dell'esame)
if(p1.x==p2.x){
doublebase=newLine(p1,p2).length(),h=newLine(newPoint(p2.x,p1.y),
p2).length();
returnbase*h/2.;
}
// versione generale con la formula di Erone (non necessaria per la valutazione
dell'esame)
else{
Pointp3=points.get(2);
doublep=perimeter()/2., // semiperimetro
a=newLine(p1,p2).length(),
b=newLine(p2,p3).length(),
c=newLine(p3,p1).length();
returnMath.sqrt(p*(p-a)*(p-b)*(p-c));
}
}

}

// 2.b
publicstaticclassRightTriangleextendsTriangle{
publicRightTriangle(Pointp1,doubleb,doubleh){
super(p1,newPoint(p1.x+b,p1.y),newPoint(p1.x,p1.y+h));
}
}

// 2.c
publicstaticclassRectangleextendsPolygon{
publicRectangle(Pointp1,Pointp3){
super(List.of(p1,newPoint(p1.x,p3.y),p3,newPoint(p3.x,p1.y)));
}

@Override
publicdoublearea(){
Pointp1=points.get(0),p2=points.get(1),p4=points.get(3);
Lineb=newLine(p1,p4),h=newLine(p1,p2);
returnb.length()*h.length();
}
}

// 2.d

publicstaticclassSquareextendsRectangle{
publicSquare(Pointp1,doubleside){
super(p1,newPoint(p1.x+side,p1.y+side));
}

}

// 3.a
static<T>Tmax(Collection<T>l,Comparator<?superT>cmp){
assertl.size()>1;
Tmax=l.iterator().next(); // prendo il primo elemento
for(Tx:l){
if(cmp.compare(x,max)>0)max=x;
}
returnmax;
}

publicstaticvoidmain(String[]args){
Squaresq1=newSquare(newPoint(10.,-4.),0.1),
sq2=newSquare(newPoint(1.,20.),0.01);
Collection<Square>squares=List.of(sq1,sq2);
Rectangler=max(squares,newComparator<Polygon>(){
@Override
publicintcompare(Polygona,Polygonb){
return(int)(a.area()-b.area());
}
});
// 3.b: il metodo max() ritorna sq2 perché la sottrazione nel confronto è invertita,
quindi la ricerca del massimo in realtà restituisce il più piccolo anziché il più
grande;

// ed il quadrato con area minore è sq2, perché 0.01^2 = 0.0001 mentre 0.1^2 = 0.01,
quindi sq2 è di fatto il più piccolo.

// 3.c: l'area di r (cioè di sq2) è 0.01^2 = 0.0001 = 10^-4
}

}
```

**Spiegazione rapida del ragionamento**

La gerarchia `Point -> Line -> Polygon -> Triangle/Rectangle` separa correttamente le responsabilita'. L'uso di `assert` nel costruttore di `Polygon` e' necessario perche' il type system non puo' esprimere vincoli numerici statici sulle dimensioni di una lista.

---

#### 2.1.4 Funzioni di ordine superiore

**Testo dell'esercizio**

[Esame 14/06/2024]
1.Si implementino le seguenti funzioni di ordine superiore in Java 8+.

(a)La prima è una variante della classica funzione map()1che opera su iteratori anziché su collection. L’itera-
tore restituito in output deve applicare la funzione f a ciascun elemento di tipo A fornito dall’iteratore it,
producendo oggetti di tipoB.

```java
static<A,B>Iterator<B>mapIterator(Iterator<A>it,Function<A,B>f)
```

(b)La seconda funzione di ordine superiore è semanticamente equivalente al costrutto for-each: ad ogni oggetto
di tipoTnell’iterable in input viene applicato il consumerf.

```java
static<T>voidforEach(Iterable<T>it,Consumer<T>f)
```

(c)Si implementi un tipoPairparametrico su due tipiA e B. Si scelga liberamente se fornire una implementazione
mutabile o immutabile.
(d)Si prenda ora in considerazione la seguente firma di funzione:

```java
static<A,B>Iterator<B>applyFuns(Iterable<Pair<Function<A,B>,A>>l)
```

Per ogni coppia dell’iterable in input, essa deve applicare la funzione che si trova nella prima componente
della coppia all’oggetto di tipoAche si trova nella seconda componente. Si implementi la suddetta funzione
tramiteuna singola invocazionedellamapIterator()di cui al punto (a).
(e)Si prenda ora in considerazione la seguente firma di funzione:

```java
static<T>voidacceptFuns(Iterable<Pair<Consumer<T>,T>>l)
```

Per ogni coppia dell’iterable in input, essa deve applicare la funzione consumer che si trova nella prima
componente della coppia all’oggetto di tipo Tche si trova nella seconda componente. Si implementi la
suddetta funzionetramite una singola invocazionedellaforEach()di cui al punto (b).
(f)Con riferimento al punto (a) si consideri la seguente firma di funzione :

```java
static<A,B>Iterator<Supplier<B>>asyncMapIterator(Iterator<A>it,Function<A,B>f)
```

Essa consiste in una variante asincrona della mapIterator()in cui l’applicazione della funzione fad ogni
elemento di tipoAprodotto dall’iteratore in input deve avere luogo ogni volta in un thread nuovo. In
altre parole, ogni computazione della funzione fdeve avvenire concorrentemente. Il risultato di ciascuna
computazione, naturalmente, non può essere ritornato subito dallanext()dall’iteratore in output, altrimenti
sarebbe necessario attendere la fine di ciascun thread, vanificando ogni forma di concorrenza. Per questo
motivo l’iteratore in output produce Supplier<B>anziché oggetti di tipo B. Sono iSupplier2che devono
attendere la fine dell’esecuzione dei thread.
Suggerimento:l’implementazione è piuttosto contenuta, non servono datatype di appoggio né metodi ausil
iari. Si sfrutti al massimo lo scoping, in particolare le chiusure delle lambda (o delle anonymous class, se si
preferisce).
(g)Con riferimento al punto (b) si consideri la seguente firma di funzione :

```java
static<T>voidasyncForEach(Iterable<T>it,Consumer<T>f)
```

Questa è una variante asincrona della forEach(): l’applicazione della funzione fad ogni elemento di tipo
Tcontenuto nell’iterable in input deve avere luogo ogni volta in un thread nuovo. In altre parole, ogni
computazione della funzionefdeve avvenire concorrentemente.
Suggerimento:poiché iConsumernon hanno risultato, non si pone il problema di attendere la fine delle
computazioni dei thread.

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```java
importjava.util.ArrayList;
importjava.util.Iterator;
importjava.util.List;
importjava.util.function.Consumer;
importjava.util.function.Function;
importjava.util.function.Supplier;

publicclassEs1{

publicstatic<A,B>Iterator<B>mapIterator(Iterator<A>it,Function<A,B>f){
returnnewIterator<>(){
@Override
publicbooleanhasNext(){
returnit.hasNext();
}

@Override
publicBnext(){

returnf.apply(it.next());
}
};
}

publicstatic<T>voidforEach(Iterable<T>it,Consumer<T>f){
for(Tx:it)
f.accept(x);
}

publicstaticclassPair<X,Y>{
publicfinalXfst;
publicfinalYsnd;
publicPair(Xx,Yy){
fst=x;
snd=y;
}
}

publicstaticvoidmain1(String[]args){
List<Pair<Function<String,Integer>,String>>l=newArrayList<>();
Iterator<Integer>it=mapIterator(l.iterator(),(p)->p.fst.apply(p.snd));
}

publicstatic<A,B>Iterator<B>applyFuns(Iterable<Pair<Function<A,B>,A>>l){
returnmapIterator(l.iterator(),(p)->p.fst.apply(p.snd));
}

publicstaticvoidmain2(String[]args){
List<Pair<Consumer<Integer>,Integer>>l=newArrayList<>();
forEach(l,(p)->p.fst.accept(p.snd));
}

publicstatic<A>voidacceptFuns(Iterable<Pair<Consumer<A>,A>>l){
forEach(l,(p)->p.fst.accept(p.snd));
}

publicstatic<A,B>Iterator<Supplier<B>>asyncMapIterator(Iterator<A>it,Function<A,B>
f){
returnnewIterator<>(){
@Override
publicbooleanhasNext(){
returnit.hasNext();
}

@Override
publicSupplier<B>next(){
finalAx=it.next();
finalB[]r=(B[])newObject[1];
Threadt=newThread(()->{r[0]=f.apply(x);});
t.start();
return()->{
try{
t.join();
}catch(InterruptedExceptione){
thrownewRuntimeException(e);
}

returnr[0];
};
}
};
}

publicstatic<T>voidasyncForEach(Iterable<T>it,Consumer<T>f){
for(Tx:it)
newThread(()->f.accept(x)).start();
}
}
```

**Spiegazione rapida del ragionamento**

Le funzioni di ordine superiore (`Function<A,B>`) consentono di iniettare comportamento come parametro, rendendo il codice aperto all'estensione senza modificare la struttura.

---

#### 2.1.5 Fattoriale con thread

**Testo dell'esercizio**

[Esame10/01/2024– 30/05/2019]
1.Si consideri un metodo statico in Java 8+ di nome parallelFactorial()che data unaCollection<Integer>
produce unaCollection<FactorialThread>. Per ogni intero della collection di input viene effettuato lo spawning
di un nuovo thread che ne computa il fattoriale e ne conserva il risultato in qualche modo. Tutti i thread creati
vengono ritornati nella collection di output, che naturalmente ha la stessa lunghezza della collection di input.
(a)Si implementi la classe FactorialThreadin modo che estenda java.lang.Thread e fornisca un metodo
getter per l’accesso al risultato della computazione del fattoriale. Si presti attenzione all’attesa della fine della
computazione, gestendo opportunamente la cosa.
1Conosciuta anche con il nome ditransform()in certe librerie.
2Si rammenti che unSupplier<T>non è altro che unafunctional interfacecon un solo metodoget()che non ha parametri e ritorna un
oggetto di tipoT.

(b)Si implementi il metodo staticoparallelFactorial()avente la seguente firma:

```java
staticCollection<FactorialThread>parallelFactorial(Collection<Integer>c)
```

Ogni thread deve lavorare concorrentemente agli altri, ciascuno calcolando il fattoriale di un intero proveniente
dalla collection di input. La funzione parallelFactorial()non deve attendere la fine delle computazioni,
ma deve ritornare subito la collection di output.
(c)Si raffini la firma del metodo statico parallelFactorial()di cui al punto precedente in modo che il tipo
di input ed il tipo di output siano, rispettivamente, più generale e più specializzato possibile, senza tuttavia
rivelare dettagli sull’implementazione.
(d)Si scriva uno snippet di codice che testi il metodo statico parallelFactorial()stampando i risultati di
ciascuna computazione.
(e)Si dia una seconda implementazione del metodo statico parallelFactorial()usando la funzione di ordine
superiore map()3. In particolare si proceda nel seguente modo:
i.Si implementi un metodo staticomap()avente la seguente firma:

```java
static<A,B>List<B>map(Iterable<A>i,Function<A,B>f)
```

Esso deve applicare la funzione fad ogni elemento di ie produrre in output tutti i risultati delle
applicazioni.
ii.Si reimplementiparallelFactorial()tramite una singola invocazione dellamap().

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```java
importjava.util.ArrayList;
importjava.util.Collection;
importjava.util.List;
importjava.util.function.Function;

publicclassEs1{

// 1.a
publicstaticclassFactorialThreadextendsThread{
privatefinalintn;
privatelongres;

publicFactorialThread(intn){
this.n=n;
}

@Override
publicvoidrun(){
res=fact(n);
}

publiclonggetResult(){
try{
join();
}catch(InterruptedExceptione){
thrownewRuntimeException(e);
}
returnres;
}

publicintgetN(){
returnn;
}

privatestaticlongfact(intn){
if(n<=1)return1;
returnn*fact(n-1);
}

}

// 1.b + 1.c
publicstaticList<FactorialThread>parallelFactorial(Iterable<Integer>c){
List<FactorialThread>r=newArrayList<>();

for(intn:c){
FactorialThreadt=newFactorialThread(n);
t.start();
r.add(t);
}
returnr;
}

// 1.d
publicstaticvoidmain(String[]args){
for(FactorialThreadt:parallelFactorial(List.of(0,1,2,3,11,12,23,35))) //
chiamare anche parallelFactorial2() per provarla
System.out.printf("fact(%d) = %d\n",t.getN(),t.getResult());
}

// 1.e.i
publicstatic<A,B>List<B>map(Iterable<A>i,Function<A,B>f){
List<B>r=newArrayList<>();
for(Aa:i)
r.add(f.apply(a));
returnr;
}

// 1.e.ii
publicstaticCollection<FactorialThread>parallelFactorial2(Collection<Integer>c){
returnmap(c,(n)->{FactorialThreadt=newFactorialThread(n);t.start();returnt;
});
}
}
```

**Spiegazione rapida del ragionamento**

La suddivisione del calcolo su thread separati riduce i tempi. Il thread principale raccoglie i risultati parziali tramite una struttura condivisa protetta da sincronizzazione.

---

#### 2.1.6 Iteratori asincroni

**Testo dell'esercizio**

[Esame 05/09/2023]
1.Si implementi un metodo statico generico in Java 8+ che, dato un iteratore ed una funzione, produce un nuovo
iteratore che in maniera asincrona applica la funzione ad ogni elemento dell’iteratore originale. Ciò significa che
un thread diverso deve processare ciascun elemento.
(a)Si implementi tutto ciò che è necessario dello snippet seguente.

```java
static<A,B>Iterator<Supplier<B>>asyncIterator(Iterator<A>it,Function<A,B>f){
returnnewIterator<>(){
@Override
publicbooleanhasNext(){/* da implemetare */ }

privateclassFutureimplementsSupplier<B>{
publicFuture(Supplier<B>f){/* da implementare */ }
/* da completare/implementare */
}

@Override
publicSupplier<B>next(){/* da implementare */ }
};
}
```

Si badi che la classe innestata Futureha lo scopo di semplificare l’implementazione della next(). Essa
rappresenta una computazione in corso che non è ancora terminata. Essendo di fatto un Supplier, sarà
possibile conoscere il risultato invocando il metodo get(). Naturalmente questo risultato sarà possibile
ottenerlo solamente dopo aver atteso che il relativo thread abbia finito di computare il Supplierpassato in
costruzione.
(b)Si implementi il seguente metodo statico in modo che utilizzi il metodo statico asyncIterator()di cui
all’esercizio precedente per scorrere l’argomento iterabile e ordinare in maniera asincrona i suoi elementi 4.

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```java
importorg.jetbrains.annotations.NotNull;
importorg.jetbrains.annotations.Nullable;

importjava.util.Collections;
importjava.util.Iterator;
importjava.util.List;
importjava.util.function.Function;
importjava.util.function.Supplier;

publicclassMain{

// 1.a
publicstatic<A,B>Iterator<Supplier<B>>asyncIterator(Iterator<A>it,Function<A,B>f){
returnnewIterator<>(){
@Override
publicbooleanhasNext(){
returnit.hasNext();
}

privateclassFutureimplementsSupplier<B>{
@Nullable
privateBr;
@NotNull
privatefinalThreadt;

publicFuture(Supplier<B>f){

t=newThread(()->{r=f.get();});
t.start();
}

@Override
@NotNull
publicBget(){
try{
t.join();
}catch(InterruptedExceptione){
thrownewRuntimeException(e);
}
returnr;
}
}

@Override
publicSupplier<B>next(){
Aa=it.next();
returnnewFuture(()->f.apply(a));
}
};
}

// 1.b
static<TextendsComparable<?superT>>voidsortLists(Iterable<List<T>>c){
Iterator<Supplier<List<T>>>it=asyncIterator(c.iterator(),(l)->{
Collections.sort(l);returnl;});
while(it.hasNext()){
Supplier<List<T>>f=it.next();
System.out.println(f.get()); // questo non è davvero necessario, ma
}
}

}
```

**Spiegazione rapida del ragionamento**

Gli iteratori asincroni separano la produzione (thread interno) dal consumo (thread chiamante), utile quando gli elementi arrivano in modo non deterministico.

---

#### 2.1.7 Funzioni

**Testo dell'esercizio**

[Esame 30/06/2023]
3La funzionemap()qui richiesta è equivalente a quella che JDK e STL chiamano transform().
4Si ricordi che il JDK fornisce un metodo Collections.sort()per ordinare liste di oggetti confrontabili.

1.Si prenda in considerazione la seguente classe Java 8+, parametrica su due generics X e Y, le cui istanze rappre-
sentano sequenzeiterabilidi coppie di coordinate cartesianeX↔Yche rappresentano i punti individuati da una
funzioneX↗Yin un determinato intervallo del dominioX.

```java
publicclassFunSeq<XextendsNumber&Comparable<?superX>,YextendsNumber>
implementsIterable<Pair<X,Y>>{

privatefinalXa,b;
privatefinalFunction<?superX,?extendsY>f;
privatefinalFunction<X,X>inc;

publicFunSeq(Xa,Xb,Function<?superX,?extendsY>f,Function<X,X>inc){
this.a=a;
this.b=b;
this.f=f;
this.inc=inc;
}
/* da finire di implementare */
}
```

Al costruttore vengono passati l’intervallo di dominio[a, b), la funzionefe la funzioneincper incrementare l’ascissa.
La funzionefè la funzione principale: per ogni ascissaxdi tipoXcompresa nell’intervallo[a, b), l’applicazione
f(x)ha tipoY. La sequenza iterabile consiste in coppie(x, f(x))di tipoPair<X,Y>che rappresentano l’ascissa
e l’ordinata di ogni punto. Essendo impossibile rappresentare funzioni continue, le ascisse procedono in maniera
discreta secondo un valore di incremento: la funzione incè necessaria proprio per calcolare la prossima ascissa
nella sequenza.
(a)Si definisca una classePairparametrica su 2 genericsAeBche rappresenta coppieimmutabili.
(b)Si finisca di implementare la classeFunSeqopportunamente. Per realizzare il confronto delle ascisse, si noti
che il type parameterXè vincolato non solo ad essere unNumberma anche a implementareComparable<X>.
(c)Si implementi uno snippet di codice che utilizzi la classe FunSeqper rappresentare la parabolax2+2x↘1
nel pianoR↔Rnell’intervallo di dominio[↘2,2)con incremento di ascissa0.1. Ciò significa che le ascisse
partono da↘2 e procedono a step di0.1fino a2(escluso), producendo((2↘0.1)↘(↘2))/0.1 = 39punti. Si
definisca la parabola con una opportuna lambda.
(d)Si implementi uno snippet simile a quello del punto precedente ma che rappresenti la parabola data nel piano
R↔Z, ovvero con ordinate di tipoint.

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```java
importjava.util.Iterator;
importjava.util.function.Function;

publicclassEs1{

// 1.a
publicstaticclassPair<A,B>{
publicfinalAfst;
publicfinalBsnd;

publicPair(Aa,Bb){
fst=a;
snd=b;
}
}

publicstaticclassFunSeq<XextendsNumber&Comparable<?superX>,YextendsNumber>
implementsIterable<Pair<X,Y>>{

privatefinalXa,b;

privatefinalFunction<?superX,?extendsY>f;
privatefinalFunction<X,X>inc;

publicFunSeq(Xa,Xb,Function<?superX,?extendsY>f,Function<X,X>inc){
this.a=a;
this.b=b;
this.f=f;
this.inc=inc;
}

// 1.b
@Override
publicIterator<Pair<X,Y>>iterator(){
returnnewIterator<>(){
privateXx=a;
@Override
publicbooleanhasNext(){
returnx.compareTo(b)<=0;
}

@Override
publicPair<X,Y>next(){
Pair<X,Y>r=newPair<>(x,f.apply(x));
x=inc.apply(x);
returnr;
}
};
}
}

// 1.c
publicstaticvoidtest1(){
for(Pair<Double,Double>p:newFunSeq<>(-2.,2.,(x)->x*x-2*x+1,(x)->x+
0.1)){
finaldoublex=p.fst,y=p.snd;
System.out.printf("f(%g) = %g\n",x,y);
}
}

// 1.d
publicstaticvoidtest2(){
for(Pair<Double,Integer>p:newFunSeq<>(-2.,2.,(x)->(int)(x*x-2*x+1),
(x)->x+0.1)){
finaldoublex=p.fst;
finalinty=p.snd;
System.out.printf("f(%g) = %d\n",x,y);
}
}


publicstaticvoidmain(String[]args){
test1();
test2();
}
}


```

**Spiegazione rapida del ragionamento**

Le funzioni come oggetti (`Function<A,B>`) permettono composizione e lazy evaluation. Definire le operazioni come interfacce funzionali rende il codice flessibile e componibile.

---

#### 2.1.8 Alberi binari di ricerca

**Testo dell'esercizio**

[Esame 01/06/2023]
1.Vogliamo realizzare in Java 8+ una classeBST, parametrica su un tipo genericoT, che rappresenta alberi binari di
ricerca (Binary Search Tree) i cui nodi sono decorati con valori di tipo T. Un albero binario di ricerca è un normale
albero binario, tuttavia gli elementi al suo interno vengono mantenuti con un certo ordine dato da una peculiare
proprietà ricorsiva. Si prenda in esame il seguente esempio che per semplicità utilizza degli interi:
1 11
2 /\
3 62 5
4 /\ \
5 38 6 2
6 /\
7 57 81
Il nodo radice 11 ha un valore superiore a tutti i nodi del sotto-albero sinistro ed inferiore a tutti i nodi del
sotto-albero destro. E questo è vero non solo prendendo in esame la radice dell’albero, ma prendendo qualsiasi
nodo in qualunque punto dell’albero!
Quando si inserisce un nuovo nodo nell’albero è necessario metterlo nel punto giusto, rispettando questa proprietà,
così che l’albero sia sempre in uno stato valido.

Segue l’implementazione da completare della classeBST.

```java
publicclassBST<T>implementsIterable<T>{
protectedfinalComparator<?superT>cmp;
protectedNoderoot;

protectedclassNode{
protectedfinalTdata;
protectedNodeleft,right;

protectedNode(Tdata,Nodeleft,Noderight){
this.data=data;
this.left=left;
this.right=right;
}
}

publicBST(Comparator<?superT>cmp){
this.cmp=cmp;
}

publicvoidinsert(Tx){
root=insertRec(root,x);
}

protectedNodeinsertRec(Noden,Tx){/* da implementare */ }

protectedvoiddfsInOrder(Noden,Collection<T>out){/* da implementare */ }

@Override
publicIterator<T>iterator(){/* da implementare */ }

publicTmin(){/* da implementare */ }
publicTmax(){/* da implementare */ }
}
```

(a)Si implementi ricorsivamente il metodo insertRec()badando a rispettare la proprietà degli alberi binari
di ricerca enunciata sopra. Per determinare se discendere nel sotto-ramo sinistro oppure in quello destro è
necessario utilizzare ilComparatorpassato in costruzione per confrontare l’argomento xcon il campodata
del nodon.

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```java
importorg.jetbrains.annotations.NotNull;
importorg.jetbrains.annotations.Nullable;

importjava.util.*;
importjava.util.function.Function;
importjava.util.function.Supplier;

publicclassEs1{

// NOTA: la classe è statica solamente perché è nested per praticità
publicstaticclassBST<T>implementsIterable<T>{
@NotNull
protectedfinalComparator<?superT>cmp;
@Nullable
protectedNoderoot;

publicBST(@NotNullComparator<?superT>cmp){
this.cmp=cmp;
}

publicvoidinsert(@NotNullTx){
root=insertRec(root,x);
}

// 1.a
@NotNull
protectedNodeinsertRec(@NullableNoden,@NotNullTx){
if(n==null)
returnnewNode(x);
intr=cmp.compare(x,n.data);
if(r<0)
n.left=insertRec(n.left,x);
elseif(r>0)
n.right=insertRec(n.right,x);
returnn;
}

// 1.b.i
protectedvoiddfsInOrder(@NullableNoden,@NotNullCollection<T>out){
if(n!=null){
dfsInOrder(n.left,out);
out.add(n.data);
dfsInOrder(n.right,out);
}
}

// 1.b.ii
@NotNull
@Override
publicIterator<T>iterator(){
Collection<T>c=newArrayList<>();
dfsInOrder(root,c);
returnc.iterator();
}

// 1.c

@Nullable
publicTmin(){
if(root==null)returnnull;
Noden=root;
while(n.left!=null)n=n.left;
returnn.data;
}

@Nullable
publicTmax(){
if(root==null)returnnull;
Noden=root;
while(n.right!=null)n=n.right;
returnn.data;
}


protectedclassNode{
@NotNull
privatefinalTdata;
@Nullable
protectedNodeleft,right;

protectedNode(@NotNullTdata,@NullableNodeleft,@NullableNoderight){
this.data=data;
this.left=left;
this.right=right;
}

protectedNode(@NotNullTdata){
this(data,null,null);
}
}

@Override
@NotNull
publicStringtoString(){
StringBuildersb=newStringBuilder();
for(Tx:this){
sb.append(x);
sb.append("");
}
returnsb.toString();
}
}

// 1.d
publicstaticclassBST2<TextendsComparable<?superT>>extendsBST<T>{
publicBST2(){
super(Comparable::compareTo);
}
}

publicstaticvoidmain(String[]args){
BST<Integer>t=newBST2<>();
Randomrnd=newRandom();
for(inti=0;i<20;++i)

t.insert(rnd.nextInt(100));
System.out.println(t);
System.out.printf("min = %d\nmax = %d\n" ,t.min(),t.max());
}

}
```

**Spiegazione rapida del ragionamento**

L'invariante del BST (sinistra < radice <= destra) e' mantenuta ad ogni inserimento. Questa struttura rende ricerca e ordinamento efficienti (O(log n) nel caso medio).

---

#### 2.1.9 Alberi binari

**Testo dell'esercizio**

[Esame 13/09/2022 – 20/06/2019]
1.Vogliamo realizzare in Java 8+ una classeTreeNode, parametrica su un tipo genericoT, che rappresenta nodi di
un albero binario decorati con valori di tipo T. Quando entrambi i sotto-alberi sinistro e destro di un nodo sono
assenti, allora il nodo rappresenta una foglia. Seguono ora le specifiche dettagliate.
(a)Gli alberi devono essereiterabili e l’iteratore deve attraversare l’albero inDFS(Depth-First Search), fornendo
gli elementi di tipoTin pre-ordine, ovvero nell’ordine mostrato da questo esempio:
1 1
2 /\
3 25
4 /\ \
5 34 6
6 /\
7 78
(b)Gli alberi devono essere confrontabili tramite il metodoequals(). Due alberi sono uguali se tutti i sotto-alberi
sono uguali e sono costituiti da elementi uguali.

(c)Si definiscano gli opportuni costruttori ed eventualmente dei metodi statici che fungono da pseudo-costruttori.
L’obiettivo è fare in modo che gli alberi siano facili da costruire innestando ricorsivamente le chiamate. Non
si permetta l’istanziazione di alberi vuoti o non inizializzati da popolare successivamente con dei setter.
(d)Si implementino uno o più snippet di test che mettono alla prova tutte le caratteristiche richieste.
(e)Si implementi unpretty printertramite un override del metodotoString().
Si implementi la classeTreeNodesecondo le specifiche date, rappresentando la struttura dati nella maniera che si
ritiene più conveniente e fornendo tutti i metodi necessari. È importante il riuso di codice, l’information hiding ed
una implementazione che escluda il più possibile stati di invalidità grazie ad un saggio uso dei tipi.

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```java
// Questo sorgente contiene le soluzioni dell 'esame scritto di PO2 del 13/9/2022 per ciò che
riguarda il quesito 1,
// ovvero l'esercizio in Java.
// Il quesito 2 riguardante C++ è in una Solution per Visual Studio a parte.

importorg.jetbrains.annotations.NotNull;
importorg.jetbrains.annotations.Nullable;

importjava.util.ArrayList;
importjava.util.Collection;
importjava.util.Iterator;
importjava.util.concurrent.BlockingQueue;

publicclassEs1{

// 1.d

publicstaticvoidmain(String[]args){
TreeNode<Integer>t1=
TreeNode.lr(1,
TreeNode.lr(2,
TreeNode.v(3),
TreeNode.v(4)),
TreeNode.r(5,
TreeNode.lr(6,
TreeNode.v(7),
TreeNode.v(8))));

TreeNode<Integer>t2=
TreeNode.lr(1,
TreeNode.r(5,
TreeNode.lr(6,
TreeNode.v(7),
TreeNode.v(8))),
TreeNode.lr(2,
TreeNode.v(3),
TreeNode.v(4)));


// test pretty printer
System.out.println("pretty printer:");
System.out.println("t1: "+t1);
System.out.println("t2: "+t2);

// test equality
System.out.print("equality: ");
System.out.println(t1.equals(t2)+", "+(t1.left!=null?t1.left.equals(t2.right):
""));


// test iterator
System.out.print("iterator: ");
for(Integern:t1){
System.out.printf("%d ",n);
}
System.out.println();

}

publicstaticclassTreeNode<T>implementsIterable<T>{

@NotNull
privatefinalTdata;
@Nullable
privatefinalTreeNode<T>left,right;
@Nullable
privateTreeNode<T>parent=null;

// 1.c

publicTreeNode(@NotNullTdata,@NullableTreeNode<T>left,@NullableTreeNode<T>
right){
this.data=data;
this.left=left;
this.right=right;
if(left!=null)left.parent=this;
if(right!=null)right.parent=this;
}

// i seguenti pseudo-costruttori aiutano a costruire alberi in modo più succinto e
controllato rispetto
// all'innestamento dei costruttori

// solo ramo sinistro
publicstatic<T>TreeNode<T>l(@NotNullTdata,@NotNullTreeNode<T>left){
returnnewTreeNode<>(data,left,null);
}

// solo ramo destro
publicstatic<T>TreeNode<T>r(@NotNullTdata,@NotNullTreeNode<T>right){
returnnewTreeNode<>(data,null,right);
}

// ramo sinistro e destro
publicstatic<T>TreeNode<T>lr(@NotNullTdata,@NotNullTreeNode<T>left,@NotNull
TreeNode<T>right){
returnnewTreeNode<>(data,left,right);
}

// foglia
publicstatic<T>TreeNode<T>v(@NotNullTdata){
returnnewTreeNode<>(data,null,null);
}

// 1.b

@Override

publicbooleanequals(@NullableObjecto){
if(oinstanceofTreeNode){
BlockingQueue<Integer>b;

TreeNode<T>t=(TreeNode<T>)o;
returnareEqual(data,t.data)&&areEqual(left,t.left)&&areEqual(right,
t.right);
}
returnfalse;
}

privatestaticbooleanareEqual(@NullableObjecta,@NullableObjectb){
returna==b||(a!=null&&a.equals(b));
//return Objects.equals(a, b); // alternativamente si può usare questo metodo
del JDK
}

// 1.e

@Override
@NotNull
publicStringtoString(){
returnString.format("%s%s%s",data,left!=null?String.format("(%s)",left):
"",right!=null?
String.format("[%s]",right):"");
}

// 1.a

@Override
publicIterator<T>iterator(){
returniterator_easy();// stub ad una delle due implementazione; cambiare lo stub
per testare l'altra
}


// questo è l'implementazione facile, suggerita pubblicamente dal docente in classe
duranto l'appello del 13/9/22
privateIterator<T>iterator_easy(){
Collection<T>r=newArrayList<>();
dfs(r);
returnr.iterator();
}

privatevoiddfs(Collection<T>c){
c.add(data);
if(left!=null)left.dfs(c);
if(right!=null)right.dfs(c);
}

// questo è l'implementazione ottimizzata, che attraversa l 'albero senza liste d'appoggio
privateIterator<T>iterator_opt(){
returnnewIterator<>(){
@Nullable
privateTreeNode<T>current=TreeNode.this;

@Override

publicbooleanhasNext(){
returncurrent!=null;
}

@Nullable
privatestatic<T>TreeNode<T>getNextNode(@NotNullTreeNode<T>n){
if(n.left!=null)
returnn.left;
elseif(n.right!=null)
returnn.right;
else{
while(n.parent!=null){
finalTreeNode<T>last=n;
n=n.parent;
if(n.right!=null&&n.right!=last)
returnn.right;
}
returnnull;
}
}

@Override
@NotNull
publicTnext(){
Tr=current.data;
current=getNextNode(current);
returnr;
}
};
}

}

}
```

**Spiegazione rapida del ragionamento**

L'iteratore incapsula la traversata in-order senza esporre la struttura interna. La separazione tra struttura (nodi) e comportamento (iterazione) rispetta il principio Open/Closed.

---

#### 2.1.10 SkippableArrayList

**Testo dell'esercizio**

[Esame01/07/2022– 08/09/2018] Si implementi in Java 8+ una sottoclasse generica di java.util.ArrayList
di nomeSkippableArrayListche estende la superclasse con un iteratore in grado di discriminare gli elementi
secondo un predicato booleano. Gli elementi che soddisfano il predicato vengono processati da una certa funzione di
trasformazione5; gli altri vengono passati ad una seconda callback (non una funzione di trasformazione).
1.Realizziamo in Java 8+ una sottoclasse di java.util.ArrayListdi nomeSkippableArrayListparametrica su
un tipoTche estende la superclasse con un iteratore in grado di discriminare gli elementi secondo un predicato e
di processarli tramite due funzioni distinte a seconda dell’esito dell’applicazione del predicato all’elemento.
(a)Si definisca unainterfaccia funzionaledi nomePredicatespecializzando l’interfaccia generica java.util.Function
del JDK in modo che il dominio sia un generic Ted il codominio siaBoolean.
(b)Si definisca una interfacciaEitherparametrica su un tipo genericoTe che definisce due metodi. Il primo
metodo, di nomeonSuccess, prende unTe ritorna unTe viene chiamato dall’iteratore quando il predicato
ha successo. Il secondo metodo, di nomeonFailure, viene invocato invece quando il predicato fallisce, prende
un argomento di tipoTe non produce alcun risultato, tuttavia può lanciare una eccezione di tipoException:
(c)Si definisca la sottoclasseSkippableArrayListparametrica su un tipoEe si implementi un metodopub-
blico avente firmaIterator<E>iterator(Predicate<E>p,Either<E>f)che crea un iteratore con le
caratteristiche accennate sopra. Più precisamente:
•l’iteratore parte sempre dall’inizio della collezione ed arriva alla fine, andando avanti di un elemento alla
volta normalmente;
•ad ogni passo l’iteratore applica il predicatopall’elemento di tipoTcorrente, che chiameremox:s ep(x)
computatrueallora viene invocato il metodoonSuccessdife passato l’elementoxcome argomento;
altrimenti viene invocato il metodoonFailuree passatoxcome argomento a quest’ultimo;
•l’invocazione dionFailuredeve essere racchiusa dentro un blocco che assicura iltrappingdelle eccezioni:
in altre parole, una eccezione proveniente dall’invocazione dionFailurenon deve interrompere l’iteratore;
•quando viene invocatoonSuccess, il suo risultato viene restituito come elemento corrente dall’iteratore;
•quando viene invocatoonFailure, l’iteratore ritorna l’elemento originale che ha fatto fallire il predicato.
(d)Si scriva un esempio di codicemainche:
•costruisce unaArrayListdi interi vuota di nomedst;
•costruisce unaSkippableArrayListdi interi di nomesrce la popola con numeri casuali compresi tra0
e1 0, inclusi gli estremi6;
•invocandosolamente una voltail metodoiterator(Predicate<E>,Either<E>)disrccon gli argo-
menti opportuni, somma 1 a tutti gli elementi disrcmaggiori di 5 e appende in coda adstquelli minori
o uguali a5.
•Il metodoiterator()con due parametri richiesto dal punto (c) è un override o un overload?

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```java

// Questo sorgente contiene le soluzioni dell 'esame scritto di PO2 del 1/7/2022 per ciò che
riguarda i quesiti 1-2, ovvero le domande che coinvolgono Java.
// Il quesito 3 riguardante C++ è in un progetto Visual Studio a parte, non qui.
// Il codice qui esposto è Java 8+.

importjava.util.ArrayList;
importjava.util.Iterator;
importjava.util.function.Function;

publicclassEs1{

// 1.a
publicinterfacePredicate<T>extendsFunction<T,Boolean>{}

// 1.b
publicinterfaceEither<T>{
TonSuccess(Tx);
voidonFailure(Tx)throwsException;
}


// 1.c
publicstaticclassSkippableArrayList<E>extendsArrayList<E>{
publicIterator<E>iterator(Predicate<E>p,Either<E>f){
finalIterator<E>it=super.iterator(); // può anche essere un campo privato
della anonymous class
returnnewIterator<E>(){
@Override
publicbooleanhasNext(){
returnit.hasNext();
}

@Override
publicEnext(){
Ex=it.next();
if(p.apply(x))
returnf.onSuccess(x);
else{
try{
f.onFailure(x);
}
catch(Exceptione){
e.printStackTrace(); // si può anche non fare niente dentro il
catch, è indifferente
}
returnx;
}
}
};
}
}

}
```

**Spiegazione rapida del ragionamento**

Estendere `ArrayList` con un metodo `skip` aggiunge solo la logica specifica senza riscrivere il contratto esistente, riusando tutta l'implementazione standard.

---

#### 2.1.11 Figure geometriche

**Testo dell'esercizio**

[Esame03/06/2022– 31/01/2020 – 24/05/2018] Definiamo in Java 8+ un sistema di classi e di interfacce che
rappresentano figure geometriche piane e solide. Le figure geometriche rappresentate non sono posizionate nel piano
cartesiano o nello spazio, sono pertanto prive di coordinate. Per semplicità esse contengono solamente le informazioni
sulla lunghezza dei lati o delle facce di cui sono costituite.
5Una funzione di trasformazione è una funzione in cui dominio è uguale al codominio, per esempio una funzione f:ω è una funzione
di trasformazione sull’insiemeω.
6Si utilizzi la classeRandomdel JDK per generare numeri casuali.

1.Prima di cominciare, realizziamo una piccola libreria interna che consiste in alcuni metodi statici generici altamente
riusabili. Sia data la funzione di ordine superiore foldimplementata tramite un metodo statico pubblico:

```java
publicstatic<T,State>Statefold(Iterable<T>i,finalStatest0,BiFunction<State,T,State>f){
Statest=st0;
for(finalTe:i)
st=f.apply(st,e);
returnst;
}
```

(a)Si implementi tramiteuna solainvocazione difold()la funzione di ordine superioresumByavente la seguente
firma:

```java
publicstatic<T>doublesumBy(Iterable<T>i,Function<T,Double>f);
```

Essa calcola la sommatoria di tutti gli elementi di i trasformandoli indoubletramitef. La utilizzeremo ad
esempio per calcolare il perimetro di un poligono sommando la lunghezza di tutti i suoi lati, oppure l’area
laterale totale di un solido sommando l’area di tutte le superfici piane di cui è costituito.
(b)Avremo bisogno di ordinare le nostrefigure geometriche sulla base di diversi criteri, ad esempio l’area o il
volume. Si implementi il metodo staticocompareBy()avente la seguente firma:

```java
publicstatic<T>intcompareBy(Ts1,Ts2,Function<T,Double>f);
```

Esso confrontas1eds2convertendoli prima indoubletramitef, riducendo pertanto il confronto al confronto
tra due numeridouble.
(c)Quale forma di polimorfismo forniscono i generics definiti sulla firma di un metodo come ad esempio quelli
del metodofold()di cui sopra?
->Polimorfismo subtype ->Polimorfismo parametrico ->Polimorfismo parametrico first-class
->Polimorfismo ad-hoc
2.Definiamo ora i tipi essenziali per rappresentare figure geometriche piane e solide.
(a)La classeEdgerappresenta grandezze 1-dimensionali come lati di poligoni, spigoli di poliedri, segmenti e
circonferenze.

```java
publicclassEdgeimplementsComparable<Edge>{
privatefinaldoublelen;
publicEdge(doublelen){this.len=len;}
publicdoublelength(){returnlen;}
@Override
publicintcompareTo(Edges){/* DA IMPLEMENTARE */ }
}
}
```

Si implementi il metodocompareTo()tramiteuna solainvocazione dellacompareBy()definita sopra.
(b)L’interfacciaSurfacerappresenta figure piane qualunque:

```java
publicinterfaceSurfaceextendsComparable<Surface>{
doublearea();
doubleperimiter();
@Override
defaultintcompareTo(Surfaces){/* DA IMPLEMENTARE */ }
}
```

Si dia una implementazione di default del metodo compareTo()tramiteuna solainvocazione della compareBy()
definita sopra che esegua il confronto tra le aree.
(c)Il sotto-tipoPolygonrappresenta poligoni: l’interfaccia specializzaSurfacedando una implementazione di
default al metodoperimeter()e permette anche l’iterazione dei lati di cui il poligono stesso è costituito.

```java
publicinterfacePolygonextendsSurface,Iterable<Edge>{
@Override
defaultdoubleperimiter(){/* DA IMPLEMENTARE */ }
}
```


Si dia una implementazione di default del metodoperimeter()tramiteuna solainvocazione dellasumBy()
definita sopra.
(d)L’interfacciaSolidrappresenta solidi qualunque:

```java
publicinterfaceSolidextendsComparable<Solid>{
doubleouterArea();// area laterale totale
doublevolume();
@Override
defaultintcompareTo(Solids){/* DA IMPLEMENTARE */ }
}
```

Si implementi il metodocompareTo()tramiteuna solainvocazione dellacompareBy()definita sopra che
esegua il confronto tra i volumi.
(e)Polyhedronè un sotto tipo diSolide rappresenta poliedri. L’interfaccia è parametrica rispetto al sotto tipo di
Polygonche descrive le facce di cui il poliedro è costituito. Ad esempio, le facce di un cubo sono quadrati: una
classeCubeimplementerebbe pertanto l’interfacciaPolyhedronaventeSquarecometype argument,Square
↑Polygon.
UnPolyhedronpermette l’iterazione delle facce poligonali di cui esso è costituito; fornisce inoltre una imple-
mentazione di default del metodoouterArea()che calcola semplicemente la sommatoria delle aree delle sue
facce.

```java
publicinterfacePolyhedron<PextendsPolygon>extendsSolid,Iterable<P>{
@Override
defaultdoubleouterArea(){/* DA IMPLEMENTARE */ }
}
```

Si implementi il metodoouterArea()tramiteuna solainvocazione dellasumBy()definita sopra.
3.Si proceda ora alla definizione di una gerarchia di classi che rappresentano figure geometriche specifiche implemen-
tando le interfacce fin qui introdotte.
(a)Si implementi una classe che rappresentasfereimmutabili avente nomeSpheree che implementa l’interfaccia
Solid. Il costruttore diSpheredeve prendere come parametro solamente undouble: il raggio della sfera.
(b)Si implementi una classe che rappresentacilindriimmutabili avente nomeCilindere che implementa l’in-
terfacciaSolid. Il costruttore diCilinderdeve prendere come parametri duedouble: il raggio della base e
l’altezza del cilindro.
(c)Si implementi una classe che rappresenta rettangoliimmutabili avente nomeRectanglee che implementa
l’interfacciaPolygon. Il costruttore diRectangledeve prendere come parametri duedouble: base e altezza.
(d)Si implementi una classe che rappresenta quadrati immutabili avente nomeSquaree che estendeRectangle.
Il costruttore diSquaredeve prendere un solo parametro di tipodouble: il lato.
4.Si prenda in considerazione la seguente classe che rappresenta parallelepipedi immutabili. I parallelepipedi sono
poliedri aventi facce rettangolari.

```java
publicclassParallelepipedimplementsPolyhedron<Rectangle>{
protectedfinaldoublewidth,height,depth;
publicParallelepiped(doublewidth,doubleheight,doubledepth){
this.width=width;
this.height=height;
this.depth=depth;
}
@Override
publicdoublevolume(){/* DA IMPLEMENTARE */ }
@Override
publicIterator<Rectangle>iterator(){
Rectangler1=newRectangle(width,height),
r2=newRectangle(width,depth),
r3=newRectangle(height,depth);
returnList.of(r1,r2,r3,r1,r2,r3).iterator();
}
}
```


(a)Si implementi il metodovolume().
(b)Si definisca la classeCubecome sottoclasse diParallelepiped. Il costruttore diCubedeve prendere solamente
un parametro di tipodouble: la lunghezza dello spigolo.
(c)È possibile co-variare il tipo di ritorno del metodoiterator()diCubein modo che ritorni un Iterator<Square>?
Si motivi la risposta.
(d)Assumendo l’implementazione diCubedi cui al punto (b), si prenda in considerazione il seguente snippet:

```java
intfacet_cnt=1;
for(Squaresq:newCube(10.)){
intside_cnt=1;
for(Edgee:sq){
System.out.printf("side #%d/%d = %f\n",side_cnt++,facet_cnt,e.length());
}
++facet_cnt;
}
```

Tale codice compila? Si motivi la risposta.
5.Si prenda in considerazione il seguente snippet di codice che crea una lista eterogena di poliedri le cui facce sono
almeno rettangoli.

```java
Cubec1=newCube(1.),c2=newCube(2.));
Parallelepipedp1=newParallelepiped(1.,2.,3.),p2=newParallelepiped(2.,3.,4.);
List<Polyhedron<?extendsRectangle>>polys=List.of(c1,c2,p1,p2);
```

Seguono ora alcune chiamate ai metodiCollections.sort()del JDK. Gli oggetti nella listapolyssono sempre
gli stessi 4 (i cubic1ec2ed i parallelepipedip1ep2), ma ogni invocazione disort()li ordina in maniera diversa
a seconda del criterio di ordinamento. Per ciascuna invocazione si indichi quale oggetto viene messo in testa alla
lista, ovvero quale oggetto computerebbe l’espressionepolys.get(0), oppure se non compila.

```java
Collections.sort(polys)
```

->non compila ->c1 ->c2 ->p1 ->p2

```java
Collections.sort(polys,(x,y)->compareBy(x,y,Polyhedron::outerArea))
```

->non compila ->c1 ->c2 ->p1 ->p2

```java
Collections.sort(polys,(x,y)->compareBy(x,y,(p)->p.outerArea()))
```

->non compila ->c1 ->c2 ->p1 ->p2

```java
Collections.sort(polys,(x,y)->compareBy(x,y,(r)->r.perimiter()))
```

->non compila ->c1 ->c2 ->p1 ->p2

```java
Collections.sort(polys,(x,y)->compareBy(x,y,newFunction<>(){
publicDoubleapply(Polyhedron<?extendsRectangle>r){
returnr.volume();
}
}))
```

->non compila ->c1 ->c2 ->p1 ->p2

```java
Collections.sort(polys,(x,y)->Double.compare(sumBy(x,Square::perimiter),
sumBy(y,Rectangle::perimiter)))
```

->non compila ->c1 ->c2 ->p1 ->p2

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```java
importjava.util.*;
importjava.util.function.BiFunction;
importjava.util.function.Function;

// Questo sorgente contiene le soluzioni dell 'esame scritto di PO2 del 3/6/2022 per ciò che
riguarda i quesiti 1-5, ovvero le domande che coinvolgono Java.
// Il quesito 6 riguardante C++ è in una Solution per Visual Studio a parte, non qui.

publicclassAppello_3_6_22{

publicstatic<T,State>Statefold(Iterable<T>i,finalStatest0,BiFunction<State,T,
State>f){
Statest=st0;
for(finalTe:i){
st=f.apply(st,e);
}
returnst;
}

// 1.a
publicstatic<T>doublesumBy(Iterable<T>i,Function<T,Double>f){
returnfold(i,0.,(r,x)->r+f.apply(x));
}


// 1.b
publicstatic<T>intcompareBy(Ts1,Ts2,Function<T,Double>f){
returnDouble.compare(f.apply(s1),f.apply(s2));
}

publicstaticclassEdgeimplementsComparable<Edge>{
privatefinaldoublelen;

publicEdge(doublelen){
this.len=len;
}

publicdoublelength(){
returnlen;
}

// 2.a
@Override
publicintcompareTo(Edges){
returncompareBy(this,s,Edge::length);
}
}

publicinterfaceSurfaceextendsComparable<Surface>{
doublearea();

doubleperimiter();

// 2.b
@Override
defaultintcompareTo(Surfaces){
returncompareBy(this,s,Surface::area);
}
}

publicinterfacePolygonextendsSurface,Iterable<Edge>{
// 2.c
@Override
defaultdoubleperimiter(){
returnsumBy(this,Edge::length);
}
}

publicinterfaceSolidextendsComparable<Solid>{
doubleouterArea();

doublevolume();

// 2.d
defaultintcompareTo(Solids){
returncompareBy(this,s,Solid::volume);
}
}

publicinterfacePolyhedron<PextendsPolygon>extendsSolid,Iterable<P>{
// 2.e

@Override
defaultdoubleouterArea(){
returnsumBy(this,P::area);
}
}

// 3.a
publicstaticclassSphereimplementsSolid{
privatefinaldoubleradius;

publicSphere(doubleradius){
this.radius=radius;
}

@Override
publicdoubleouterArea(){
return4.*Math.PI*radius*radius;
}

@Override
publicdoublevolume(){
return4./3.*Math.PI*radius*radius*radius;
}
}

// 3.b
publicstaticclassCilinderimplementsSolid{
privatefinaldoubleradius,height;

publicCilinder(doubleradius,doubleheight){
this.radius=radius;
this.height=height;
}

@Override
publicdoubleouterArea(){
doubleb=Math.PI*radius*radius,l=2.*Math.PI*radius*height; // hanno
capito solo laterale
return2.*b+l;
}

@Override
publicdoublevolume(){
returnMath.PI*radius*radius*height;
}
}

// 3.c
publicstaticclassRectangleimplementsPolygon{
privatefinaldoublewidth,height;

publicRectangle(doublewidth,doubleheight){
this.width=width;
this.height=height;
}

@Override

publicdoublearea(){
returnwidth*height;
}

@Override
publicIterator<Edge>iterator(){
Edgew=newEdge(width),h=newEdge(height);
returnList.of(w,h,w,h).iterator();
}
}

// 3.d
publicstaticclassSquareextendsRectangle{
publicSquare(doubleside){
super(side,side);
}
}

publicstaticclassParallelepipedimplementsPolyhedron<Rectangle>{
protecteddoublewidth,height,depth;

publicParallelepiped(doublewidth,doubleheight,doubledepth){
this.width=width;
this.height=height;
this.depth=depth;
}

// 4.a
@Override
publicdoublevolume(){
returnwidth*height*depth;
}

@Override
publicIterator<Rectangle>iterator(){
Rectangler1=newRectangle(width,height),r2=newRectangle(width,depth),r3=
newRectangle(height,depth);
returnList.of(r1,r2,r3,r1,r2,r3).iterator();
}
}

// 4.b
publicstaticclassCubeextendsParallelepiped{
publicCube(doubleside){
super(side,side,side);
}
}


publicstaticvoidmain(String[]args){
// 4.d
{
intfacet_cnt=1;
// questo foreach non compila perché Cube è sottotipo di Iterable<Rectangle>, non di
Iterable<Square>
// si badi che NON è possibile co-variare il tipo di ritorno del metodo iterator() di
Cube in modo che si specializzi in Iterator<Square>

// perché è sound co-variare il tipo più esterno di un tipo parametrico, ma non il
type argument
// for (Square sq : new Cube(10.)) {
for(Rectanglesq:newCube(10.)){// così invece compilerebbe
intside_cnt=1;
for(Edgee:sq){
System.out.printf("side #%d/%d = %f\n",side_cnt++,facet_cnt,e.length());
}
++facet_cnt;
}
}

// 5
{
Cubec1=newCube(1.),c2=newCube(2.);
Parallelepipedp1=newParallelepiped(1.,2.,3.),p2=newParallelepiped(2.,3.,
4.);
List<Polyhedron<?extendsRectangle>>polys=newArrayList<>(List.of(c1,c2,p1,
p2));

// per testare rapidamente i risultati di queste sort, si usi debugger
Collections.sort(polys);
// c1
Collections.sort(polys,(x,y)->compareBy(x,y,Polyhedron::outerArea));
// c1
Collections.sort(polys,(x,y)->compareBy(x,y,(p)->p.outerArea()));
// c1
// Collections.sort(polys, (x, y) -> compareBy(x, y, (r) -> r.perimiter()));
// non compila
Collections.sort(polys,(x,y)->compareBy(x,y,newFunction<>(){
// c1
@Override
publicDoubleapply(Polyhedron<?extendsRectangle>r){
returnr.volume();
}
}));
// Collections.sort(polys, (x, y) -> Double.compare(sumBy(x, Square::perimiter),
// non compila
// sumBy(y, Rectangle::perimiter)));
}
}
}
```

**Spiegazione rapida del ragionamento**

La gerarchia di figure con metodi `area()` e `perimeter()` polimorfici evita if/else sul tipo. Il metodo `max()` generico con `Comparator<Polygon>` mostra l'uso corretto di PECS (`? super Polygon`).

---

#### 2.1.12 Artisti

**Testo dell'esercizio**

[Esame 20/06/2019] Si consideri la seguente interfaccia parametrica in linguaggioJava:

```java
publicinterfaceEquatable<T>{
booleanequalsTo(Tx);
}
```

Essa non sostituisce il meccanismo basato sul metodo equals(Object)della classeObject, tuttavia permette di
implementare il confronto di uguaglianza in maniera più sicura delegando parte della logica ad un metodo fortemente
tipato.
Si consideri ora la classe parametricaPerson, che supporta il confronto di uguaglianza con oggetti di tipoP:

```java
publicclassPerson<PextendsPerson<P>>implementsEquatable<P>{
publicfinalStringname;
publicfinalintage;

publicPerson(Stringname,intage){
this.name=name;
this.age=age;
}

@Override
publicbooleanequals(Objecto){/* da implementare */ }

@Override
publicbooleanequalsTo(Pother){/* da implementare */ }

@Override
publicStringtoString(){returnname;}
}
```

1.Si implementi il metodoequals(Object)della classePersonin modo che, datip1ep2di tipoPerson, le seguenti
invarianti siano rispettate:
•p1.equals(p1)==true
•p1.equals(null)==false
•p1.equals(e)==falsese l’espressioneeha tipo diverso dal tipo dip17
•p1.equals(p2)==p1.equalsTo(p2)
2.Si implementi il metodoequalsTo(P)della classePersonin modo che due oggetti siano uguali quando hanno il
medesimo nome e la medesima età.
Si consideri ora il seguente codice:

```java
publicclassArtistextendsPerson<Artist>{
publicfinalHairhair;

publicArtist(Stringname,intage,Hairhair){
super(name,age);
this.hair=hair;
}

@Override
publicbooleanequalsTo(Artistother){/* da implementare */ }
}

publicclassHairimplementsEquatable<Hair>{
```

7Ricordiamo che il metodo della classeObjectavente firmaClass<?extendsObject>getClass()consente di estrarre a runtime il tipo
rawdi un oggetto.

```java
publicfinalintlength;
publicfinalSet<Color>colors;

publicHair(intlength,Set<Color>colors){
this.colors=colors;
this.length=length;
}

@Override
publicbooleanequals(Objecto){/* da implementare */ }

@Override
publicbooleanequalsTo(Hairx){/* da implementare */ }
}

publicenumColor{
BROWN,DARK,BLONDE,RED,GRAY;
}
```

1.Si implementino i metodiequals(Object)eequalsTo(Hair)della classeHairin modo che due oggetti siano uguali
quando hanno la medesima lunghezza ed i medesimi colori (indipendentemente dal loro ordine). Si rispettino le
stesse invarianti del punto (a) per l’implementazione del metodo equals(Object)e si deleghi, come per la classe
Person, la parte fortemente tipata del confronto al metodoequalsTo(Hair).
2.Il metodoequalsTo(Artist)è davvero unoverridedel metodoequalsTo(P)ereditato dalla superclasse?
->No, perché latype erasureelimina il genericPche viene sostuito col suo constraintPersonnella super-
classe, la quale introduce di fatto un metodo equalsTo(Person),d ic u iequalsTo(Artist)non è un
override ma un overload.
->Sì, perché il metodoequalsTo()viene gestito in modo particolare dal compilatore Java, permettendo
override anche con firme differenti.
->No, perché la firma è diversa e quindi è un overload, non un override.
->Sì, perché il generic Pviene sostituito col tipo Artistnello scope della sottoclasse, pertanto viene
ereditato un metodoequalsTo(Artist), rendendo questo un vero override.
3.Si implementi il metodoequalsTo(Artist)della classeArtistin modo che due oggetti siano uguali quando hanno
i medesimi nome, età e capelli. Si badi a riusare opportunamente l’implementazione ereditata dalla superclasse.
Si considerino ora i seguenti binding in Java:

```java
Personalice=newPerson("Alice",23),
david=newArtist("Bowie",69,newHair(75,Set.of(Color.RED,Color.BROWN,
```

Color.GRAY)));

```java
Artistmorgan=newArtist("Morgan",47,newHair(20,Set.of(Color.GRAY,Color.DARK))),
madonna=newArtist("Madonna",60,newHair(50,Set.of(Color.BLONDE)));
```

4.Si prenda in considerazione il seguente metodo della classe java.util.Collectionsdel JDK 7+ per computare
l’elemento massimo in una collection secondo un criterio di ordinamento dato:

```java
static<T>Tmax(Collection<?extendsT>c,Comparator<?superT>cmp)
```

Si considerino ora i seguenti binding in Java:

```java
List<Artist>artists=Arrays.asList((Artist)david,morgan,madonna);
List<Person>persons=Arrays.asList(alice,david,morgan,madonna);
```

(a)Si scriva uno statement di invocazione del metodo max()che computi l’oggetto della listaartistsavente il
più alto prodotto tra lunghezza dei capelli e numero di colori.
(b)Si scriva uno statement di invocazione del metodo max()che computi l’oggetto della listapersonsavente il
nome che viene per primo in ordine lessicografico.
(c)Il seguente statement sarebbe accettato dal compilatore Java 7+?

```java
Artistc=Collections.max(artists,newComparator<Person>(){
publicintcompare(Persona,Personb){
returna.age-b.age;
}
});
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```java
importjava.util.*;

publicclassEs1{

publicinterfaceEquatable<T>{
booleanequalsTo(Tx);
}

publicstaticclassPerson<PextendsPerson<P>>implementsEquatable<P>{

publicfinalStringname;
publicfinalintage;


publicPerson(Stringname,intage){
this.name=name;
this.age=age;
}

// 1.a
@Override
publicbooleanequals(Objecto){
// self check
if(this==o)
returntrue;
// null check
if(o==null)
returnfalse;
// type check
if(getClass()!=o.getClass())
returnfalse;
// field comparison
returnequalsTo((P)o); // deleghiamo al metodo equalsTo(P) il confronto tipato
tra i campi
}

// 1.b
@Override
publicbooleanequalsTo(Pother){
returnname.equals(other.name)&&age==other.age;
}

@Override
publicStringtoString(){
returnname;
}

}

publicstaticclassArtistextendsPerson<Artist>{

publicfinalHairhair;

publicArtist(Stringname,intage,Hairhair){
super(name,age);
this.hair=hair;
}

// 1.d RISPOSTA: 4
// 1.e
@Override
publicbooleanequalsTo(Artistother){
returnsuper.equalsTo(other)&&hair.equals(other.hair);
}

}

publicenumColor{
BROWN,DARK,BLONDE,RED,GRAY;
}


publicstaticclassHairimplementsEquatable<Hair>{
publicfinalintlength;
publicfinalSet<Color>colors;

publicHair(intlength,Set<Color>colors){
this.colors=colors;
this.length=length;
}

// 1.c
@Override
publicbooleanequals(Objecto){ // uguale a Person.equals(Object)
if(this==o)
returntrue;
if(o==null)
returnfalse;
if(getClass()!=o.getClass())
returnfalse;
returnequalsTo((Hair)o);
}

// 1.c
@Override
publicbooleanequalsTo(Hairx){
returncolors.equals(x.colors)&&length==x.length;
}
}


publicstaticvoidprint(Stringfmt,Object...o){
System.out.println(String.format(fmt,o));
}

publicstaticvoidmain(String[]args){
Personalice=newPerson("Alice",23),
david=newArtist("Bowie",69,newHair(75,Set.of(Color.RED,Color.BROWN,
Color.GRAY)));

Artistmorgan=newArtist("Morgan",47,newHair(20,Set.of(Color.GRAY,Color.DARK))),
madonna=newArtist("Madonna",60,newHair(50,Set.of(Color.BLONDE)));

// 1.g (lanciate il programma per printare le risposte booleane)
List<Boolean>bs=Arrays.asList(
alice.equals(null), // false
alice.equals(alice), // true
//null.equals(david), // alcuni compilatori rifiutano null.metodo(), altri
lo accettano ma a runtime viene lanciato NullPointerException
alice.equals(david), // false
alice.equalsTo(morgan), // false
morgan.equals(morgan), // true
morgan.equals(madonna), // false
morgan.equals(david), // false
//morgan.equalsTo(david), // non compila
david.equalsTo(morgan), // false
madonna.equals(3) // false
//, madonna.equalsTo("Madonna") // non compila
);

print("[1.g] %s",bs);

// 1.h
List<Artist>artists=Arrays.asList((Artist)david,morgan,madonna);
List<Person>persons=Arrays.asList(alice,david,morgan,madonna);

// 1.h.i
Artista=Collections.max(artists,newComparator<Artist>(){
@Override
publicintcompare(Artista,Artistb){
returna.hair.length*a.hair.colors.size()-b.hair.length*
b.hair.colors.size();
}
});
print("[1.h.i] %s",a);

// 1.h.ii
Personb=Collections.max(persons,newComparator<Person>(){
@Override
publicintcompare(Persona,Personb){
returna.name.compareTo(b.name);
}
});
print("[1.h.ii] %s",b);

// 1.h.iii: RISPOSTA: 2
Artistc=Collections.max(artists,newComparator<Person>(){
publicintcompare(Persona,Personb){
returna.age-b.age;
}
});

}


}
```

**Spiegazione rapida del ragionamento**

La gerarchia `Person -> Artist` con `equalsTo()` parametrico mostra come estendere il confronto di uguaglianza in modo type-safe, evitando cast non sicuri.

---

#### 2.1.13 Iteratori

**Testo dell'esercizio**

[Esame30/05/2019– 22/01/2019] Si implementi il seguente sistema di iteratori e trasformazioni tra iteratori.
1.Si definisca unainterfaccia funzionalesimile ajava.util.BiFunctionche rappresenta funzioni binarie, parame-
trica sui 2 tipi degli argomenti e sul tipo di ritorno.
2.Si definisca un tipo per la coppia eterogenea immutabile, ovvero una classe Pairparametrica su 2 tipi distinti che
rappresentano rispettivamente il tipo del primo e del secondo elemento della coppia.
3.Si definisca un tipo per la tripla eterogenea immutabile, ovvero una sottoclasse diPairdi nomeTriple, parametrica
su 3 tipi distinti che rappresentano i tipi dei 3 elementi.
4.Si definisca un metodo staticoevalIteratorgenerico su 3 tipiA,BeCche, dato un iteratore su coppieA≃Bed
una funzione binariaA≃B↗C, produce un nuovo iteratore su tripleA≃B≃Cche si comporta come unwrapperdi
quello in input. L’iteratore in output applica la funzione binaria ad ogni coppia di elementi letti dall’iteratore in
input e produce una tripla con i due valori appena letti e passati alla funzione assieme al risultato di quest’ultima.
Si utilizzino i tipiPair,TripleeBiFunctiondefiniti nei punti precedenti.

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```java
importjava.util.*;

publicclassEs2{


// 2.a
@FunctionalInterface
publicinterfaceBiFunction<A,B,C>{
Capply(Aa,Bb);
}

// 2.b
publicstaticclassPair<A,B>{
publicfinalAfst;
publicfinalBsnd;

publicPair(Aa,Bb){
this.fst=a;
this.snd=b;

}
}

// 2.c
publicstaticclassTriple<A,B,C>extendsPair<A,B>{
publicfinalCtrd;

publicTriple(Aa,Bb,Cc){
super(a,b);
this.trd=c;
}
}

// 2.d
publicstatic<A,B,C>Iterator<Triple<A,B,C>>evalIterator(Iterator<Pair<A,B>>it,
BiFunction<A,B,C>f){
returnnewIterator<>(){
@Override
publicbooleanhasNext(){
returnit.hasNext();
}

@Override
publicTriple<A,B,C>next(){
Pair<A,B>p=it.next();
returnnewTriple<>(p.fst,p.snd,f.apply(p.fst,p.snd));
}
};
}

}
```

**Spiegazione rapida del ragionamento**

Definire un `Iterator<T>` dedicato con stato interno rende la scansione riproducibile e conforme al contratto Java (hasNext/next). Il for-each funziona automaticamente su `Iterable<T>`.

---

#### 2.1.14 Sequenza di Fibonacci

**Testo dell'esercizio**

[Esame 30/05/2019]
1.Si implementi un classeFiboSequencele cui istanze rappresentano sequenze contigue di numeri di Fibonacci di
lunghezza data in costruzione. Tali istanze devono essere iterabilitramite il costruttofor-eachdi Java, devono
pertanto implementare l’interfaccia parametrica del JDKjava.util.Iterable<T>. Ad esempio, il seguente codice
deve compilare e stampare i primi 100 numeri di Fibonacci:

```java
for(intn:newFiboSequence(100)){
System.out.println(n);
}
```

Requisito obbligatorio è che i numeri di Fibonacci generati dall’iteratore8sottostiano ad un meccanismo dicaching
che ne allevia il costo computazionale memorizzando il risultato di ogni passo di ricorsione, in modo che ogni
computazione successiva con il medesimo input costi solamente un accesso in lettura alla cache.
2.Si scriva il codicede-zuccheratodello statementfor-eachdi cui al punto precedente.
3.Si riscriva la classeFiboSequencein modo che la cache sia condivisa tra molteplici istanze.

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```java
importjava.util.HashMap;
importjava.util.Iterator;
importjava.util.Map;

publicclassEs3{

// 3.a
publicstaticclassFiboSequenceimplementsIterable<Integer>{

privatefinalintmax;
privatefinalMap<Integer,Integer>cache=newHashMap<>();

publicFiboSequence(intmax){
this.max=max;
}

@Override
publicIterator<Integer>iterator(){
returnnewIterator<>(){
privateinti=0;

@Override
publicbooleanhasNext(){
returni<max;

}

@Override
publicIntegernext(){
returnfib(i++);
}

privateintfib(intn){
if(n<2)return1;
else{
Integerx=cache.get(n);
if(x!=null)returnx;
else{
intr=fib(n-1)+fib(n-2);
cache.put(n,r); // si commenti questo statement per
// disabilitare la cache e vedere la differenza
// enorme di tempi di calcolo
returnr;
}
}
}
};
}
}

// si lanci questo main e si osservi quanto velocemente la macchina produce 100 numeri di
fibonacci:
// se non ci fosse la cache sarebbe drammaticamente più lento a causa delle continue
ricorsioni
publicstaticvoidmain(String[]args){
for(intn:newFiboSequence(100)){
System.out.println(n);
}
}


// 3.b
publicstaticvoidmain__desugared(){
// questo for senza il terzo statement è equivalente ad un while
for(Iterator<Integer>it=newFiboSequence(100).iterator();it.hasNext();){
intn=it.next();
System.out.println(n);
}
}


// 3.c
publicstaticclassGlobalFiboSequenceimplementsIterable<Integer>{

privatefinalintmax;
privatefinalstaticMap<Integer,Integer>cache=newHashMap<>();// è tutto uguale a
// FiboSequence
eccetto per
//la cache che è
statica

publicGlobalFiboSequence(intmax){

this.max=max;
}

@Override
publicIterator<Integer>iterator(){
returnnewIterator<>(){
privateinti=0;

@Override
publicbooleanhasNext(){
returni<max;
}

@Override
publicIntegernext(){
returnfib(i++);
}

privateintfib(intn){
if(n<2)return1;
else{
Integerx=cache.get(n);
if(x!=null)returnx;
else{
intr=fib(n-1)+fib(n-2);
cache.put(n,r); // si commenti questo statement per disabilitare
la cache
// e vedere la differenza enorme di tempi di
calcolo
returnr;
}
}
}
};
}
}

}
```

**Spiegazione rapida del ragionamento**

Generare Fibonacci tramite un iteratore (lazy) evita di calcolare tutta la sequenza in anticipo, utile per sequenze lunghe o potenzialmente infinite.

---

#### 2.1.15 Classe Random

**Testo dell'esercizio**

[Esame 04/09/2018] Si prenda in considerazione questa semplificazione della classejava.util.Randomdel JDK:
essenzialmente essa o!re un costruttore senza parametri, un secondo costruttore con il seed per inizializzare il PRNG
ed alcuni metodi per generare valori numerici di tipo differente:

```java
publicclassRandom{
publicRandom(){...}
publicRandom(intseed){...}
publicbooleannextBoolean(){...}
publicintnextInt(){...}
publicdoublenextDouble(){...}
}
```

8Nell’esempio l’iteratore è implicitamente utilizzato dal costrutto for-each.

1.Si scriva unwrapperdella classeRandomche si comporta come unsingleton, facendo attenzione a riprodurre ogni
aspetto dell’originale.
2.Si definisca una classeRandomIteratorche implementa l’interfacciajava.util.Iterator<Integer>del JDK e
che si comporta come un iteratore su interi, generando un numero casuale ad ogni invocazione del metodo next()
anziché scorrendo una vera collection, fino ad esaurire la sequenza di lunghezza specificata in costruzione. Si
implementino opportunamente il costruttore ed i metodi richiesti dall’interfaccia.

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```java
importorg.jetbrains.annotations.NotNull;
importorg.jetbrains.annotations.Nullable;

importjava.util.Iterator;
importjava.util.Random;

publicclassEs7{

// 7.b
publicstaticclassRandomSingleton{
privateRandomSingleton(){
}

@Nullable
privatestaticRandominstance;

@NotNull

privatestaticRandominstance(){
if(instance==null)
instance=newRandom();
returninstance;
}

// fare un setter per cambiare il seed del PRNG è uno dei modi possibili per riprodurre
// il comportamento originale della classe Random del JDK. Normalmente, per avere un
// seed specifico è necessario costruire un oggetto Random tramite l 'apposito
// costruttore. Nella nostra conversione a singleton rappresentiamo invece questo
// aspetto come un setter che internamente reistanzia il singleton in maniera
// trasparente all'utente.
publicvoidsetSeed(intseed){
instance=newRandom(seed);
}

publicstaticintnextInt(){
returninstance().nextInt();
}

publicstaticbooleannextBoolean(){
returninstance().nextBoolean();
}

publicstaticdoublenextDouble(){
returninstance().nextDouble();
}

}

// 7.c
publicstaticclassRandomIteratorimplementsIterator<Integer>{
privateintlen;

publicRandomIterator(intlen){
this.len=len;
}

@Override
publicbooleanhasNext(){
returnlen>0;
}

@Override
publicIntegernext(){
--len;
returnRandomSingleton.nextInt();
}
}

}
```

**Spiegazione rapida del ragionamento**

Incapsulare `Random` in una classe dedicata con interfaccia ristretta migliora testabilita', controllo dello stato e consente mock nei test.

---

#### 2.1.16 FancyArrayList

**Testo dell'esercizio**

[Esame 20/06/2018] Si implementi inJavauna sottoclasse generica diArrayListdi nomeFancyArrayListche estende
le funzionalità della superclasse con un iteratore più versatile. Il nuovo iteratore deve essere in grado di andare sia
avanti che indietro secondo un valore di incremento intero non nullo; e di processare gli elementi che incontra durante
l’attraversamento applicando una funzione di trasformazione9.
1.Si definisca unainterfaccia funzionaledi nomeFunctionparametrica sia sul tipo del dominio che sul tipo del
codominio, equivalente a quella definita dal JDK 8+ nel package java.util.function.
2.Si definisca la sottoclasseFancyArrayListparametrica su un tipoEe si implementi un metodopubblico aventefir-
maIterator<E>iterator(intstep,Function<E,E>f)che crea un iteratore con le caratteristiche accennate
sopra tramite unaclasse anonima. Più precisamente:
•quando il valore del parametrostepè positivo, l’iteratore parte dall’inizio della collezione e va avanti incre-
mentando il cursore distepposizioni ad ogni passo; quando invece stepè negativo, l’iteratore parte dalla
fine della collezione e vaindietrodecrementando il cursore;
•ad ogni passo l’iteratore applica la funzione di trasformazione fall’elemento da restituire.
3.Si aggiungano aFancyArrayListi seguenti metodipubblici, badando ad implementarli in funzione del metodo
iterator(int,Function<E,E>)realizzato per l’esercizio precedente,senza replicazionidi codice. Per ciascuno
si specifichi inoltre se è un override oppure no, utilizzando opportunamente l’annotazione @Override.
(a)Si implementi il metodo avente firmaIterator<E>iterator()che produce un iteratore convenzionale che
procede in avanti di una posizione alla volta.
(b)Si implementi il metodo avente firmaIterator<E>backwardIterator()che produce un iteratore rovescio
che procede indietro di una posizione alla volta.
4.Si rifattorizzi il metodoiterator(int,Function<E,E>)realizzato per il punto (2) in modo che non usi una
classe anonima, ma una nuova classe innestatastatica e parametrica di nomeFancyIterator. Si presti particolare
attenzione all’uso dei generics ed al passaggio esplicito della enclosing instanceal costruttore.

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```java
importjava.util.ArrayList;
importjava.util.Iterator;

publicclassEs10{


publicstaticclassFancyArrayList<E>extendsArrayList<E>{

// 10.a
@FunctionalInterface
publicinterfaceFunction<A,B>{
Bapply(Aa);
}

// 10.b
publicIterator<E>iterator(intstep,Function<E,E>f){
returnnewIterator<>(){
privateintpos=step>0?0:size()-1;

@Override
publicbooleanhasNext(){
returnpos>=0&&pos<size();
}

@Override
publicEnext(){
Er=get(pos);
pos+=step;
returnf.apply(r);
}
};
}

// 10.c.i
@Override
publicIterator<E>iterator(){
returniterator(1,(x)->x); // si può fare l 'identità con la lambda (solo Java
8+)
}

// 10.c.ii
publicIterator<E>backwardIterator(){
returniterator(-1,newFunction<>(){// oppure con una anonymous class
semplicissima
@Override
publicEapply(Ex){
returnx;
}
});
}

// 10.d
// bisogna chiamare questo metodo con un nome diverso da iterator()
// altrimenti il sorgente non compila
publicIterator<E>iterator__refactored(intstep,Function<E,E>f){
returnnewFancyIterator<>(this,step,f);
}

// questa classe è il refactoring della anonynmous class dell 'esercizio 10.b:
// la differenza è che conserva la enclosing instance esplicitamente
privatestaticclassFancyIterator<E>implementsIterator<E>{
privateFancyArrayList<E>enclosing;

privateFunction<E,E>f;
privateintstep,pos;

publicFancyIterator(FancyArrayList<E>parent,intstep,Function<E,E>f){
this.enclosing=parent;
this.step=step;
this.f=f;
pos=step>0?0:parent.size()-1;
}

@Override
publicbooleanhasNext(){// questo metodo è uguale
returnpos>=0&&pos<enclosing.size();
}

@Override
publicEnext(){
Er=enclosing.get(pos);
pos+=step;
returnf.apply(r);
}
}

}

}
```

**Spiegazione rapida del ragionamento**

`FancyArrayList` estende `ArrayList` aggiungendo operazioni funzionali (`map`, `filter`, ecc.) mantenendo compatibilita' con l'API standard.

---

#### 2.1.17 Minimo e massimo in una lista

**Testo dell'esercizio**

[Esame 20/06/2018] Si realizzi inJavaun algoritmo che trova il minimo ed il massimo in una lista generica e restituisca
i risultati, rispettivamente, come primo e secondo elemento di una coppia omogenea.
1.Si definisca un tipo per la coppia eterogenea, ovvero una classe Pairparametrica su due tipi distintiAeBche
rappresentano rispettivamente il tipo del primo e del secondo elemento della coppia.
2.Si implementi un metodo statico e generico avente la seguente firma 10:
static<E>Pair<E,E>findMinAndMax(List<E>l,Comparator<E>c)
L’algoritmo di ricerca del minimo e del massimo deve eseguire una sola traversatadella lista; si assuma che gli
argomenti siano non-nulli e che la lista abbia sempre almeno un elemento.
3.Si definisca un metodo in overload con il precedente che non usi un Comparatorma aggiunga il constraint Comparable
al genericE, secondo la seguente firma :
static<EextendsComparable<E>>Pair<E,E>findMinAndMax(List<E>l)
Si implementi questo metodo in funzione del precedente, senza replicarel’algoritmo di ricerca.
9Una funzione di trasformazione è una funzione in cui dominio è uguale al codominio, per esempio f:ωper un qualche insiemeω.
10L’interfaccia parametricaComparator<T>include un metodo binario avente firma intcompare(Ta,Tb), il quale confronta i due
argomenti e ritorna un intero secondo la semantica standard del confronto at r ev i ein Java.

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```java
importjava.util.Comparator;
importjava.util.List;

publicclassEs11{

// 11.a
// versione con campi pubblici immutabili, ma si potevano fare anche i getter volendo
publicstaticclassPair<A,B>{
publicfinalAa;
publicfinalBb;
publicPair(Aa,Bb){
this.a=a;
this.b=b;
}
}

// 11.b
publicstatic<E>Pair<E,E>findMinAndMax(List<?extendsE>l,Comparator<E>c){
Emin=l.get(0),max=min;
for(Ex:l){
if(c.compare(x,max)>0)max=x;
if(c.compare(x,min)<0)min=x;
}
returnnewPair<>(min,max);
}

// 11.c
publicstatic<EextendsComparable<E>>Pair<E,E>findMinAndMax(List<?extendsE>l){
returnfindMinAndMax(l,newComparator<>(){

@Override
publicintcompare(Ea,Eb){
returna.compareTo(b);
}
});
}

}
```

**Spiegazione rapida del ragionamento**

Una singola passata (O(n)) e' sufficiente per trovare sia minimo che massimo. L'uso di `Comparable` o `Comparator` rende il metodo generico su qualsiasi tipo ordinabile.

---

#### 2.1.18 Hanna Barbera

**Testo dell'esercizio**

[Esame 24/05/2018] Si assumano le seguenti classi e interfacceJava, innestate per brevità in una sola classe a
top-level:

```java
publicclassHannaBarbera{

publicinterfaceFood{
intgetWeight();
}

publicinterfaceAnimalextendsFood{
voideat(Foodf);
}

publicstaticclassMouseimplementsAnimal{
privateintweight;

publicMouse(intweight){this.weight=weight;}

@Override
publicvoideat(Foodf){weight+=f.getWeight()/2;}

@Override
publicintgetWeight(){returnweight;}
}

publicstaticclassCatimplementsAnimal{
privateintweight;

publicCat(intweight){this.weight=weight;}

@Override
publicvoideat(Foodf){weight+=f.getWeight()/5;}

@Override
publicintgetWeight(){returnweight;}
}

publicstaticclassCheeseimplementsFood{
@Override
publicintgetWeight(){return50;}
}
}
```

1.Come sarebbe possibile rifattorizzare questo codice in modo da ridurre al minimo le ripetizioni e massimizzare il
riuso? Si scriva il codice rifattorizzato, facendo attenzione a non cambiare il comportamento del programma.
2.Si prenda in considerazione il seguente metodo main()per la classeHannaBarbera, supponendo che le restanti
classi ed interfacce innestate siano quelle rifattorizzate secondo quanto richiesto dall’esercizio 1.

```java
publicstaticvoidmain(String[]args){
Animaltom=newCat(200);
Animaljerry=newMouse(10);
jerry.eat(newCheese());
jerry.eat(newFood(){
@Override
publicintgetWeight(){return10;}
});
tom.eat(jerry);
System.out.println(String.format("Tom now weights %d",tom.getWeight()));
}
```

(a)Qualora una o più parti del metodo main()non siano compatibili a causa della rifattorizzazione eseguita
precedentemente, si scriva in maniera chiara quali modifiche sono necessarie per renderlo compilabile mante-
nendo la semantica dell’originale; se preferibile, si riscriva per chiarezza l’intero codicemain()opportunamente
modificato.
(b)Qual’è il peso del gatto Tom che viene scritto in output alla fine?

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```java
publicclassEs12{

// tipi dati dal testo

publicinterfaceFood{
intgetWeight();
}

publicinterfaceAnimalextendsFood{
voideat(Foodf);
}

publicstaticclassMouseimplementsAnimal{
privateintweight;

publicMouse(intweight){
this.weight=weight;
}

@Override
publicvoideat(Foodf){
weight+=f.getWeight()/3;
}

@Override
publicintgetWeight(){
returnweight;
}
}

publicstaticclassCatimplementsAnimal{
privateintweight;

publicCat(intweight){
this.weight=weight;
}

@Override
publicvoideat(Foodf){
weight+=f.getWeight()/10;
}

@Override
publicintgetWeight(){
returnweight;
}
}


publicstaticclassCheeseimplementsFood{
@Override
publicintgetWeight(){
return300;
}
}


// 12.a
// le interfacce Food e Animal non serve rifattorizzarle, sono a posto così
publicabstractclassAbstractAnimalimplementsAnimal{
privateintweight,div;

protectedAbstractAnimal(intweight,intdiv){
this.weight=weight;
}

@Override
publicvoideat(Foodf){
weight+=f.getWeight()/div;
}

@Override
publicintgetWeight(){
returnweight;
}
}

publicclassCatextendsAbstractAnimal{
protectedCat(intweight,intdiv){
super(weight,5); // valori del tema A
}

publicclassMouseextendsAbstractAnimal{
protectedMouse(intweight,intdiv){
super(weight,2);
}
}


// 12.b.i
publicstaticvoidmain(String[]args){
Animaltom=newCat(200); // costanti del tema A
Animaljerry=newMouse(10);
jerry.eat(newCheese());
jerry.eat(newFood(){
@Override
publicintgetWeight(){
return10;
}
});
tom.eat(jerry);
System.out.println(String.format("Tom now weights %d",tom.getWeight()));//b.ii 208

}
}
```

**Spiegazione rapida del ragionamento**

La modellazione dei personaggi Hanna Barbera usa la gerarchia per condividere comportamenti comuni, con override per comportamenti specifici di ogni personaggio.

---

### 2.2 C++

#### 2.2.1 Pair

**Testo dell'esercizio**

[Esame 10/09/2024 –01/06/2023]
1.Vogliamo implementare in C++ una classe pair templatizzata su due tipi generici AeBche rappresentano i tipi
del primo e del secondo elemento della coppia. Si utilizzi la versione del linguaggio che si preferisce, è sufficiente
dichiarare quale.
(a)La classepairdeve rispettare gli stili della generic programming e comportarsi come un valore: si definiscano
gli opportuni costruttori, tra cui quello per copia, e l’operatore di assegnamento.
(b)Si implementino tutti i metodi e gli operatori ritenuti necessari o interessanti.
(c)Si implementi un costruttore per copia templatizzato su due tipi generici CeDche è in grado di costruire una
pair<A,B>dato un argomento di tipoconstpair<C,D>&.

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```cpp
exportmodulepairs;

import<string>;
import<functional>;


export
template<classA,typenameB>
classpair
{
template<classC,typenameD>friendclasspair; // necessario per il copy
constructor templatizzato

private:
Afirst;
Bsecond;

public:
// 2.a
pair():first(),second(){}
pair(constA&a,constB&b):first(a),second(b){}
pair(constpair<A,B>&p):first(p.first),second(p.second){}

pair<A,B>&operator=(constpair<A,B>&p)
{
first=p.first;
second=p.second;
return*this;
}

// 2.c
template<classC,typenameD>
pair(constpair<C,D>&p):first(p.first),second(p.second){}


// 2.b
pair<A,B>operator++(int)
{
pair<A,B>tmp(*this);
first++;
second++;
returntmp;
}

pair<A,B>&operator++()
{
++first;
++second;
return*this;
}

booloperator==(constpair<A,B>&p)const
{
returnfirst==p.first&&second==p.second;

}

booloperator!=(constpair<A,B>&p)const
{
return!(*this==p);
}

// altri operatori aritmentici ed in-place sono analoghi
pair<A,B>operator+(constpair<A,B>&p)const
{
returnpair<A,B>(first+p.first,second+p.second);
}

pair<A,B>&operator+=(constpair<A,B>&p)
{
first+=p.first;
second+=p.second;
return*this;
}

constA&fst()const
{
returnfirst;
}

A&fst()
{
returnfirst;
}

constB&snd()const
{
returnsecond;
}

B&snd()
{
returnsecond;
}

};


exportintmain()
{
pair<int,int>p1(4,5);
pair<int,int>p2(p1);

pair<std::string,bool>p3("ciao",true);
pair<double,double>p4(p1);

p1=p2;

intn=p1.fst();
p1.snd()=p1.snd()*3;
p4+=p1; // converte implicitamente il RV in un pair<double, double> tramite un
conversion copy-constructor templatizzato


return0;
}
```

**Spiegazione rapida del ragionamento**

Il template `pair<A,B>` e' un esempio classico di contenitore generico C++: sfrutta la deduzione dei tipi e gli operatori per massimizzare espressivita' e type safety.

---

#### 2.2.2 Matrici

**Testo dell'esercizio**

[Esame 25/06/2024 – 05/09/2023 –03/06/2022]
1.Si scriva in C++ una classe genericamatrixche rappresenta matrici bidimensionali di valori di tipoT, dove T è un
tipo templatizzato. Tale classe deve comportarsi come un valore, secondo lo stile deicontainerdi STL, pertanto
deve supportare il costruttore per copia, l’operatore di assegnamento ed il costruttore di default. Si aggiungano
poi metodi e operatori a piacere, tra cui almeno i seguenti:
•costruttore con 2 parametri + 1 opzionale: numero di righe, numero di colonne e valore iniziale di tipo T
•distruttore (se necessario);
•member typeiterator,const_iteratorevalue_type;
•accesso tramite riga e colonna implementando 2 overload (const e non-const) di operator();
•2overload (const e non-const) dei metodibegin()edend()per poter iterare tutta la matrice dall’angolo
superiore sinistro all’angolo inferiore destro come se fosse piatta.
L’implementazione può utilizzare STL liberamente, se lo si desidera.
Si prenda a riferimento il seguente snippet per la specifica dei requisiti del tipo matrix:

```cpp
matrix<double>m1; // non inizializzata
matrix<double>m2(10,20);// 10*20 inizializzata col default constructor di double
matrix<double>m3(m2); // costruita per copia
m1=m2; // assegnamento
m3(3,1)=11.23; // operatore di accesso come left-value
for(typenamematrix<double>::iteratorit=m1.begin();it!=m1.end();++it){
typenamematrix<double>::value_type&x=*it;// de-reference non-const
x=m2(0,2);// operatore di accesso come right-value
}
matrix<string>ms(5,4,"ciao");// 5*4 inizializzata col la stringa passata come terzo
```

argomento

```cpp
for(typenamematrix<string>::const_iteratorit=ms.begin();it!=ms.end();++it)
cout<<*it;// de-reference const
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```cpp
// Questo sorgente contiene le soluzioni dell 'esame scritto di PO2 del 3/6/2022 per ciò che
riguarda il quesito 6, ovvero la domanda che coinvolge C++.
// I quesiti 1-5 riguardanti Java sono in un progetto IntelliJ a parte, non qui.
// Il codice C++ qui esposto è standard C++ vanilla (a.k.a. C++03), sebbene il progetto VS sia
configurato con il compilatore di default C++14

#include<iostream>
#include<vector>

usingnamespacestd;

// 6
template<classT>
classmatrix
{
private:
size_tcols;
vector<T>v;

public:
matrix():cols(0),v(){}
matrix(size_trows,size_tcols_):cols(cols_),v(rows*cols){}
matrix(size_trows,size_tcols_,constT&v):cols(cols_),v(rows*cols,v){}
matrix(constmatrix<T>&m):cols(m.cols),v(m.v){}

typedefTvalue_type;
typedeftypenamevector<T>::iteratoriterator;
typedeftypenamevector<T>::const_iteratorconst_iterator;

matrix<T>&operator=(constmatrix<T>&m)
{
v=m.v;
return*this;
}

T&operator()(size_ti,size_tj)
{
returnv[i*cols+j];
}

constT&operator()(size_ti,size_tj)const
{
return(*this)(i,j);
}

iteratorbegin()
{
returnv.begin();
}

iteratorend()
{

returnv.end();
}

const_iteratorbegin()const
{
returnbegin();
}

const_iteratorend()const
{
returnend();
}
};


intmain()
{
matrix<double>m1; // non inizializzata
matrix<double>m2(10,20);// 10*20 inizializzata col default constructor di double
matrix<double>m3(m2); // costruita per copia
m1=m2; // assegnamento
m3(3,1)=11.23; // operatore di accesso come left-value

for(typenamematrix<double>::iteratorit=m1.begin();it!=m1.end();++it){
typenamematrix<double>::value_type&x=*it; // de-reference non-const
x=m2(0,
2); //
operatore di accesso come right-value

}

matrix<string>ms(5,4,"ciao");// 5*4 inizializzata col la stringa passata come terzo
argomento
for(typenamematrix<string>::const_iteratorit=ms.begin();it!=ms.end();++it)
cout<<*it; // de-reference const
}
```

**Spiegazione rapida del ragionamento**

La matrice template separa rappresentazione (array bidimensionale) da operazioni (operatori `*`, `[]`). Gli iteratori const/non-const permettono uso in contesti read-only.

---

#### 2.2.3 Sommatoria

**Testo dell'esercizio**

[Esame 14/06/2024]
1.Si svolgano i seguenti esercizi in linguaggio C++11.
(a)Si implementi una funzione templatizzata su un tipo Cche, dato un container STL avente tipoC, ne somma
tutti gli elementi tramite l’operatore di addizione e ritorna il risultato della sommatoria.
(b)Quali vincoli sul template parameter Csono implicitamente richiesti? Si scrivano dettagliatamente tutti i
vincoli per C e per gli eventuali member type utilizzati nell’implementazione.
Esempio:il tipoCdeve avere il tal metodo, deve supportare il tal costruttore, il tal operatore ecc.; poi deve
avere un certo member type e quest’ultimo deve supportare il tal metodo, operatore ecc.

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```cpp
// Questo sorgente contiene le soluzioni dell 'esame scritto di PO2 del 5/9/2023 per ciò che
riguarda il quesito 2, ovvero la domanda che coinvolge C++.
// I quesiti 1-5 riguardanti Java sono in un sorgente Java a parte, non qui.
// Il codice C++ qui esposto è standard C++ vanilla (a.k.a. C++03), sebbene il progetto VS sia
configurato con il compilatore di default C++14

#include<iostream>
#include<vector>

usingnamespacestd;

template<classContainer>
ostream&operator<<(ostream&os,constContainer&c)
{
os<<"[";
for(typenameContainer::const_iteratorit=c.begin();it!=c.end();++it)
os<<""<<*it;
os<<"]";
returnos;
}


template<classContainer>
typenameContainer::value_typesum(constContainer&c)
{
typenameContainer::value_typer;
for(typenameContainer::const_iteratorit=c.begin();it!=c.end();++it)
{
r+=*it;
}
returnr;
}


intmain()
{
}
```

**Spiegazione rapida del ragionamento**

La sommatoria generica usa template + `operator+` per funzionare su qualsiasi tipo numerico. L'identita' additiva (valore zero) deve essere gestita con attenzione.

---

#### 2.2.4 Map

**Testo dell'esercizio**

[Esame 10/01/2024]
1.Si implementino in C++11 alcune varianti della funzione map()rispettando la sua semantica tipica: la funzione
passata come argomento allamap()deve essere applicata ad ogni elemento della sequenza di input, producendo
una sequenza di output con tutti i risultati delle applicazioni.
(a)Si implementi la seguente versione dellamap()che opera su iteratori. Si badi che in questo caso la funzione
non ha tipo di ritorno. L’output consiste in un iteratore passato come argomento: lì la funzione dovrà scrivere
i risultati.

```cpp
template<classInputIterator,classOutputIterator>
voidmap(InputIteratorfrom,InputIteratorto,OutputIteratorout,
function<typenameOutputIterator::value_type(typenameInputIterator::value_type)>f)
```

(b)Si implementi la seguente versione dellamap()che opera su vector di STL. In questo caso l’output è un vero
e proprio tipo di ritorno. L’implementazione deve invocare lamap()di cui al punto precedente.

```cpp
template<classA,classB>
vector<B>map(constvector<A>&v,function<B(A)>f)
```

(c)Il tipo templatizzatofunctionè definito da STL a partire dallo standard C++11, così come le espressioni
lambda. Nessuno dei due esisteva in C++vanilla(oggi chiamato C++03).
i.Come sarebbe stato possibile scrivere in C++03 le firme delle map()di cui ai punti (a) e (b) senza usare
il tipofunction?
ii.Le firme compatibili con C++03 potrebbero coesistere con le firme originali di cui ai punti ( a)e(b)? In
altre parole, sarebbero considerati tutti overload validi dal compilatore C++11?
iii.Come sarebbe stato possibile invocare lemap()compatibili con C++03 senza le lambda?

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```cpp
// Scritto PO2 10 9 24.cpp : This file contains the 'main'function. Program execution begins and
ends there.
//

#include<iostream>
#include<functional>
#include<vector>
#include<type_traits>

usingnamespacestd;

// 2.a
template<classInputIterator,classOutputIterator>
voidmap(InputIteratorfrom,InputIteratorto,OutputIteratorout,function<typename
OutputIterator::value_type(typenameInputIterator::value_type)>f)
{
while(from!=to)
*out++=f(*from++);
}

// 2.b
template<classA,classB>
vector<B>map(constvector<A>&v,function<B(A)>f)
{
vector<B>r(v.size());
map(v.begin(),v.end(),r.begin(),f);
returnr;
}

// 2.c
namespacecpp03{

// 2.c.i
template<classA,classB,classF>
vector<B>map(constvector<A>&v,Ff)
{
vector<B>r(v.size());
for(inti=0;i<v.size();++i)
r[i]=f(v[i]);

returnr;
}

// 2.c.ii
// no, non possono coesistere, perché sarebbero overload ambigui. Infatti ho dovuto metterli
in un sotto-namespace a parte per far compilare questo sorgente.

// 2.c.iii
// bisogna usare i function object, cioè oggetti per cui è definito l 'operatore di
applicazione operator()
// esempio:
classmyfunction{
public:
booloperator()(intn){returnn>2;}
};

voidtest()
{
vector<int>v1{1,2,3,4,5};
vector<bool>v2=map<int,bool>(v1,myfunction());// senza le annotazioni esplicite dei
template argument non compila (questo non è richiesto nell 'esame perché è una cosa
molto sottile)

}
}

// main for testing
//

template<classT>
ostream&operator<<(ostream&os,constvector<T>&v)
{
os<<"[ ";
for(autoit=v.begin();it!=v.end();++it)
os<<*it<<", ";
os<<"\b\b]";
returnos;
}

intmain()
{
vector<int>v1{1,2,3,4,5};
vector<bool>v2(v1.size());
map(v1.begin(),v1.end(),v2.begin(),[](intn){returnn>2;});

v2=map<int,bool>(v1,[](intn){returnn>2;});// anche qui bisogna mettere i template
argument espliciti

cout<<v1<<endl<<v2<<endl;

return0;
}
```

**Spiegazione rapida del ragionamento**

La `map` template usa `operator[]` per accesso chiave-valore. La gestione dell'allocazione rispetta RAII per evitare leak.

---

#### 2.2.5 Funzioni

**Testo dell'esercizio**

[Esame 30/06/2023]
1.Si implementi in C++ una classe templatizzata fun_seqanaloga a quella dell’esercizio2.1.7(Java), adottando
però i pattern, gli stili e le convenzioni di C++ e di STL. Alcuni suggerimenti:
•si usi il tipostd::pairdefinito da STL per rappresentare le coppie;
•l’iterabilità in C++ è rappresentata delconceptdenominatoContainerin STL;
•la funzione di incremento passata al costruttore in Java non è necessaria in C++ e può essere sostituita dagli
opportuni operatori;
•il vincoloComparablesul type parameterXin Java non ha equivalente in C++, è sufficiente assumere che il
relativo template argument supporti un operatore di confronto.
Si scriva anche uno snippet analogo a quello del punto c(Es.2.1.7Java).
11Non è necessaria nessuna revisione recente del linguaggio, l’esercizio è esprimibile in C++ vanilla, detto C++03.

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```cpp
exportmodulepairs;

import<iostream>;
import<functional>;

import<utility>;

usingnamespacestd;

export
template<classX,classY>
classfun_seq
{
private:
constXa,b,dx;
constfunction<Y(constX&)>f;

public:
usingvalue_type=pair<X,Y>;

fun_seq(constX&_a,constX&_b,constX&_dx,constfunction<Y(constX&)>&_f):
a(_a),b(_b),dx(_dx),f(_f){}

classiterator
{
private:
Xx;
fun_seq<X,Y>that;

public:
explicititerator(constX&_x,constfun_seq<X,Y>&_that):x(_x),that(_that)
{}

iterator(constiterator&it):x(it.x),that(it.that){}

pair<X,Y>operator*()const
{
returnpair<X,Y>(x,that.f(x));
}

booloperator!=(constiterator&it)
{
return!(x>it.x-that.dx&&x<it.x+that.dx);
}

iteratoroperator++()
{
iteratorr(*this);
x+=that.dx;
returnr;
}
};

iteratorbegin()const
{
returniterator(a,*this);
}

iteratorend()const
{
returniterator(b,*this);
}



};


exportintmain()
{
fun_seq<double,double>sq1(-2.,2.,0.1,[](constdouble&x){returnx*x+2*x-
1;});
for(pair<double,double>p:sq1)
cout<<"f("<<p.first<<") = "<<p.second<<endl;

fun_seq<double,int>sq2(-2.,2.,0.1,[](constdouble&x){returnint(x*x+2*x-
1);});
for(pair<double,int>p:sq2)
cout<<"f("<<p.first<<") = "<<p.second<<endl;

return0;
}
```

**Spiegazione rapida del ragionamento**

Le funzioni come callable (`operator()`) in C++ consentono composizione e stato locale (functors). Le lambda C++11+ sono zucchero sintattico sulla stessa idea.

---

#### 2.2.6 Curve

**Testo dell'esercizio**

[Esame 13/01/2023]
1.Si prenda in considerazione la seguente classe C++14 che rappresenta curve definite tramite funzioni unarieR↗R
in un certo intervallo di dominio[a, b]⇐R.

```cpp
#include<functional>
#include<iostream>
#include<utility>

usingnamespacestd;
usingreal=double;
usingunary_fun=function<real(constreal&)>;

#define RESOLUTION (1000) // risoluzione dell'intervallo [a, b]

classcurve
{
private:
reala,b;
unary_funf;
public:
curve(constreal&a_,constreal&b_,constunary_fun&f_):f(f_),a(a_),b(b_){}
curve(constreal&c)=default;

realget_dx()const{return(b-a)/RESOLUTION;}
pair<real,real>interval()const{returnpair<real,real>(a,b);}
realoperator()(constreal&x)const{returnf(x);}

curvederivative()const{/* DA IMPLEMENTARE */ }
curveprimitive()const{/* DA IMPLEMENTARE */ }
realintegral()const{/* DA IMPLEMENTARE */ }

classiterator
{
private:
constcurve&c;
realx;
public:
iterator(constcurve&c_,constreal&x_):c(c_),x(x_){}
iterator(constiterator&c)=default;

pair<real,real>operator*()const{/* DA IMPLEMENTARE */ }
iteratoroperator++(){/* DA IMPLEMENTARE */ }
iteratoroperator++(int){/* DA IMPLEMENTARE */ }
booloperator!=(constiterator&it)const{/* DA IMPLEMENTARE */ }
};

iteratorbegin()const{/* DA IMPLEMENTARE */ }
iteratorend()const{/* DA IMPLEMENTARE */ }
};
```

(a)Si implementi il metododerivative()che calcola la derivata, cioè la curva con una lambda che calcola il
rapporto differenzialef(x+dx)->f(x)
dx
(b)Si implementi il metodoprimitive()che calcola la primitiva, ovvero la curva che rappresenta l’integrale
indefinito con una lambda che calcola il prodotto differenziale f(x)·dx.
(c)Si implementi il metodointegral()che calcola l’integrale definito
∫b
af(x)dx, ovvero la sommatoria dei
prodotti differenziali calcolati dalla primitiva nell’intervallo[a, b].

(d)Si implementino i metodi relativi all’iteratore tenendo presente che:
•il metodobegin()computa un iteratore all’inizio dell’intervallo[a, b];
•il metodoend()computa un iteratore oltre la fine dell’intervallo[a, b], includendo l’estremobnell’itera-
zione;
•l’operatore di de-reference dell’iteratore produce una coppia di reali che rappresentano un punto della
curva sul piano cartesiano;
•gli operatori di pre e post incremento incrementano l’ascissa corrente xdella quantitàdx.
Il seguente snippet di test può servire da riferimento per capire il comportamento dell’iteratore:

```cpp
curvec(-10.,10.,[](constreal&x){returnx*x-2*x+1;});
for(curve::iteratorit=c.begin();it!=c.end();++it)
{
constpair<real,real>&p=*it;
constreal&x=p.first,&y=p.second;
cout<<"c("<<x<<") = "<<y<<endl;
}
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```cpp
#include<functional>
#include<iostream>
#include<utility>
#include<algorithm>

usingnamespacestd;


usingreal=double;
usingunary_fun=function<real(constreal&)>;

#define RESOLUTION (10)


classcurve
{

private:
reala,b;
unary_funf;

public:
curve(constreal&a_,constreal&b_,constunary_fun&f_):f(f_),a(a_),b(b_){}

realget_dx()const{return(b-a)/RESOLUTION;}

pair<real,real>interval()const{returnpair<real,real>(a,b);}

curvederivative()const
{
returncurve(a,b,[&,dx=get_dx()](constreal&x){ // anche la capture
[=] sarebbe stata sufficiente
constrealdy=f(x+dx)-f(x);
returndy/dx;
});
}


curveprimitive()const
{
returncurve(a,b,[&,dx=get_dx()](constreal&x){ // oppure la [&]
constrealy=f(x);
returny*dx;
});
}

realintegral()const
{
constunary_fun&F=primitive();
returnF(b)-F(a);
}

realoperator()(constreal&x)const
{
returnf(x);
}

classiterator
{
private:
constcurve&c;
realx;

public:
iterator(constcurve&c_,constreal&x_):c(c_),x(x_){}

pair<real,real>operator*()const
{
returnpair<real,real>(x,c.f(x));
}

iterator&operator++() // il pre-incremento modifica sè stesso e non
ritorna una copia
{
x+=c.get_dx();
return*this;
}

iteratoroperator++(int) // il post-incremento fa una copia, modifica sé
stesso e ritorna la copia
{
autor(*this);
++(*this);
returnr;
}

booloperator!=(constiterator&it)const
{
returnfabs(x-it.x)>=c.get_dx(); // non si confrontano mai i
float direttamente con l'operatore di uguaglianza o disuguaglianza
}
};

iteratorbegin()const

{
returniterator(*this,a);
}

iteratorend()const
{
returniterator(*this,b+get_dx());
}

};


ostream&operator<<(ostream&os,constcurve&c)
{
os<<"dom = ["<<c.interval().first<<", "<<c.interval().second<<"] dx = " <<
c.get_dx()<<": "<<endl;
/*for (const pair<real, real>& p : c)
{
const real& x = p.first, & y = p.second;
os << "\tf(" << x << ") = " << y << endl;
}*/
for(curve::iteratorit=c.begin();it!=c.end();++it)
{
constpair<real,real>&p=*it;
constreal&x=p.first,&y=p.second;
os<<"\tf("<<x<<") = "<<y<<endl;
}
returnos<<endl;
}

intmain()
{
curvef(-1.,1.,[](constreal&x){returnx*x;});

cout<<f<<endl
<<f.derivative()<<endl
<<f.primitive()<<endl
<<f.primitive().derivative()<<endl
<<f.derivative().primitive()<<endl
;
return0;
}
```

**Spiegazione rapida del ragionamento**

Le curve come gerarchia di classi con `operator<<` per stampa e metodi virtuali per calcolo permettono di trattare uniformemente curve diverse con interfaccia comune.

---

#### 2.2.7 Alberi binari

**Testo dell'esercizio**

[Esame 13/09/2022]
1.Vogliamo realizzare in C++ lo stesso sistema di alberi binari di cui al Quesito 2.1.9(Java). La traduzione del
codice da Java a C++ non deve essere letterale: è necessario adottare gli stili e le convenzioni di C++ e di STL, ad
esempio formulando il confronto tramite l’overloading degli opportuni operatori, definendo gli opportuni member
type per gli iteratori e implementando gli overload const e non-const dei metodi laddove necessario. La classe tree_node
deve avere un template parameterTed aderire allo stile dellavalue-oriented programming, comportandosi
come un valore con gli opportuni costruttori e operatori di assegnamento. Segue la specifica dettagliata.
(a)Gli alberi devono essereiterabili e l’iteratore deve attraversare l’albero in maniera algoritmicamente equiva-
lente all’implementazione Java di cui al Quesitoa(Es.2.1.9Java). Si definiscano imember typeper iteratori
const e non-const e le relative coppie di metodi begin()edend(). Se necessario, implementare gli iteratori
tramite classi ausiliarie.
(b)La definizione di equivalenza di due alberi è semanticamente equivalente a quella definita nel Quesito b(Es.
2.1.9Java). Si assuma che gli oggetti di tipo Tsiano confrontabili tramite l’operatore==.
(c)Si definiscano gli opportuni costruttori, operatori di assegnamento ed eventuali distruttori secondo lo stile della
value-oriented programming. Se vantaggioso, si forniscano metodi statici che fungano da pseudo-costruttori
e permettano di costruire alberi facilmente innestando ricorsivamente le chiamate.
(d)Si implementino uno o più snippet di test.
(e)Si implementi unpretty printertramite un overload globale dell’operatore<<.
Si utilizzi la revisione del linguaggio C++ che si preferisce, purché si specifichi quale. È importante fare buon uso
del type system, riusando il codice laddove possibile.

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```cpp

// Questo sorgente contiene le soluzioni dell 'esame scritto di PO2 del 1/7/2022 per ciò che
riguarda il quesito 2, ovvero l 'esercizio di C++.
// Il quesito 1 riguardante Java è in un progetto IntelliJ a parte, non qui.
// Il codice qui esposto è C++14.
// ATTENZIONE: il codice qui fornito è ricco di dettagli e complessità, allo scopo di fornire
materiale di studio. La versione richiesta all 'esame è molto più semplice.

#include<iostream>
#include<iterator>
#include<cstddef>
#include<vector>


usingstd::vector;

#define EASY_ITERATOR // commentare questa macro per compilare la versione ottimizzata
degli iteratori

template<classT>
classtree_node
{
public:
Tdata;
tree_node<T>*left,*right;

staticboolare_equal(consttree_node<T>*a,consttree_node<T>*b)
{
returna==b||(a!=nullptr&&b!=nullptr&&*a==*b);
}

public:
// 2.c

tree_node()=default;
tree_node(consttree_node<T>&t)=default;
tree_node<T>&operator=(consttree_node<T>&t)=default;

~tree_node()
{
if(left!=nullptr)
{
deleteleft;
left=nullptr;
}
if(right!=nullptr)
{
deleteright;
right=nullptr;
}
}

tree_node(constT&v,tree_node<T>*l,tree_node<T>*r):data(v),left(l),right(r)
{
#i f d e f E A S Y _ I T E R A T O R
prepopulate();
#e l s e
if(left!=nullptr)left->parent=this;
if(right!=nullptr)right->parent=this;
#e n d i f
}

// 2.b

booloperator==(consttree_node<T>&t)const
{
returndata==t.data&&are_equal(left,t.left)&&are_equal(right,t.right);
}

// 2.a


usingvalue_type=T;


// implementazione facile degli iteratori, suggerita pubblicamente dal docente durante
l'appello del 13/9/22
//

#i f d e f E A S Y _ I T E R A T O R

usingconst_iterator=typenamevector<T>::const_iterator;
usingiterator=typenamevector<T>::iterator;

private:
// per motivi di semplicità ogni nodo ha un campo di tipo vector che viene popolato al
volo per essere iterator
// si faccia attenzione ad un particolare: per poter ritornare begin() ed end() dello
stesso vector, bisogna conservarlo come membro del nodo
vector<T>children;

voiddfs(vector<T>&v)
{
// perché non popoliamo direttamente il campo children? il motivo è molto
sottile: ogni nodo ha un suo campo children, ma noi non vogliamo che ogni
nodo popoli il proprio vector; noi vogliamo

// che quando decidiamo di popolare il campo di children di un certo nodo,
allora viene popolato il vector di QUEL nodo
// ogni nodo quindi ha un suo campo children che viene popolato con i SUOI
sottorami: tutto ciò è uno spreco di spazio, certo, però permette di fare
iteratori a partire da QUALUNQUE nodo

// in maniera semplice e immutabile
v.push_back(data);
if(left!=nullptr)left->dfs(v);
if(right!=nullptr)right->dfs(v);
}

// questa viene chiamata dal costruttore: ogni nodo prepopola il proprio campo children
voidprepopulate()
{
dfs(children);
}

public:

const_iteratorbegin()const
{
returnchildren.begin();
}

const_iteratorend()const
{
returnchildren.end();
}

iteratorbegin()
{
returnchildren.begin();
}


iteratorend()
{
returnchildren.end();
}


// implementazione ottimizzata degli iteratori
//

#e l s e

private:
tree_node<T>*parent;

statictree_node<T>*get_next_node(consttree_node<T>*n){
if(n->left!=nullptr)
returnn->left;
elseif(n->right!=nullptr)
returnn->right;
else{
while(n->parent!=nullptr){
consttree_node<T>*last=n;
n=n->parent;
if(n->right!=nullptr&&n->right!=last)
returnn->right;
}
returnnullptr;
}
}

public:
// iteratore non-const
classmy_iterator
{
friendclassmy_const_iterator; // questo serve perché altrimenti
my_const_iterator non può accedere al campo current di my_iterator

private:
tree_node<T>*current;

public:
usingiterator_category=std::forward_iterator_tag; // questo member type
indica che si tratta di un ForwardIterator, si veda la doc di STL per i
dettagli

usingdifference_type=std::ptrdiff_t;
usingvalue_type=T;
usingpointer=T*;
usingreference=T&;

my_iterator()=default;
my_iterator(constmy_iterator&i)=default;
my_iterator&operator=(constmy_iterator&i)=default;

my_iterator(tree_node<T>*t):current(t){}

referenceoperator*(){returncurrent->data;}

pointeroperator->(){return&current->data;}

my_iterator&operator++()
{
current=get_next_node(current);
return*this;
}

my_iteratoroperator++(int)
{
my_iteratorr(*this);
current=get_next_node();
returnr;
}

booloperator==(constmy_iterator&i)const{returncurrent==i.current;}
booloperator!=(constmy_iterator&i)const{return!(*this==i);}

};

// interatore const
// si noti come sono praticamente uguali a parte il fatto che gesticono un nodo const
oppure no, con conseguente impatto in tutti i tipi di ritorno dei vari operatori
// questa replicazione di codice sarebbe evitabile solamente tramite un complesso uso dei
template, troppo complesso per questo corso
// chi è interessato a sapere come evitare questa duplicazione di codice dovuta a const
vs. non-const può approfondire qui:
https://stackoverflow.com/questions/765148/how-to-remove-constness-of-const-iterator

classmy_const_iterator
{
private:
consttree_node<T>*current;

public:
usingiterator_category=std::forward_iterator_tag;
usingdifference_type=std::ptrdiff_t;
usingvalue_type=constT;
usingpointer=constT*;
usingreference=constT&;

my_const_iterator()=default;
my_const_iterator(constmy_const_iterator&i)=default;
my_const_iterator&operator=(constmy_const_iterator&i)=default;

// questo costruttore è molto interessante: permette di costruire un
my_const_iterator dato un my_iterator: in altre parole possiamo convertire un
iteratore non-const in uno const

// il motivo per cui è necessario è se chiamiamo begin() su un tree_node
non-const ma vogliamo un const_iterator perché lo leggiamo soltanto
my_const_iterator(constmy_iterator&i):current(i.current){}

my_const_iterator(consttree_node<T>*t):current(t){}

referenceoperator*()const {returncurrent->data; }
pointeroperator->()const{return&current->data; }

my_const_iterator&operator++()

{
current=get_next_node(current);
return*this;
}

my_const_iteratoroperator++(int)
{
my_const_iteratorr(*this);
current=get_next_node();
returnr;
}

booloperator==(constmy_const_iterator&i)const{ returncurrent==
i.current;}
booloperator!=(constmy_const_iterator&i)const{ return!(*this==i);
}

};

// definiamo questi member type perché sono quelli che i Container STL solitamente
definiscono
usingconst_iterator=my_const_iterator; // rebinding della nested class
my_const_iterator definita sopra
usingiterator=my_iterator; // rebinding della nested
class my_iterator definita sopra
usingvalue_type=T;

const_iteratorbegin()const
{
returnconst_iterator(this);
}

const_iteratorend()const
{
returnconst_iterator(nullptr);
}

iteratorbegin()
{
returniterator(this);
}

iteratorend()
{
returniterator(nullptr);
}

#e n d i f

};

// 2.c
// pseudo-costruttori
// invece di fare metodi statici facciamo funzioni templatizzate globali, così il template
argument è inferito e diventano più comode da usare

template<classT>

tree_node<T>*lr(constT&v,tree_node<T>*l,tree_node<T>*r)
{
returnnewtree_node<T>(v,l,r);
}

template<classT>
tree_node<T>*l(constT&v,tree_node<T>*n)
{
returnnewtree_node<T>(v,n,nullptr);
}

template<classT>
tree_node<T>*r(constT&v,tree_node<T>*n)
{
returnnewtree_node<T>(v,nullptr,n);
}

template<classT>
tree_node<T>*v(constT&v)
{
returnnewtree_node<T>(v,nullptr,nullptr);
}

// 2.e

template<classT>
std::ostream&operator<<(std::ostream&os,consttree_node<T>&t)
{
os<<t.data;
if(t.left!=nullptr)os<<"("<<*t.left<<")";
if(t.right!=nullptr)os<<"["<<*t.right<<"]";
returnos;
}

usingnamespacestd;

// 2.d

intmain()
{
autot1=
shared_ptr<tree_node<int>>( // usiamo gli shared_ptr per non
doverci ricordare di fare delete
// con i pseudo-costruttori globali è comodissimo costruire un albero,
basta innestare le chiamate
lr(1,
lr(2,
v(3),
v(4)),
r(5,
lr(6,
v(7),
v(8)))));

autot2=
shared_ptr<tree_node<int>>(
lr(1,

r(5, // il sottoalbero
destro di t2 è uguale al sinistro di t1 e viceversa
lr(6,
v(7),
v(8))),
lr(2,
v(3),
v(4))));

// test dell'operatore di stream (<<)
cout<<"pretty printer: "<<endl
<<"t1: "<<*t1<<endl // dereferenziamo per stampare perché il nostro
operator<< non vuole un pointer ma un reference
<<"t2: "<<*t2<<endl;

// test dell'operatore di uguaglianza (==)
cout<<"equality: "<<(*t1==*t2)<<", "<<(*t1->left==*t2->right)<<
endl; // dereferenziamo gli operandi sinistro e destro del nostro operator==
perchè non accetta pointer ma reference


// test dell'iteratore non-const
cout<<"iterator: ";
for(tree_node<int>::iteratorit=t1->begin();it!=t1->end();++it)
{
int&n=*it; // dereferenziando l'iteratore abbiamo accesso
non-const al dato dentro il nodo
n*=2; // il campo data in ogni nodo può quindi
essere modificato
cout<<n<<"";
}
cout<<endl<<"t1 modificato: "<<*t1<<endl; // ristampiamo t1 dopo le
modifiche

// test dell'iteratore const
cout<<"const iterator: ";
for(tree_node<int>::const_iteratorit=t1->begin();it!=t1->end();++it) //
t1->begin() ritorna un iterator, che viene convertito in un const_iterator dal
costruttore alla linea 142

{
constint&n=*it; // dereferenziando l'iteratore abbiamo accesso const
al dato dentro il nodo, quindi non possiamo modificarlo ma solo leggerlo
cout<<n<<"";
}
cout<<endl;



}
```

**Spiegazione rapida del ragionamento**

L'albero binario value-oriented gestisce copia profonda, move e RAII tramite costruttori/distruttori. L'iteratore implementa la traversata in-order rispettando il contratto STL.

---

#### 2.2.8 Smart Pointer

**Testo dell'esercizio**

[Esame 01/07/2022]
1.Si definisca in linguaggio C++ una classe smart_ptrtemplatizzata su un tipoTche implementi la logica di uno
smart pointer. Si implementino tutti i costruttori, i distruttori e gli operatori che si ritengono necessari ed utili
affinché uno smart pointer si comporti in maniera compatibile con un pointer C. In altre parole, uno smart pointer
deve implementare non solo ilreference countingma deve anche comportarsi come un puntatore classico, inclusi
gli operatori di incremento/decremento, l’aritmetica dei puntatori ed ovviamente il de-reference.

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```cpp
// Questo sorgente contiene le soluzioni dell 'esame scritto di PO2 del 1/7/2022 per ciò che
riguarda il quesito 3, ovvero la domanda che coinvolge C++.
// I quesiti 1-2 riguardanti Java sono in un progetto IntelliJ a parte, non qui.
// Il codice qui esposto è C++14 per qualche piccolo particolare, ma in gran parte è
essenzialmente vanilla.

#include<iostream>

#include<vector>

template<classT>
classsmart_ptr
{
private:
T*pt;
ptrdiff_toffset;
boolis_array;

usingcounter_t=unsignedshort;

counter_t*cnt;

voiddec()
{
if(cnt!=nullptr&&--(*cnt)==0)
{
if(pt!=nullptr) // se non punta a nulla non c 'èn i e n t ed a
liberare
{
if(is_array)deletept;
elsedelete[]pt;
}
deletecnt;
}
}

voidinc()
{
if(cnt!=nullptr)++(*cnt);
}

public:
usingvalue_type=T;

smart_ptr():pt(nullptr),offset(0),is_array(false),cnt(nullptr){}

explicitsmart_ptr(T*pt_,boolis_array_=false):pt(pt_),offset(0),
is_array(is_array_),cnt(newcounter_t(1)){}

smart_ptr(constsmart_ptr<T>&p):pt(p.pt),offset(p.offset),is_array(p.is_array),
cnt(p.cnt)
{
inc();
}

~smart_ptr()
{
dec();
}

smart_ptr<T>&operator=(constsmart_ptr<T>&p)
{
dec();
pt=p.pt;
cnt=p.cnt;

offset=p.offset;
is_array=p.is_array;
inc();
return*this;
}

// subscript
// l'operatore di subscript è l 'unica implementazione reale; tutti gli altri operatori
sono implementati in funzione di questo
// questo approccio è poco error-prone perché concentra solamente qui il calcolo esatto
dell'indirizzo usando l'offset
// in altre parole, l 'operatore di subscript funge da API interna a basso livello; tutto
il resto è costruito sopra di essa
constT&operator[](size_ti)const
{
returnpt[offset+i];
}
T&operator[](size_ti)
{
// capita spesso che l 'implementazione const e l'implementazione non-const siano
identiche
// in questi casi è necessario duplicare il codice, che è una pratica inelegante
ed error-prone
// questo trucco sfrutta un giro di const cast per rimandare questa
implementazione a quella const appena sopra, che è l 'unica che implementiamo
davvero

// ATTENZIONE: tutto questo NON è richiesto dal tema d 'esame, lo mostriamo
solamente a scopo didattico per insegnare una tecnica avanzata di
non-duplicazione del codice

returnconst_cast<T&>(const_cast<constsmart_ptr<T>&>(*this).operator[](i));
}

// de-reference
constT&operator*()const
{
// usa l'operatore di subscript
returnpt[0];
}
T&operator*()
{
// stessa tecnica per non duplicare: chiamiamo la versione const di questo
operatore definita qui sopra, che è l 'unica che implementiamo davvero
returnconst_cast<T&>(const_cast<constsmart_ptr<T>&>(*this).operator*());
}

// field access
constT*operator->()const
{
// anche questa implementazione sfrutta altri operatori scritti sopra: in
particolare usa de-reference che a sua volta usa il subscript, in questo modo
non richiede manutenzione

return&*(*this);
// ^
// si faccia attenzione a un particolare: solo l 'operatore * indicato dalla
freccetta invoca l'overload definito da noi
// il de-reference di this dentro le parentesi tonde e l 'operatore & più esterno
invocano gli operatori nativi di C++, non i nostri overload

}
T*operator->()
{
// altro uso della tecnica avanzata per non duplicare l 'implementazione non-const
// cerchiamo di capirla: lo scopo è chiamare l 'implementazione const di questo
operatore senza duplicare il codice
// 1) trasformiamo *this (che in questo scope è di tipo smart_ptr<T>&) in un
const smart_ptr<T>&
// 2) ora che *this è castato a const, invochiamo l 'operatore o il metodo che ci
interessa: la risoluzione dell'overload risolverà l'implementazione const,
non farà una ricorsione!

// 3) siccome stiamo invocando la versione const, il risultato è const: ma il
nostro tipo di ritorno deve essere un T* non-const, pertanto bisogna
const-castare per TOGLIERE il const

returnconst_cast<T*>(const_cast<constsmart_ptr<T>&>(*this).operator->());
}

// plus
smart_ptr<T>&operator+=(ptrdiff_toff)
{
offset+=off;
return*this;
}
smart_ptr<T>operator+(ptrdiff_toff)const
{
// questa implementazione usa il copy-constructor e l 'operator+= definito qui
sopra
returnsmart_ptr<T>(*this)+=off;
}

// minus
smart_ptr<T>&operator-=(ptrdiff_toff)
{
offset-=off; // analogo al +=
return*this;
}
smart_ptr<T>operator-(ptrdiff_toff)
{
returnsmart_ptr<T>(*this)-=off;
}

// pre
smart_ptr<T>&operator++()
{
// questa implementazione usa l 'operator+= e basta
return*this+=1;
}
smart_ptr<T>&operator--()
{
return*this-=1; // analogo al ++ ma usa il -=
}

// post
smart_ptr<T>operator++(int)
{
smart_ptr<T>r(*this); // copia

++(*this); // pre-incrementa this: usiamo il
pre-incremento affinché questa implementazione dipenda totalmente
dall'implementazione di operator++

returnr; // ritorna la copia
}
smart_ptr<T>operator--(int)
{
smart_ptr<T>r(*this);
--(*this); // analogo a ++
returnr;
}

};

usingstd::string;

intmain()
{
int*a=newint[5];
smart_ptr<int>a1(a,true),a2;
a2=a1+10;
a1-=3;
a1=++a1-*a2--+a1[3];

smart_ptr<double>b(newdouble[10],true);
for(unsignedinti=0;i<10;++i)
b++;

string*sp=newstring("ciao");
smart_ptr<string>s1(sp),s2;
s2=s1;
size_ti=s2->find('a',0);



}
```

**Spiegazione rapida del ragionamento**

Lo smart pointer con reference counting (`shared_ptr`-like) implementa RAII: il distruttore decrementa il contatore e libera la memoria solo quando il count raggiunge zero.

---

## Sezione 3 – Domande aperte

Le seguenti domande verificano le conoscenze generali sugli argomenti del corso.

```cpp
Le seguenti domande servono a veriﬁcare le conoscenze dello studente relativamente agli argomenti coperti durante il
corso. Lo studente deve dimostrare di aver studiato l’argomento ed essere in grado di esporlo in modo adeguato.
1.Incapsulamento:s p i e g a r e c o s a s i i n t e n d e p e r m e t o d o s e t t e r .F o r n i r e a l m e n o u n a m o t i v a z i o n e p e r l a q u a l e l ’ u s o d i
setter è preferibile alla deﬁnizione di campi pubblici. Fare un esempio concreto (con codice) di un setter che svolge
una funzione non ottenibile con un campo pubblico.
2.Ereditarietà:s p i e g a r e a p p r o f o n d i t a m e n t e i n c o n c e t t o d i s o v r a s c r i t t u r a d i u n m e t o d o e q u a l i s o n o l e c o n d i z i o n i
nelle quali può essere utile. Fare un esempio pratico mostrando le necessarie porzioni di codice.
3.Espressioni Lambda:c o n s i d e r a r e l ’ i n t e r f a c c i aActionListenercon l’unico metodovoid actionPerformed(ActionEvent
e). Considerare ilJButton bem o s t r a r e( c o nc o d i c e )c o m eu t i l i z z a r ei lm e t o d ovoid addActionListener(ActionListener
l)per aggiungere unActionListenerche stampi a console la stringa "Java",c o d i ﬁ c a t or i s p e t t i v a m e n t ec o m e
una classe anonima e come un’espressione Lambda.
```
