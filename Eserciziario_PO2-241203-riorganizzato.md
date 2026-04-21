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

```text
[Esame10/09/2024– 04/09/2018]
1.Si scriva in linguaggio Java 8+ la ﬁrma e l’implementazione di un metodo statico e generico compareMany()che
confronta due collection. Dati due argomenti c1ec2aventi tipoCollection<A>eCollection<B>(doveAeB
sono i duetype parameterdel metodo), esso confronta ogni elemento di tipoAdella prima collection con l’elemento
di tipoBalla stessa posizione della seconda. Il confronto degli elementi deve avvenire chiamando il metodo co->
mpareTo()opportunamente: questo implica che ci sono dei vincoli sui generics Ae/oB. Il risultato del metodo
compareMany()èd it i p ointed e v er i s p e t t a r el as e m a n t i c ad e lc o n f r o n t oc o s i d d e t t oat r ev i e.S i i m p l e m e n t i l a
semantica del confronto nel seguente modo:
•c1ec2sono uguali se e solo se le lunghezze sono uguali e tutti gli elementi sono uguali;
•c1èm i n o r ed ic2se la lunghezza dic1èm i n o r eou g u a l eaq u e l l ad ic2es et u t t ig l ie l e m e n t id ic1sono
minori o uguali ai corrispettivi elementi dic2;
•altrimentic1èm a g g i o r ed ic2.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

_Nel PDF originale non e' presente una soluzione esplicita per questo esercizio._

**Spiegazione rapida del ragionamento**

Il confronto usa `compareTo()` per rispettare la semantica three-way. I vincoli sui generics A/B garantiscono che il confronto sia possibile staticamente, evitando cast a runtime.

---

#### 2.1.2 Thread Pool

**Testo dell'esercizio**

```text
[Esame 10/09/2024 –13/01/2023– 22/01/2019]S ip r e n d ai nc o n s i d e r a z i o n el as e g u e n t ei n t e r f a c c i aJ a v a :
1 publicinterfacePool<T,R>{
2 voidadd(Tx); // popola la pool con un nuovo elemento
3 Racquire()throwsInterruptedException;// acquisisce una risorsa
4 voidrelease(Rx); // rilascia una risorsa e la rimette nella pool
5 }
Una pool è un container di oggetti che si comporta come una coda bloccante: è possibile ottenere una risorsa con
acquire()per poi restituirla alla pool tramite il metodo release().
Il genericTastrae il tipo degli oggetti contenuti internamente nella pool, mentre Rèi lt i p od e l l ar i s o r s ar e s t i t u i t a
dallaacquire():i l m o t i v o p e r c u i s o n o d u e g e n e r i c d i ! e r e n t i è p e r c o n s e n t i r e a l l e c l a s s i c h e i m p l e m e n t a n o q u e s t a
interfaccia di rappresentare in modo diverso, se necessario, gli elementi conservati all’interno e le risorse restituite dalla
acquire().
Quando la coda è vuota e nessun oggetto è disponibile, il metodo acquire()deve essere bloccante: al ﬁne di
sempliﬁcare l’implementazione, si utilizzi un oggetto di tipo LinkedBlockingQueuecome campo interno. Riportiamo
un estratto della classeLinkedBlockingQueuedeﬁnita dal JDK con i metodi pubblici più signiﬁcativi:
1 classLinkedBlockingQueue<E>implementsBlockingQueue<E>{
2 LinkedBlockingQueue(); // costruttore
3 voidadd(Ex); // aggiunge un elemento
4 Etake()throwsInterruptedException; // estrae la testa (bloccante)
5 Epeek(); // ritorna la testa senza rimuoverla,
6 // oppure null se vuota
7
8 intsize(); // numero di elementi
9 // etc...
10 }
1.Si deﬁnisca una interfacciaBasicPoolche estende l’interfacciaPoolec h eh au ns o l og e n e r i cp e rr a p p r e s e n t a r es i a
il tipo degli elementi interni sia il tipo delle risorse restituite dalla acquire().
2.Si implementi una classeSimplePoolche implementa l’interfacciaBasicPooler e a l i z z au n as e m p l i c ec o d ab l o c -
cante.

3.Si implementi una classeAutoPoolche implementa l’interfacciaPoolrealizzando un meccanismo diauto-release
delle risorse. Sepèu n ap o o l ,l ’ o b i e t t i v oèf a r ei nm o d oc h eu n ar i s o r s axacquisita tramite la chiamatap.acqui->
re()non debba essere esplicitamente riconsegnata alla pool chiamandop.release(x),m as i ap o s s i b i l er i l a s c i a r l a
invocandox.release().
Suggerimento:s i d e ﬁ n i s c a u n n u o v o t i p o p a r a m e t r i c oResourceche si comporta come unproxyper l’oggetto
contenuto al suo interno e che implementa la logica di auto-releaserilasciandothis.
4.Si automatizzi il meccanismo di cui sopra facendo in modo che un oggetto di tipoResourcesi rilasciautonomamente
quando non esistono più riferimenti ad esso.
Suggerimento:l a s u p e r c l a s s eObjectdeﬁnisce un metodofinalize()che viene invocato nel momento in cui
l’oggetto viene cancellato dal garbage collector.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1
2 importjava.util.Random;
3 importjava.util.concurrent.BlockingQueue;
4 importjava.util.concurrent.LinkedBlockingQueue;
5
6 publicclassEs1{
7
8 publicinterfacePool<R>{
9 Racquire()throwsInterruptedException;
10 voidrelease(Rx);
11 }
12
13 // 2.b
14 publicstaticclassSimplePool<T>implementsPool<T>{
15
16 privatefinalBlockingQueue<T>q=newLinkedBlockingQueue<>();
17
18 @Override
19 publicTacquire()throwsInterruptedException{
20 returnq.take();
21 }
22
23 @Override
24 publicvoidrelease(Tx){
25 q.add(x);
26 }
27 }
28
29 // 2.c + 2.d
30
31 publicinterfaceResource<T>{
32 Tget();
33 voidautorelease();
34 }
35
36 publicstaticclassAutoPool<T>implementsPool<Resource<T>>{
37
38 privatefinalBlockingQueue<T>q=newLinkedBlockingQueue<>();
39
40 // questo metodo non è richiesto dall 'interfaccia AutoPool, però è utile perché serve a
popolare la pool, come accade nel main
41 publicvoidadd(Tx){
42 q.add(x);
43 }
44
45 publicResource<T>acquire()throwsInterruptedException{
46 Tr=q.take();
47 returnnewResource<>(){
48
49 @Override
50 publicTget(){
51 System.out.printf("acquired: %s%n",r);
52 returnr;
53 }

54
55 @Override
56 publicvoidautorelease(){
57 System.out.printf("released: %s%n",r);
58 add(r);
59 }
60
61 @SuppressWarnings("deprecation")
62 @Override
63 protectedvoidfinalize(){
64 autorelease();
65 }
66 };
67 }
68
69 @Override
70 publicvoidrelease(Resource<T>x){
71 x.autorelease();
72 }
73 }
74
75 publicstaticvoidmain(String[]args){
76 AutoPool<Integer>pool=newAutoPool<>();
77 Randomrnd=newRandom();
78 for(inti=0;i<5;++i){
79 pool.add(i); // popolo la pool con alcuni oggetti
80 }
81
82 try{
83 while(true){
84 Resource<Integer>r=pool.acquire();
85 System.out.println("using "+r.get());
86 Thread.sleep(Math.abs(rnd.nextInt()%1000));
87 }
88 }catch(InterruptedExceptione){
89 e.printStackTrace();
90 }
91
92 }
93
94
95 }
96
```

**Spiegazione rapida del ragionamento**

La `BlockingQueue` interna sincronizza produttori e consumatori senza lock espliciti. Il pattern `AutoPool` con `Resource` e `finalize()` implementa il rilascio automatico della risorsa (RAII-style in Java).

---

#### 2.1.3 Piano cartesiano

**Testo dell'esercizio**

```text
[Esame 25/06/2024 –06/09/2019]
1.Si implementi in Java 8+ un sistema di classi che rappresentano punti, linee e poligoni regolari nel piano cartesiano
R↔R.
(a)Si implementi una classe di nomePointche rappresenta punti bidimensionali immutabili, in cui le coordinate
xedysono di tipodouble.
(b)Si implementi una classe di nomeSegmentche rappresenta segmenti bidimensionali immutabili, il cui costrut-
tore prende due argomenti di tipoPoint.E s s a d e v e f o r n i r e u n m e t o d olength()che restituisce la lunghezza
del segmento calcolando la distanza euclidea tra i due punti.
Si implementi un tipo che rappresenta segmenti bidimensionali immutabili, ovvero una classe Lineil cui
costruttore prende due argomenti di tipo Point.E s s a d e v e f o r n i r e u n m e t o d olength()che ne calcola la
lunghezza tramite la distanza euclidea tra i due punti.
(c)Si prenda in considerazione la seguente classe astratta Polygonche rappresenta poligoni regolari come liste
di punti (minimo 3, veriﬁcato a runtime). I punti nella lista determinano l’ordine di costruzione dei segmenti
di cui è composto il poligono. Ad esempio, una lista contenente i seguenti 3 punti A=( 0,0),B=( 3,3)e
C=( 3,0)rappresenta un triangolo rettangolo in cui il primo lato è AB,i ls e c o n d oèBCed il terzo èCA.
1 publicabstractclassPolygon{
2 protectedfinalList<Point>points;
3 protectedPolygon(List<Point>points){
4 assertpoints.size()>=3;
5 this.points=points;
6 }
7 publicIterator<Line>lineIterator(){/* da implementare */ }
8 publicdoubleperimeter(){/* da implementare */ }
9 publicabstractdoublearea();
10 }
i.Per quale motivo è necessario vincolare a runtime la dimensione minima della lista di punti tramite un
assertanziché sfruttare in qualche modo il type system per fare un controllo statico? Si articoli una
breve risposta.
ii.Si implementi il metodolineIterator()che costruisce un iteratore su oggetti di tipoLinees ic o m p o r t a
come unwrapperdell’iteratore estratto dal campo points.G l i o g g e t t i p r o d o t t i d a l l ’ i t e r a t o r e d iL->
inedevono essere costruitidinamicamenteleggendo coppie di punti adiacenti dall’iteratore di Poin->
t.S i i m p l e m e n t i u n a l o g i c a d icachingdell’ultimo punto letto per permettere la costruzione di un
nuovo segmento adiacente all’ultimo ad ogni invocazione del metodo next().S i b a d i i n o l t r e a r i u s a r e
opportunamente il primo punto come secondo estremo dell’ultimo segmento costruito.
iii.Si implementi il metodo perimeter()in funzione del metodo lineIterator(),o v v e r oc a l c o l a n d oi l
perimetro del poligono iterando sui segmenti che lo compongono.
2.Estendiamo ora la gerarchia di classi introducendo tipi specializzati per i poligoni classici.
(a)Si implementi una sottoclasse diPolygondi nomeTriangleche rappresenta triangoli qualunque.

1 publicclassTriangleextendsPolygon{
2 publicTriangle(Pointp1,Pointp2,Pointp3){/* da implementare */ }
3 @Override
4 publicdoublearea(){/* da implementare */ }
5 }
Il costruttore prende 3 argomenti di tipo Pointed e v ec h i a m a r ei ls u p e r - c o s t r u t t o r eo p p o r t u n a m e n t e . S i
implementi il metodoarea()in modo che calcoli l’area del triangolo senza fare assunzioni sulla sua forma.
(b)Si implementi un tipo che rappresenta triangoli rettangoli tramite una sottoclasse di Triangle.S i b a d i i n
particolar modo a deﬁnire un costruttore che sintetizzi al massimo le informazioni passate come argomenti.
Si prediligano i controlli statici a quelli dinamici, se possibile, e si tenti di minimizzare i casi di ambiguità
og l is t a t id ii n v a l i d i t àd e l l ’ o g g e t t o ;n e lc a s oi nc u is ir i t e n g an e c e s s a r i of a r ed e ic o n t r o l l iar u n t i m e ,s iu s ii l
costruttoassert.
(c)Si implementi una sottoclasse diPolygondi nomeRectangleche rappresenta rettangoli.
1 publicclassRectangleextendsPolygon{
2 publicRectangle(Pointp1,Pointp3){/* da implementare */ }
3 @Override
4 publicdoublearea(){/* da implementare */ }
5 }
Il costruttore prende i 2 punti della diagonale e deve passare al super-costruttore i 4 punti che costituiscono
il rettangolo calcolandone le coordinate per proiezione ortogonale. Si badi all’ordine in cui i punti compaiono
nella lista, a"nché siano adiacenti e permettano di comporre i lati correttamente. Si implementi il metodo
area()in modo che calcoli l’area del rettangolo.
(d)Si implementi una sottoclasse diRectangledi nomeSquareche rappresenta quadrati.
1 publicstaticclassSquareextendsRectangle{
2 publicSquare(Pointp1,doubleside){/* da implementare */ }
3 }
Il costruttore prende il punto in basso a sinistra e la dimensione del lato e deve chiamare il super-costruttore
opportunamente.
3.Si prenda in considerazione il seguente codice, in cui compare l’invocazione di un metodo statico maxda deﬁnire:
1 Squaresq1=newSquare(newPoint(10.,-4.),0.1),
2 sq2=newSquare(newPoint(1.,20.),0.01);
3 Collection<Square>squares=List.of(sq1,sq2);
4 Rectangler=max(squares,newComparator<Polygon>(){
5 @Override
6 publicintcompare(Polygona,Polygonb){
7 return(int)(a.area()-b.area());
8 }
9 });
(a)Si scriva la ﬁrma e l’implementazione del metodo statico max()invocato nell’ultimo statement a"nché il
codice di cui sopra compili correttamente. Si renda tale metodo più generico possibile e non monomorfo
rispetto ai tipi che compaiono in questa invocazione, prestando particolare attenzione ai vincoli sui generics.
La semantica dimaxèf a c i l m e n t ei n t u i b i l e : t r o v al ’ e l e m e n t om a g g i o r eu s a n d oi lc o m p a r a t o r ep e rc o n f r o n t a r e
gli elementi della collection di input.
(b)Assumendo il comportamento corretto del metodomax(),q u a l ed e l l es e g u e n t ie s p r e s s i o n ib o o l e a n eèv e r a ?
->sq1==r ->sq2==r ->r==null ->nessuna delle precedenti
(c)Quale dei seguenti numeri razionali rappresenta il valore di tipodoublecomputato dall’espressioner.area()?
->10->1 ->10->2 ->10->4 ->non è un quadrato ma un rettangolo
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 importjava.util.*;
2
3 publicclassEs{
4
5 // 1.a
6 publicstaticclassPoint{
7 publicfinaldoublex,y;
8
9 publicPoint(doublex,doubley){
10 this.x=x;
11 this.y=y;
12 }

13 }
14
15 // 1.b
16 publicstaticclassLine{
17 privatefinalPointp1,p2;
18
19 publicLine(Pointp1,Pointp2){
20 this.p1=p1;
21 this.p2=p2;
22 }
23
24 publicdoublelength(){
25 returnMath.sqrt(Math.pow(p1.x-p2.x,2.)+Math.pow(p1.y-p2.y,2.));
26 }
27 }
28
29 // 1.c
30 publicabstractstaticclassPolygon{
31 protectedfinalList<Point>points;
32
33 protectedPolygon(List<Point>points){
34 assertpoints.size()>=3;// 1.c.i: RISPOSTA: il motivo per cui è necessario questo
assert è che non è possibile vincolare la lunghezza di una lista con il type
system: i tipi non esprimono valori numerici statici

35 this.points=points;
36 }
37
38 // 1.c.ii
39 publicIterator<Line>lineIterator(){
40 returnnewIterator<>(){
41 privatefinalIterator<Point>it=points.iterator();
42 privatePointlast=it.next(); // ci sono sempre almeno 3 punti, non può
fallire questa next()
43
44 @Override
45 publicbooleanhasNext(){
46 returnit.hasNext();
47 }
48
49 @Override
50 publicLinenext(){
51 Pointp=it.next();
52 Liner=newLine(last,p);
53 last=p;
54 returnr;
55 }
56 };
57 }
58
59 // 1.c.iii
60 publicdoubleperimeter(){
61 Iterator<Line>lineIt=lineIterator();
62 doubler=0.;
63 while(lineIt.hasNext()){
64 r+=lineIt.next().length();
65 }
66 returnr;

67 }
68
69 publicabstractdoublearea();
70 }
71
72 // 2.a
73 publicstaticclassTriangleextendsPolygon{
74 publicTriangle(Pointp1,Pointp2,Pointp3){
75 super(List.of(p1,p2,p3));
76 }
77
78 @Override
79 publicdoublearea(){
80 Pointp1=points.get(0),p2=points.get(1);
81 // versione facile con la base orizzontale (sufficiente per la valutazione
dell'esame)
82 if(p1.x==p2.x){
83 doublebase=newLine(p1,p2).length(),h=newLine(newPoint(p2.x,p1.y),
p2).length();
84 returnbase*h/2.;
85 }
86 // versione generale con la formula di Erone (non necessaria per la valutazione
dell'esame)
87 else{
88 Pointp3=points.get(2);
89 doublep=perimeter()/2., // semiperimetro
90 a=newLine(p1,p2).length(),
91 b=newLine(p2,p3).length(),
92 c=newLine(p3,p1).length();
93 returnMath.sqrt(p*(p-a)*(p-b)*(p-c));
94 }
95 }
96
97 }
98
99 // 2.b
100 publicstaticclassRightTriangleextendsTriangle{
101 publicRightTriangle(Pointp1,doubleb,doubleh){
102 super(p1,newPoint(p1.x+b,p1.y),newPoint(p1.x,p1.y+h));
103 }
104 }
105
106 // 2.c
107 publicstaticclassRectangleextendsPolygon{
108 publicRectangle(Pointp1,Pointp3){
109 super(List.of(p1,newPoint(p1.x,p3.y),p3,newPoint(p3.x,p1.y)));
110 }
111
112 @Override
113 publicdoublearea(){
114 Pointp1=points.get(0),p2=points.get(1),p4=points.get(3);
115 Lineb=newLine(p1,p4),h=newLine(p1,p2);
116 returnb.length()*h.length();
117 }
118 }
119
120 // 2.d

121 publicstaticclassSquareextendsRectangle{
122 publicSquare(Pointp1,doubleside){
123 super(p1,newPoint(p1.x+side,p1.y+side));
124 }
125
126 }
127
128 // 3.a
129 static<T>Tmax(Collection<T>l,Comparator<?superT>cmp){
130 assertl.size()>1;
131 Tmax=l.iterator().next(); // prendo il primo elemento
132 for(Tx:l){
133 if(cmp.compare(x,max)>0)max=x;
134 }
135 returnmax;
136 }
137
138 publicstaticvoidmain(String[]args){
139 Squaresq1=newSquare(newPoint(10.,-4.),0.1),
140 sq2=newSquare(newPoint(1.,20.),0.01);
141 Collection<Square>squares=List.of(sq1,sq2);
142 Rectangler=max(squares,newComparator<Polygon>(){
143 @Override
144 publicintcompare(Polygona,Polygonb){
145 return(int)(a.area()-b.area());
146 }
147 });
148 // 3.b: il metodo max() ritorna sq2 perché la sottrazione nel confronto è invertita,
quindi la ricerca del massimo in realtà restituisce il più piccolo anziché il più
grande;

149 // ed il quadrato con area minore è sq2, perché 0.01^2 = 0.0001 mentre 0.1^2 = 0.01,
quindi sq2 è di fatto il più piccolo.
150
151 // 3.c: l'area di r (cioè di sq2) è 0.01^2 = 0.0001 = 10^-4
152 }
153
154 }
```

**Spiegazione rapida del ragionamento**

La gerarchia `Point -> Line -> Polygon -> Triangle/Rectangle` separa correttamente le responsabilita'. L'uso di `assert` nel costruttore di `Polygon` e' necessario perche' il type system non puo' esprimere vincoli numerici statici sulle dimensioni di una lista.

---

#### 2.1.4 F unzioni di ordine superiore

**Testo dell'esercizio**

```text
[Esame 14/06/2024]
1.Si implementino le seguenti funzioni di ordine superiore in Java 8+.

(a)La prima è una variante della classica funzione map()1che opera su iteratori anziché su collection. L’itera-
tore restituito in output deve applicare la funzione fac i a s c u ne l e m e n t od it i p oAfornito dall’iteratoreit,
producendo oggetti di tipoB.
1 static<A,B>Iterator<B>mapIterator(Iterator<A>it,Function<A,B>f)
(b)La seconda funzione di ordine superiore è semanticamente equivalente al costrutto for-each: ad ogni oggetto
di tipoTnell’iterable in input viene applicato il consumerf.
1 static<T>voidforEach(Iterable<T>it,Consumer<T>f)
(c)Si implementi un tipoPairparametrico su due tipiAeB.S is c e l g al i b e r a m e n t es ef o r n i r eu n ai m p l e m e n t a z i o n e
mutabile o immutabile.
(d)Si prenda ora in considerazione la seguente ﬁrma di funzione:
1 static<A,B>Iterator<B>applyFuns(Iterable<Pair<Function<A,B>,A>>l)
Per ogni coppia dell’iterable in input, essa deve applicare la funzione che si trova nella prima componente
della coppia all’oggetto di tipoAche si trova nella seconda componente. Si implementi la suddetta funzione
tramiteuna singola invocazionedellamapIterator()di cui al punto (a).
(e)Si prenda ora in considerazione la seguente ﬁrma di funzione:
1 static<T>voidacceptFuns(Iterable<Pair<Consumer<T>,T>>l)
Per ogni coppia dell’iterable in input, essa deve applicare la funzione consumer che si trova nella prima
componente della coppia all’oggetto di tipo Tche si trova nella seconda componente. Si implementi la
suddetta funzionetramite una singola invocazionedellaforEach()di cui al punto (b).
(f)Con riferimento al punto (a)s ic o n s i d e r il as e g u e n t eﬁ r m ad if u n z i o n e :
1 static<A,B>Iterator<Supplier<B>>asyncMapIterator(Iterator<A>it,Function<A,B>f)
Essa consiste in una variante asincrona della mapIterator()in cui l’applicazione della funzione fad ogni
elemento di tipoAprodotto dall’iteratore in input deve avere luogo ogni volta in un thread nuovo. In
altre parole, ogni computazione della funzione fdeve avvenire concorrentemente. Il risultato di ciascuna
computazione, naturalmente, non può essere ritornato subito dallanext()dall’iteratore in output, altrimenti
sarebbe necessario attendere la ﬁne di ciascun thread, vaniﬁcando ogni forma di concorrenza. Per questo
motivo l’iteratore in output produce Supplier<B>anziché oggetti di tipoB.S o n oiSupplier2che devono
attendere la ﬁne dell’esecuzione dei thread.
Suggerimento:l’implementazione è piuttosto contenuta, non servono datatype di appoggio né metodi ausil
iari. Si sfrutti al massimo lo scoping, in particolare le chiusure delle lambda (o delle anonymous class, se si
preferisce).
(g)Con riferimento al punto (b)s ic o n s i d e r il as e g u e n t eﬁ r m ad if u n z i o n e :
1 static<T>voidasyncForEach(Iterable<T>it,Consumer<T>f)
Questa è una variante asincrona della forEach():l ’ a p p l i c a z i o n e d e l l a f u n z i o n efad ogni elemento di tipo
Tcontenuto nell’iterable in input deve avere luogo ogni volta in un thread nuovo. In altre parole, ogni
computazione della funzionefdeve avvenire concorrentemente.
Suggerimento:poiché iConsumernon hanno risultato, non si pone il problema di attendere la ﬁne delle
computazioni dei thread.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 importjava.util.ArrayList;
2 importjava.util.Iterator;
3 importjava.util.List;
4 importjava.util.function.Consumer;
5 importjava.util.function.Function;
6 importjava.util.function.Supplier;
7
8 publicclassEs1{
9
10 publicstatic<A,B>Iterator<B>mapIterator(Iterator<A>it,Function<A,B>f){
11 returnnewIterator<>(){
12 @Override
13 publicbooleanhasNext(){
14 returnit.hasNext();
15 }
16
17 @Override
18 publicBnext(){

19 returnf.apply(it.next());
20 }
21 };
22 }
23
24 publicstatic<T>voidforEach(Iterable<T>it,Consumer<T>f){
25 for(Tx:it)
26 f.accept(x);
27 }
28
29 publicstaticclassPair<X,Y>{
30 publicfinalXfst;
31 publicfinalYsnd;
32 publicPair(Xx,Yy){
33 fst=x;
34 snd=y;
35 }
36 }
37
38 publicstaticvoidmain1(String[]args){
39 List<Pair<Function<String,Integer>,String>>l=newArrayList<>();
40 Iterator<Integer>it=mapIterator(l.iterator(),(p)->p.fst.apply(p.snd));
41 }
42
43 publicstatic<A,B>Iterator<B>applyFuns(Iterable<Pair<Function<A,B>,A>>l){
44 returnmapIterator(l.iterator(),(p)->p.fst.apply(p.snd));
45 }
46
47 publicstaticvoidmain2(String[]args){
48 List<Pair<Consumer<Integer>,Integer>>l=newArrayList<>();
49 forEach(l,(p)->p.fst.accept(p.snd));
50 }
51
52 publicstatic<A>voidacceptFuns(Iterable<Pair<Consumer<A>,A>>l){
53 forEach(l,(p)->p.fst.accept(p.snd));
54 }
55
56 publicstatic<A,B>Iterator<Supplier<B>>asyncMapIterator(Iterator<A>it,Function<A,B>
f){
57 returnnewIterator<>(){
58 @Override
59 publicbooleanhasNext(){
60 returnit.hasNext();
61 }
62
63 @Override
64 publicSupplier<B>next(){
65 finalAx=it.next();
66 finalB[]r=(B[])newObject[1];
67 Threadt=newThread(()->{r[0]=f.apply(x);});
68 t.start();
69 return()->{
70 try{
71 t.join();
72 }catch(InterruptedExceptione){
73 thrownewRuntimeException(e);
74 }

75 returnr[0];
76 };
77 }
78 };
79 }
80
81 publicstatic<T>voidasyncForEach(Iterable<T>it,Consumer<T>f){
82 for(Tx:it)
83 newThread(()->f.accept(x)).start();
84 }
85 }
```

**Spiegazione rapida del ragionamento**

Le funzioni di ordine superiore (`Function<A,B>`) consentono di iniettare comportamento come parametro, rendendo il codice aperto all'estensione senza modificare la struttura.

---

#### 2.1.5 F attoriale con thread

**Testo dell'esercizio**

```text
[Esame10/01/2024– 30/05/2019]
1.Si consideri un metodo statico in Java 8+ di nome parallelFactorial()che data unaCollection<Integer>
produce unaCollection<FactorialThread>.P e r o g n i i n t e r o d e l l a c o l l e c t i o n d i i n p u t v i e n e e ! e t t u a t o l ospawning
di un nuovo thread che ne computa il fattoriale e ne conserva il risultato in qualche modo. Tutti i thread creati
vengono ritornati nella collection di output, che naturalmente ha la stessa lunghezza della collection di input.
(a)Si implementi la classe FactorialThreadin modo che estenda java.lang.Threadef o r n i s c au nm e t o d o
getter per l’accesso al risultato della computazione del fattoriale. Si presti attenzione all’attesa della ﬁne della
computazione, gestendo opportunamente la cosa.
1Conosciuta anche con il nome ditransform()in certe librerie.
2Si rammenti che unSupplier<T>non è altro che unafunctional interfacecon un solo metodoget()che non ha parametri e ritorna un
oggetto di tipoT.

(b)Si implementi il metodo staticoparallelFactorial()avente la seguente ﬁrma:
1 staticCollection<FactorialThread>parallelFactorial(Collection<Integer>c)
Ogni thread deve lavorare concorrentemente agli altri, ciascuno calcolando il fattoriale di un intero proveniente
dalla collection di input. La funzione parallelFactorial()non deve attendere la ﬁne delle computazioni,
ma deve ritornare subito la collection di output.
(c)Si ra"ni la ﬁrma del metodo statico parallelFactorial()di cui al punto precedente in modo che il tipo
di input ed il tipo di output siano, rispettivamente, più generale e più specializzato possibile, senza tuttavia
rivelare dettagli sull’implementazione.
(d)Si scriva uno snippet di codice che testi il metodo statico parallelFactorial()stampando i risultati di
ciascuna computazione.
(e)Si dia una seconda implementazione del metodo statico parallelFactorial()usando la funzione di ordine
superiore map()3. In particolare si proceda nel seguente modo:
i.Si implementi un metodo staticomap()avente la seguente ﬁrma:
1 static<A,B>List<B>map(Iterable<A>i,Function<A,B>f)
Esso deve applicare la funzione fad ogni elemento di iep r o d u r r ei no u t p u tt u t t iir i s u l t a t id e l l e
applicazioni.
ii.Si reimplementiparallelFactorial()tramite una singola invocazione dellamap().
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 importjava.util.ArrayList;
2 importjava.util.Collection;
3 importjava.util.List;
4 importjava.util.function.Function;
5
6 publicclassEs1{
7
8 // 1.a
9 publicstaticclassFactorialThreadextendsThread{
10 privatefinalintn;
11 privatelongres;
12
13 publicFactorialThread(intn){
14 this.n=n;
15 }
16
17 @Override
18 publicvoidrun(){
19 res=fact(n);
20 }
21
22 publiclonggetResult(){
23 try{
24 join();
25 }catch(InterruptedExceptione){
26 thrownewRuntimeException(e);
27 }
28 returnres;
29 }
30
31 publicintgetN(){
32 returnn;
33 }
34
35 privatestaticlongfact(intn){
36 if(n<=1)return1;
37 returnn*fact(n-1);
38 }
39
40 }
41
42 // 1.b + 1.c
43 publicstaticList<FactorialThread>parallelFactorial(Iterable<Integer>c){
44 List<FactorialThread>r=newArrayList<>();

45 for(intn:c){
46 FactorialThreadt=newFactorialThread(n);
47 t.start();
48 r.add(t);
49 }
50 returnr;
51 }
52
53 // 1.d
54 publicstaticvoidmain(String[]args){
55 for(FactorialThreadt:parallelFactorial(List.of(0,1,2,3,11,12,23,35))) //
chiamare anche parallelFactorial2() per provarla
56 System.out.printf("fact(%d) = %d\n",t.getN(),t.getResult());
57 }
58
59 // 1.e.i
60 publicstatic<A,B>List<B>map(Iterable<A>i,Function<A,B>f){
61 List<B>r=newArrayList<>();
62 for(Aa:i)
63 r.add(f.apply(a));
64 returnr;
65 }
66
67 // 1.e.ii
68 publicstaticCollection<FactorialThread>parallelFactorial2(Collection<Integer>c){
69 returnmap(c,(n)->{FactorialThreadt=newFactorialThread(n);t.start();returnt;
});
70 }
71 }
```

**Spiegazione rapida del ragionamento**

La suddivisione del calcolo su thread separati riduce i tempi. Il thread principale raccoglie i risultati parziali tramite una struttura condivisa protetta da sincronizzazione.

---

#### 2.1.6 Iteratori asincroni

**Testo dell'esercizio**

```text
[Esame 05/09/2023]
1.Si implementi un metodo statico generico in Java 8+ che, dato un iteratore ed una funzione, produce un nuovo
iteratore che in maniera asincrona applica la funzione ad ogni elemento dell’iteratore originale. Ciò signiﬁca che
un thread diverso deve processare ciascun elemento.
(a)Si implementi tutto ciò che è necessario dello snippet seguente.
1 static<A,B>Iterator<Supplier<B>>asyncIterator(Iterator<A>it,Function<A,B>f){
2 returnnewIterator<>(){
3 @Override
4 publicbooleanhasNext(){/* da implemetare */ }
5
6 privateclassFutureimplementsSupplier<B>{
7 publicFuture(Supplier<B>f){/* da implementare */ }
8
9 /* da completare/implementare */
10 }
11
12 @Override
13 publicSupplier<B>next(){/* da implementare */ }
14 };
15 }
Si badi che la classe innestata Futureha lo scopo di sempliﬁcare l’implementazione della next().E s s a
rappresenta una computazione in corso che non è ancora terminata. Essendo di fatto un Supplier,s a r à
possibile conoscere il risultato invocando il metodo get().N a t u r a l m e n t e q u e s t o r i s u l t a t o s a r à p o s s i b i l e
ottenerlo solamente dopo aver atteso che il relativo thread abbia ﬁnito di computare il Supplierpassato in
costruzione.
(b)Si implementi il seguente metodo statico in modo che utilizzi il metodo statico asyncIterator()di cui
all’esercizio precedente per scorrere l’argomento iterabile e ordinare in maniera asincrona i suoi elementi 4.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 importorg.jetbrains.annotations.NotNull;
2 importorg.jetbrains.annotations.Nullable;
3
4 importjava.util.Collections;
5 importjava.util.Iterator;
6 importjava.util.List;
7 importjava.util.function.Function;
8 importjava.util.function.Supplier;
9
10 publicclassMain{
11
12 // 1.a
13 publicstatic<A,B>Iterator<Supplier<B>>asyncIterator(Iterator<A>it,Function<A,B>f){
14 returnnewIterator<>(){
15 @Override
16 publicbooleanhasNext(){
17 returnit.hasNext();
18 }
19
20 privateclassFutureimplementsSupplier<B>{
21 @Nullable
22 privateBr;
23 @NotNull
24 privatefinalThreadt;
25
26 publicFuture(Supplier<B>f){

27 t=newThread(()->{r=f.get();});
28 t.start();
29 }
30
31 @Override
32 @NotNull
33 publicBget(){
34 try{
35 t.join();
36 }catch(InterruptedExceptione){
37 thrownewRuntimeException(e);
38 }
39 returnr;
40 }
41 }
42
43 @Override
44 publicSupplier<B>next(){
45 Aa=it.next();
46 returnnewFuture(()->f.apply(a));
47 }
48 };
49 }
50
51 // 1.b
52 static<TextendsComparable<?superT>>voidsortLists(Iterable<List<T>>c){
53 Iterator<Supplier<List<T>>>it=asyncIterator(c.iterator(),(l)->{
Collections.sort(l);returnl;});
54 while(it.hasNext()){
55 Supplier<List<T>>f=it.next();
56 System.out.println(f.get()); // questo non è davvero necessario, ma
57 }
58 }
59
60 }
```

**Spiegazione rapida del ragionamento**

Gli iteratori asincroni separano la produzione (thread interno) dal consumo (thread chiamante), utile quando gli elementi arrivano in modo non deterministico.

---

#### 2.1.7 F unzioni

**Testo dell'esercizio**

```text
[Esame 30/06/2023]
3La funzionemap()qui richiesta è equivalente a quella che JDK e STL chiamano transform().
4Si ricordi che il JDK fornisce un metodo Collections.sort()per ordinare liste di oggetti confrontabili.

1.Si prenda in considerazione la seguente classe Java 8+, parametrica su due generics XeY,l ec u ii s t a n z er a p p r e -
sentano sequenzeiterabilidi coppie di coordinate cartesianeX↔Yche rappresentano i punti individuati da una
funzioneX↗Yin un determinato intervallo del dominioX.
1 publicclassFunSeq<XextendsNumber&Comparable<?superX>,YextendsNumber>
2 implementsIterable<Pair<X,Y>>{
3
4 privatefinalXa,b;
5 privatefinalFunction<?superX,?extendsY>f;
6 privatefinalFunction<X,X>inc;
7
8 publicFunSeq(Xa,Xb,Function<?superX,?extendsY>f,Function<X,X>inc){
9 this.a=a;
10 this.b=b;
11 this.f=f;
12 this.inc=inc;
13 }
14
15 /* da finire di implementare */
16 }
Al costruttore vengono passati l’intervallo di dominio[a, b),l af u n z i o n efel af u n z i o n eincper incrementare l’ascissa.
La funzionefèl af u n z i o n ep r i n c i p a l e : p e ro g n ia s c i s s axdi tipoXcompresa nell’intervallo[a, b),l ’ a p p l i c a z i o n e
f(x)ha tipoY.L as e q u e n z ai t e r a b i l ec o n s i s t ei nc o p p i e(x, f(x))di tipoPair<X,Y>che rappresentano l’ascissa
el ’ o r d i n a t ad io g n ip u n t o . E s s e n d oi m p o s s i b i l er a p p r e s e n t a r ef u n z i o n ic o n t i n u e ,l ea s c i s s ep r o c e d o n oi nm a n i e r a
discreta secondo un valore di incremento: la funzione incèn e c e s s a r i ap r o p r i op e rc a l c o l a r el ap r o s s i m aa s c i s s a
nella sequenza.
(a)Si deﬁnisca una classePairparametrica su 2 genericsAeBche rappresenta coppieimmutabili.
(b)Si ﬁnisca di implementare la classeFunSeqopportunamente. Per realizzare il confronto delle ascisse, si noti
che il type parameterXèv i n c o l a t on o ns o l oa de s s e r eu nNumberma anche a implementareComparable<X>.
(c)Si implementi uno snippet di codice che utilizzi la classe FunSeqper rappresentare la parabolax2+2x↘1
nel pianoR↔Rnell’intervallo di dominio[↘2,2)con incremento di ascissa0.1. Ciò signiﬁca che le ascisse
partono da↘2ep r o c e d o n oas t e pd i0.1ﬁno a2(escluso), producendo((2↘0.1)↘(↘2))/0.1 = 39punti. Si
deﬁnisca la parabola con una opportuna lambda.
(d)Si implementi uno snippet simile a quello del punto precedente ma che rappresenti la parabola data nel piano
R↔Z,o v v e r oc o no r d i n a t ed it i p oint.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 importjava.util.Iterator;
2 importjava.util.function.Function;
3
4 publicclassEs1{
5
6 // 1.a
7 publicstaticclassPair<A,B>{
8 publicfinalAfst;
9 publicfinalBsnd;
10
11 publicPair(Aa,Bb){
12 fst=a;
13 snd=b;
14 }
15 }
16
17 publicstaticclassFunSeq<XextendsNumber&Comparable<?superX>,YextendsNumber>
18 implementsIterable<Pair<X,Y>>{
19
20 privatefinalXa,b;

21 privatefinalFunction<?superX,?extendsY>f;
22 privatefinalFunction<X,X>inc;
23
24 publicFunSeq(Xa,Xb,Function<?superX,?extendsY>f,Function<X,X>inc){
25 this.a=a;
26 this.b=b;
27 this.f=f;
28 this.inc=inc;
29 }
30
31 // 1.b
32 @Override
33 publicIterator<Pair<X,Y>>iterator(){
34 returnnewIterator<>(){
35 privateXx=a;
36 @Override
37 publicbooleanhasNext(){
38 returnx.compareTo(b)<=0;
39 }
40
41 @Override
42 publicPair<X,Y>next(){
43 Pair<X,Y>r=newPair<>(x,f.apply(x));
44 x=inc.apply(x);
45 returnr;
46 }
47 };
48 }
49 }
50
51 // 1.c
52 publicstaticvoidtest1(){
53 for(Pair<Double,Double>p:newFunSeq<>(-2.,2.,(x)->x*x-2*x+1,(x)->x+
0.1)){
54 finaldoublex=p.fst,y=p.snd;
55 System.out.printf("f(%g) = %g\n",x,y);
56 }
57 }
58
59 // 1.d
60 publicstaticvoidtest2(){
61 for(Pair<Double,Integer>p:newFunSeq<>(-2.,2.,(x)->(int)(x*x-2*x+1),
(x)->x+0.1)){
62 finaldoublex=p.fst;
63 finalinty=p.snd;
64 System.out.printf("f(%g) = %d\n",x,y);
65 }
66 }
67
68
69 publicstaticvoidmain(String[]args){
70 test1();
71 test2();
72 }
73 }
74
75
```

**Spiegazione rapida del ragionamento**

Le funzioni come oggetti (`Function<A,B>`) permettono composizione e lazy evaluation. Definire le operazioni come interfacce funzionali rende il codice flessibile e componibile.

---

#### 2.1.8 Alberi binari di ricerca

**Testo dell'esercizio**

```text
[Esame 01/06/2023]
1.Vogliamo realizzare in Java 8+ una classeBST,p a r a m e t r i c as uu nt i p og e n e r i c oT,c h er a p p r e s e n t aa l b e r ib i n a r id i
ricerca (Binary Search Tree) i cui nodi sono decorati con valori di tipo T.U na l b e r ob i n a r i od ir i c e r c aèu nn o r m a l e
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
1 publicclassBST<T>implementsIterable<T>{
2 protectedfinalComparator<?superT>cmp;
3 protectedNoderoot;
4
5 protectedclassNode{
6 protectedfinalTdata;
7 protectedNodeleft,right;
8
9 protectedNode(Tdata,Nodeleft,Noderight){
10 this.data=data;
11 this.left=left;
12 this.right=right;
13 }
14 }
15
16 publicBST(Comparator<?superT>cmp){
17 this.cmp=cmp;
18 }
19
20 publicvoidinsert(Tx){
21 root=insertRec(root,x);
22 }
23
24 protectedNodeinsertRec(Noden,Tx){/* da implementare */ }
25
26 protectedvoiddfsInOrder(Noden,Collection<T>out){/* da implementare */ }
27
28 @Override
29 publicIterator<T>iterator(){/* da implementare */ }
30
31 publicTmin(){/* da implementare */ }
32 publicTmax(){/* da implementare */ }
33 }
(a)Si implementi ricorsivamente il metodo insertRec()badando a rispettare la proprietà degli alberi binari
di ricerca enunciata sopra. Per determinare se discendere nel sotto-ramo sinistro oppure in quello destro è
necessario utilizzare ilComparatorpassato in costruzione per confrontare l’argomento xcon il campodata
del nodon.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 importorg.jetbrains.annotations.NotNull;
2 importorg.jetbrains.annotations.Nullable;
3
4 importjava.util.*;
5 importjava.util.function.Function;
6 importjava.util.function.Supplier;
7
8 publicclassEs1{
9
10 // NOTA: la classe è statica solamente perché è nested per praticità
11 publicstaticclassBST<T>implementsIterable<T>{
12 @NotNull
13 protectedfinalComparator<?superT>cmp;
14 @Nullable
15 protectedNoderoot;
16
17 publicBST(@NotNullComparator<?superT>cmp){
18 this.cmp=cmp;
19 }
20
21 publicvoidinsert(@NotNullTx){
22 root=insertRec(root,x);
23 }
24
25 // 1.a
26 @NotNull
27 protectedNodeinsertRec(@NullableNoden,@NotNullTx){
28 if(n==null)
29 returnnewNode(x);
30 intr=cmp.compare(x,n.data);
31 if(r<0)
32 n.left=insertRec(n.left,x);
33 elseif(r>0)
34 n.right=insertRec(n.right,x);
35 returnn;
36 }
37
38 // 1.b.i
39 protectedvoiddfsInOrder(@NullableNoden,@NotNullCollection<T>out){
40 if(n!=null){
41 dfsInOrder(n.left,out);
42 out.add(n.data);
43 dfsInOrder(n.right,out);
44 }
45 }
46
47 // 1.b.ii
48 @NotNull
49 @Override
50 publicIterator<T>iterator(){
51 Collection<T>c=newArrayList<>();
52 dfsInOrder(root,c);
53 returnc.iterator();
54 }
55
56 // 1.c

57 @Nullable
58 publicTmin(){
59 if(root==null)returnnull;
60 Noden=root;
61 while(n.left!=null)n=n.left;
62 returnn.data;
63 }
64
65 @Nullable
66 publicTmax(){
67 if(root==null)returnnull;
68 Noden=root;
69 while(n.right!=null)n=n.right;
70 returnn.data;
71 }
72
73
74 protectedclassNode{
75 @NotNull
76 privatefinalTdata;
77 @Nullable
78 protectedNodeleft,right;
79
80 protectedNode(@NotNullTdata,@NullableNodeleft,@NullableNoderight){
81 this.data=data;
82 this.left=left;
83 this.right=right;
84 }
85
86 protectedNode(@NotNullTdata){
87 this(data,null,null);
88 }
89 }
90
91 @Override
92 @NotNull
93 publicStringtoString(){
94 StringBuildersb=newStringBuilder();
95 for(Tx:this){
96 sb.append(x);
97 sb.append("");
98 }
99 returnsb.toString();
100 }
101 }
102
103 // 1.d
104 publicstaticclassBST2<TextendsComparable<?superT>>extendsBST<T>{
105 publicBST2(){
106 super(Comparable::compareTo);
107 }
108 }
109
110 publicstaticvoidmain(String[]args){
111 BST<Integer>t=newBST2<>();
112 Randomrnd=newRandom();
113 for(inti=0;i<20;++i)

114 t.insert(rnd.nextInt(100));
115 System.out.println(t);
116 System.out.printf("min = %d\nmax = %d\n" ,t.min(),t.max());
117 }
118
119 }
```

**Spiegazione rapida del ragionamento**

L'invariante del BST (sinistra < radice <= destra) e' mantenuta ad ogni inserimento. Questa struttura rende ricerca e ordinamento efficienti (O(log n) nel caso medio).

---

#### 2.1.9 Alberi binari

**Testo dell'esercizio**

```text
[Esame 13/09/2022 – 20/06/2019]
1.Vogliamo realizzare in Java 8+ una classeTreeNode,p a r a m e t r i c as uu nt i p og e n e r i c oT,c h er a p p r e s e n t an o d id i
un albero binario decorati con valori di tipo T.Q u a n d oe n t r a m b iis o t t o - a l b e r is i n i s t r oed e s t r od iu nn o d os o n o
assenti, allora il nodo rappresenta una foglia. Seguono ora le speciﬁche dettagliate.
(a)Gli alberi devono essereiterabiliel ’ i t e r a t o r ed e v ea t t r a v e r s a r el ’ a l b e r oi nD F S(Depth-First Search), fornendo
gli elementi di tipoTinpre-ordine,o v v e r on e l l ’ o r d i n em o s t r a t od aq u e s t oe s e m p i o :
1 1
2 /\
3 25
4 /\ \
5 34 6
6 /\
7 78
(b)Gli alberi devono essere confrontabili tramite il metodoequals().D u e a l b e r i s o n o u g u a l i s e t u t t i i s o t t o - a l b e r i
sono uguali e sono costituiti da elementi uguali.

(c)Si deﬁniscano gli opportuni costruttori ed eventualmente dei metodi statici che fungono da pseudo-costruttori.
L’obiettivo è fare in modo che gli alberi siano facili da costruire innestando ricorsivamente le chiamate. Non
si permetta l’istanziazione di alberi vuoti o non inizializzati da popolare successivamente con dei setter.
(d)Si implementino uno o più snippet di test che mettono alla prova tutte le caratteristiche richieste.
(e)Si implementi unpretty printertramite un override del metodotoString().
Si implementi la classeTreeNodesecondo le speciﬁche date, rappresentando la struttura dati nella maniera che si
ritiene più conveniente e fornendo tutti i metodi necessari. È importante il riuso di codice, l’information hiding ed
una implementazione che escluda il più possibile stati di invalidità grazie ad un saggio uso dei tipi.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 // Questo sorgente contiene le soluzioni dell 'esame scritto di PO2 del 13/9/2022 per ciò che
riguarda il quesito 1,
2 // ovvero l'esercizio in Java.
3 // Il quesito 2 riguardante C++ è in una Solution per Visual Studio a parte.
4
5 importorg.jetbrains.annotations.NotNull;
6 importorg.jetbrains.annotations.Nullable;
7
8 importjava.util.ArrayList;
9 importjava.util.Collection;
10 importjava.util.Iterator;
11 importjava.util.concurrent.BlockingQueue;
12
13 publicclassEs1{
14
15 // 1.d
16
17 publicstaticvoidmain(String[]args){
18 TreeNode<Integer>t1=
19 TreeNode.lr(1,
20 TreeNode.lr(2,
21 TreeNode.v(3),
22 TreeNode.v(4)),
23 TreeNode.r(5,
24 TreeNode.lr(6,
25 TreeNode.v(7),
26 TreeNode.v(8))));
27
28 TreeNode<Integer>t2=
29 TreeNode.lr(1,
30 TreeNode.r(5,
31 TreeNode.lr(6,
32 TreeNode.v(7),
33 TreeNode.v(8))),
34 TreeNode.lr(2,
35 TreeNode.v(3),
36 TreeNode.v(4)));
37
38
39 // test pretty printer
40 System.out.println("pretty printer:");
41 System.out.println("t1: "+t1);
42 System.out.println("t2: "+t2);
43
44 // test equality
45 System.out.print("equality: ");
46 System.out.println(t1.equals(t2)+", "+(t1.left!=null?t1.left.equals(t2.right):
""));
47

48 // test iterator
49 System.out.print("iterator: ");
50 for(Integern:t1){
51 System.out.printf("%d ",n);
52 }
53 System.out.println();
54
55 }
56
57 publicstaticclassTreeNode<T>implementsIterable<T>{
58
59 @NotNull
60 privatefinalTdata;
61 @Nullable
62 privatefinalTreeNode<T>left,right;
63 @Nullable
64 privateTreeNode<T>parent=null;
65
66 // 1.c
67
68 publicTreeNode(@NotNullTdata,@NullableTreeNode<T>left,@NullableTreeNode<T>
right){
69 this.data=data;
70 this.left=left;
71 this.right=right;
72 if(left!=null)left.parent=this;
73 if(right!=null)right.parent=this;
74 }
75
76 // i seguenti pseudo-costruttori aiutano a costruire alberi in modo più succinto e
controllato rispetto
77 // all'innestamento dei costruttori
78
79 // solo ramo sinistro
80 publicstatic<T>TreeNode<T>l(@NotNullTdata,@NotNullTreeNode<T>left){
81 returnnewTreeNode<>(data,left,null);
82 }
83
84 // solo ramo destro
85 publicstatic<T>TreeNode<T>r(@NotNullTdata,@NotNullTreeNode<T>right){
86 returnnewTreeNode<>(data,null,right);
87 }
88
89 // ramo sinistro e destro
90 publicstatic<T>TreeNode<T>lr(@NotNullTdata,@NotNullTreeNode<T>left,@NotNull
TreeNode<T>right){
91 returnnewTreeNode<>(data,left,right);
92 }
93
94 // foglia
95 publicstatic<T>TreeNode<T>v(@NotNullTdata){
96 returnnewTreeNode<>(data,null,null);
97 }
98
99 // 1.b
100
101 @Override

102 publicbooleanequals(@NullableObjecto){
103 if(oinstanceofTreeNode){
104 BlockingQueue<Integer>b;
105
106 TreeNode<T>t=(TreeNode<T>)o;
107 returnareEqual(data,t.data)&&areEqual(left,t.left)&&areEqual(right,
t.right);
108 }
109 returnfalse;
110 }
111
112 privatestaticbooleanareEqual(@NullableObjecta,@NullableObjectb){
113 returna==b||(a!=null&&a.equals(b));
114 //return Objects.equals(a, b); // alternativamente si può usare questo metodo
del JDK
115 }
116
117 // 1.e
118
119 @Override
120 @NotNull
121 publicStringtoString(){
122 returnString.format("%s%s%s",data,left!=null?String.format("(%s)",left):
"",right!=null?
123 String.format("[%s]",right):"");
124 }
125
126 // 1.a
127
128 @Override
129 publicIterator<T>iterator(){
130 returniterator_easy();// stub ad una delle due implementazione; cambiare lo stub
per testare l'altra
131 }
132
133
134 // questo è l'implementazione facile, suggerita pubblicamente dal docente in classe
duranto l'appello del 13/9/22
135 privateIterator<T>iterator_easy(){
136 Collection<T>r=newArrayList<>();
137 dfs(r);
138 returnr.iterator();
139 }
140
141 privatevoiddfs(Collection<T>c){
142 c.add(data);
143 if(left!=null)left.dfs(c);
144 if(right!=null)right.dfs(c);
145 }
146
147 // questo è l'implementazione ottimizzata, che attraversa l 'albero senza liste d'appoggio
148 privateIterator<T>iterator_opt(){
149 returnnewIterator<>(){
150 @Nullable
151 privateTreeNode<T>current=TreeNode.this;
152
153 @Override

154 publicbooleanhasNext(){
155 returncurrent!=null;
156 }
157
158 @Nullable
159 privatestatic<T>TreeNode<T>getNextNode(@NotNullTreeNode<T>n){
160 if(n.left!=null)
161 returnn.left;
162 elseif(n.right!=null)
163 returnn.right;
164 else{
165 while(n.parent!=null){
166 finalTreeNode<T>last=n;
167 n=n.parent;
168 if(n.right!=null&&n.right!=last)
169 returnn.right;
170 }
171 returnnull;
172 }
173 }
174
175 @Override
176 @NotNull
177 publicTnext(){
178 Tr=current.data;
179 current=getNextNode(current);
180 returnr;
181 }
182 };
183 }
184
185 }
186
187 }
```

**Spiegazione rapida del ragionamento**

L'iteratore incapsula la traversata in-order senza esporre la struttura interna. La separazione tra struttura (nodi) e comportamento (iterazione) rispetta il principio Open/Closed.

---

#### 2.1.10 SkippableArrayList

**Testo dell'esercizio**

```text
[Esame01/07/2022– 08/09/2018]S ii m p l e m e n t ii nJ a v a8 +u n as o t t o c l a s s eg e n e r i c ad ijava.util.ArrayLis->
tdi nomeSkippableArrayListche estende la superclasse con un iteratore in grado di discriminare gli elementi
secondo un predicato booleano. Gli elementi che soddisfano il predicato vengono processati da una certa funzione di
trasformazione5;g l ia l t r iv e n g o n op a s s a t ia du n as e c o n d ac a l l b a c k( n o nu n af u n z i o n ed it r a s f o r m a z i o n e ) .
1.Realizziamo in Java 8+ una sottoclasse di java.util.ArrayListdi nomeSkippableArrayListparametrica su
un tipoTche estende la superclasse con un iteratore in grado di discriminare gli elementi secondo un predicato e
di processarli tramite due funzioni distinte a seconda dell’esito dell’applicazione del predicato all’elemento.
(a)Si deﬁnisca unainterfaccia funzionaledi nomePredicatespecializzando l’interfaccia genericajava.util.->
Functiondel JDK in modo che il dominio sia un generic Ted il codominio siaBoolean.
(b)Si deﬁnisca una interfacciaEitherparametrica su un tipo genericoTe che deﬁnisce due metodi. Il primo
metodo, di nomeonSuccess,p r e n d eu nTer i t o r n au nTev i e n ec h i a m a t od a l l ’ i t e r a t o r eq u a n d oi lp r e d i c a t o
ha successo. Il secondo metodo, di nomeonFailure,v i e n ei n v o c a t oi n v e c eq u a n d oi lp r e d i c a t of a l l i s c e ,p r e n d e
un argomento di tipoTen o np r o d u c ea l c u nr i s u l t a t o ,t u t t a v i ap u òl a n c i a r eu n ae c c e z i o n ed it i p oException:
(c)Si deﬁnisca la sottoclasseSkippableArrayListparametrica su un tipoEes ii m p l e m e n t iu nm e t o d op u b -
blico avente ﬁrmaIterator<E>iterator(Predicate<E>p,Either<E>f)che crea un iteratore con le
caratteristiche accennate sopra. Più precisamente:
•l’iteratore parte sempre dall’inizio della collezione ed arriva alla ﬁne, andando avanti di un elemento alla
volta normalmente;
•ad ogni passo l’iteratore applica il predicatopall’elemento di tipoTcorrente, che chiameremox:s ep(x)
computatrueallora viene invocato il metodoonSuccessdifep a s s a t ol ’ e l e m e n t oxcome argomento;
altrimenti viene invocato il metodoonFailureep a s s a t oxcome argomento a quest’ultimo;
•l’invocazione dionFailuredeve essere racchiusa dentro un blocco che assicura iltrappingdelle eccezioni:
in altre parole, una eccezione proveniente dall’invocazione dionFailurenon deve interrompere l’iteratore;
•quando viene invocatoonSuccess,i ls u or i s u l t a t ov i e n er e s t i t u i t oc o m ee l e m e n t oc o r r e n t ed a l l ’ i t e r a t o r e ;
•quando viene invocatoonFailure,l ’ i t e r a t o r er i t o r n al ’ e l e m e n t oo r i g i n a l ec h eh af a t t of a l l i r ei lp r e d i c a t o .
(d)Si scriva un esempio di codicemainche:
•costruisce unaArrayListdi interi vuota di nomedst;
•costruisce unaSkippableArrayListdi interi di nomesrcel ap o p o l ac o nn u m e r ic a s u a l ic o m p r e s it r a0
e1 0 ,i n c l u s ig l ie s t r e m i6;
•invocandosolamente una voltail metodoiterator(Predicate<E>,Either<E>)disrccon gli argo-
menti opportuni, somma 1 a tutti gli elementi disrcmaggiori di 5 e appende in coda adstquelli minori
ou g u a l ia5 .
•Il metodoiterator()con due parametri richiesto dal punto (c)èu no v e r r i d eou no v e r l o a d ?
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1
2 // Questo sorgente contiene le soluzioni dell 'esame scritto di PO2 del 1/7/2022 per ciò che
riguarda i quesiti 1-2, ovvero le domande che coinvolgono Java.
3 // Il quesito 3 riguardante C++ è in un progetto Visual Studio a parte, non qui.
4 // Il codice qui esposto è Java 8+.
5
6 importjava.util.ArrayList;
7 importjava.util.Iterator;
8 importjava.util.function.Function;
9
10 publicclassEs1{
11
12 // 1.a
13 publicinterfacePredicate<T>extendsFunction<T,Boolean>{}
14
15 // 1.b
16 publicinterfaceEither<T>{
17 TonSuccess(Tx);
18 voidonFailure(Tx)throwsException;
19 }
20

21 // 1.c
22 publicstaticclassSkippableArrayList<E>extendsArrayList<E>{
23 publicIterator<E>iterator(Predicate<E>p,Either<E>f){
24 finalIterator<E>it=super.iterator(); // può anche essere un campo privato
della anonymous class
25 returnnewIterator<E>(){
26 @Override
27 publicbooleanhasNext(){
28 returnit.hasNext();
29 }
30
31 @Override
32 publicEnext(){
33 Ex=it.next();
34 if(p.apply(x))
35 returnf.onSuccess(x);
36 else{
37 try{
38 f.onFailure(x);
39 }
40 catch(Exceptione){
41 e.printStackTrace(); // si può anche non fare niente dentro il
catch, è indifferente
42 }
43 returnx;
44 }
45 }
46 };
47 }
48 }
49
50 }
```

**Spiegazione rapida del ragionamento**

Estendere `ArrayList` con un metodo `skip` aggiunge solo la logica specifica senza riscrivere il contratto esistente, riusando tutta l'implementazione standard.

---

#### 2.1.11 Figure geometriche

**Testo dell'esercizio**

```text
[Esame03/06/2022– 31/01/2020 – 24/05/2018]D e ﬁ n i a m oi nJ a v a8 +u ns i s t e m ad ic l a s s ie di n t e r f a c c ec h e
rappresentano ﬁgure geometriche piane e solide. Le ﬁgure geometriche rappresentate non sono posizionate nel piano
cartesiano o nello spazio, sono pertanto prive di coordinate. Per semplicità esse contengono solamente le informazioni
sulla lunghezza dei lati o delle facce di cui sono costituite.
5Una funzione di trasformazione è una funzione in cui dominio è uguale al codominio, per esempio una funzione f:ωèu n af u n z i o n e
di trasformazione sull’insiemeω.
6Si utilizzi la classeRandomdel JDK per generare numeri casuali.

1.Prima di cominciare, realizziamo una piccola libreria interna che consiste in alcuni metodi statici generici altamente
riusabili. Sia data la funzione di ordine superiore foldimplementata tramite un metodo statico pubblico:
1 publicstatic<T,State>Statefold(Iterable<T>i,finalStatest0,BiFunction<State,T,
State>f){
2 Statest=st0;
3 for(finalTe:i)
4 st=f.apply(st,e);
5 returnst;
6 }
(a)Si implementi tramiteuna solainvocazione difold()la funzione di ordine superioresumByavente la seguente
ﬁrma:
1 publicstatic<T>doublesumBy(Iterable<T>i,Function<T,Double>f);
Essa calcola la sommatoria di tutti gli elementi di i trasformandoli in doubletramitef.L a u t i l i z z e r e m o a d
esempio per calcolare il perimetro di un poligono sommando la lunghezza di tutti i suoi lati, oppure l’area
laterale totale di un solido sommando l’area di tutte le superﬁci piane di cui è costituito.
(b)Av r e m o b i s o g n o d i o r d i n a r e l e n o s t r e ﬁ g u r e g e o m e t r i ch e s u l l a b a s e d i d i ve r s i c r i t e r i , a d e s e m p i o l ’ a r e a o i l
volume. Si implementi il metodo staticocompareBy()avente la seguente ﬁrma:
1 publicstatic<T>intcompareBy(Ts1,Ts2,Function<T,Double>f);
Esso confrontas1eds2conventerdoli prima indoubletramitef,r i d u c e n d op e r t a n t oi lc o n f r o n t oa lc o n f r o n t o
tra due numeridouble.
(c)Quale forma di polimorﬁsmo forniscono i generics deﬁniti sulla ﬁrma di un metodo come ad esempio quelli
del metodofold()di cui sopra?
->Polimorﬁsmo subtype ->Polimorﬁsmo parametrico ->Polimorﬁsmo parametrico ﬁrst-class
->Polimorﬁsmo ad-hoc
2.Deﬁniamo ora i tipi essenziali per rappresentare ﬁgure geometriche piane e solide.
(a)La classeEdgerappresenta grandezze 1-dimensionali come lati di poligoni, spigoli di poliedri, segmenti e
circonferenze.
1 publicclassEdgeimplementsComparable<Edge>{
2 privatefinaldoublelen;
3 publicEdge(doublelen){this.len=len;}
4 publicdoublelength(){returnlen;}
5 @Override
6 publicintcompareTo(Edges){/* DA IMPLEMENTARE */ }
7 }
8 }
Si implementi il metodocompareTo()tramiteuna solainvocazione dellacompareBy()deﬁnita sopra.
(b)L’interfacciaSurfacerappresenta ﬁgure piane qualunque:
1 publicinterfaceSurfaceextendsComparable<Surface>{
2 doublearea();
3 doubleperimiter();
4 @Override
5 defaultintcompareTo(Surfaces){/* DA IMPLEMENTARE */ }
6 }
Si dia una implementazione di default del metodo compareTo()tramiteuna solainvocazione dellacompa->
reBy()deﬁnita sopra che esegua il confronto tra le aree.
(c)Il sotto-tipoPolygonrappresenta poligoni: l’interfaccia specializzaSurfacedando una implementazione di
default al metodoperimeter()ep e r m e t t ea n c h el ’ i t e r a z i o n ed e il a t id ic u ii lp o l i g o n os t e s s oèc o s t i t u i t o .
1 publicinterfacePolygonextendsSurface,Iterable<Edge>{
2 @Override
3 defaultdoubleperimiter(){/* DA IMPLEMENTARE */ }
4 }

Si dia una implementazione di default del metodoperimeter()tramiteuna solainvocazione dellasumBy()
deﬁnita sopra.
(d)L’interfacciaSolidrappresenta solidi qualunque:
1 publicinterfaceSolidextendsComparable<Solid>{
2 doubleouterArea();// area laterale totale
3 doublevolume();
4 @Override
5 defaultintcompareTo(Solids){/* DA IMPLEMENTARE */ }
6 }
Si implementi il metodocompareTo()tramiteuna solainvocazione dellacompareBy()deﬁnita sopra che
esegua il confronto tra i volumi.
(e)Polyhedronèu ns o t t o t i p od iSolider a p p r e s e n t ap o l i e d r i . L ’ i n t e r f a c c i aèp a r a m e t r i c ar i s p e t t oa ls o t t o t i p od i
Polygonche descrive le facce di cui il poliedro è costituito. Ad esempio, le facce di un cubo sono quadrati: una
classeCubeimplementerebbe pertanto l’interfacciaPolyhedronaventeSquarecometype argument,Square
↑Polygon.
UnPolyhedronpermette l’iterazione delle facce poligonali di cui esso è costituito; fornisce inoltre una imple-
mentazione di default del metodoouterArea()che calcola semplicemente la sommatoria delle aree delle sue
facce.
1 publicinterfacePolyhedron<PextendsPolygon>extendsSolid,Iterable<P>{
2 @Override
3 defaultdoubleouterArea(){/* DA IMPLEMENTARE */ }
4 }
Si implementi il metodoouterArea()tramiteuna solainvocazione dellasumBy()deﬁnita sopra.
3.Si proceda ora alla deﬁnizione di una gerarchia di classi che rappresentano ﬁgure geometriche speciﬁche implemen-
tando le interfacce ﬁn qui introdotte.
(a)Si implementi una classe che rappresentasfereimmutabili avente nomeSphereec h ei m p l e m e n t al ’ i n t e r f a c c i a
Solid. Il costruttore diSpheredeve prendere come parametro solamente undouble:i l r a g g i o d e l l a s f e r a .
(b)Si implementi una classe che rappresentacilindriimmutabili avente nomeCilinderec h ei m p l e m e n t al ’ i n -
terfacciaSolid. Il costruttore diCilinderdeve prendere come parametri duedouble:i l r a g g i o d e l l a b a s e e
l’altezza del cilindro.
(c)Si implementi una classe che rappresenta rettangoliimmutabili avente nomeRectangleec h ei m p l e m e n t a
l’interfacciaPolygon. Il costruttore diRectangledeve prendere come parametri duedouble:b a s e e a l t e z z a .
(d)Si implementi una classe che rappresenta quadrati immutabili avente nomeSquareec h ee s t e n d eRectangle.
Il costruttore diSquaredeve prendere un solo parametro di tipodouble:i l l a t o .
4.Si prenda in considerazione la seguente classe che rappresenta parallelepipedi immutabili. I parallelepipedi sono
poliedri aventi facce rettangolari.
1 publicclassParallelepipedimplementsPolyhedron<Rectangle>{
2 protectedfinaldoublewidth,height,depth;
3 publicParallelepiped(doublewidth,doubleheight,doubledepth){
4 this.width=width;
5 this.height=height;
6 this.depth=depth;
7 }
8 @Override
9 publicdoublevolume(){/* DA IMPLEMENTARE */ }
10 @Override
11 publicIterator<Rectangle>iterator(){
12 Rectangler1=newRectangle(width,height),
13 r2=newRectangle(width,depth),
14 r3=newRectangle(height,depth);
15 returnList.of(r1,r2,r3,r1,r2,r3).iterator();
16 }
17 }

(a)Si implementi il metodovolume().
(b)Si deﬁnisca la classeCubecome sottoclasse diParallelepiped. Il costruttore diCubedeve prendere solamente
un parametro di tipodouble:l a l u n g h e z z a d e l l o s p i g o l o .
(c)Èp o s s i b i l ec o - v a r i a r ei lt i p od ir i t o r n od e lm e t o d oiterator()diCubein modo che ritorni unIterator<S->
quare>?S i m o t i v i l a r i s p o s t a .
(d)Assumendo l’implementazione diCubedi cui al punto (b), si prenda in considerazione il seguente snippet:
1 intfacet_cnt=1;
2 for(Squaresq:newCube(10.)){
3 intside_cnt=1;
4 for(Edgee:sq){
5 System.out.printf("side #%d/%d = %f\n",side_cnt++,facet_cnt,e.length());
6 }
7 ++facet_cnt;
8 }
Tale co dice compila? Si motivi la risp osta.
5.Si prenda in considerazione il seguente snippet di codice che crea una lista eterogena di poliedri le cui facce sono
almeno rettangoli.
1 Cubec1=newCube(1.),c2=newCube(2.));
2 Parallelepipedp1=newParallelepiped(1.,2.,3.),p2=newParallelepiped(2.,3.,4.);
3 List<Polyhedron<?extendsRectangle>>polys=List.of(c1,c2,p1,p2);
Seguono ora alcune chiamate ai metodiCollections.sort()del JDK. Gli oggetti nella listapolyssono sempre
gli stessi 4 (i cubic1ec2ed i parallelepipedip1ep2), ma ogni invocazione disort()li ordina in maniera diversa
as e c o n d ad e lc r i t e r i od io r d i n a m e n t o . P e rc i a s c u n ai n v o c a z i o n es ii n d i c h iq u a l eo g g e t t ov i e n em e s s oi nt e s t aa l l a
lista, ovvero quale oggetto computerebbe l’espressionepolys.get(0),o p p u r es en o nc o m p i l a .
1 Collections.sort(polys)
->non compila ->c1 ->c2 ->p1 ->p2
1 Collections.sort(polys,(x,y)->compareBy(x,y,Polyhedron::outerArea))
->non compila ->c1 ->c2 ->p1 ->p2
1 Collections.sort(polys,(x,y)->compareBy(x,y,(p)->p.outerArea()))
->non compila ->c1 ->c2 ->p1 ->p2
1 Collections.sort(polys,(x,y)->compareBy(x,y,(r)->r.perimiter()))
->non compila ->c1 ->c2 ->p1 ->p2
1 Collections.sort(polys,(x,y)->compareBy(x,y,newFunction<>(){
2 publicDoubleapply(Polyhedron<?extendsRectangle>r){
3 returnr.volume();
4 }
5 }))
->non compila ->c1 ->c2 ->p1 ->p2
1 Collections.sort(polys,(x,y)->Double.compare(sumBy(x,Square::perimiter),
2 sumBy(y,Rectangle::perimiter)))
->non compila ->c1 ->c2 ->p1 ->p2
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 importjava.util.*;
2 importjava.util.function.BiFunction;
3 importjava.util.function.Function;
4
5 // Questo sorgente contiene le soluzioni dell 'esame scritto di PO2 del 3/6/2022 per ciò che
riguarda i quesiti 1-5, ovvero le domande che coinvolgono Java.
6 // Il quesito 6 riguardante C++ è in una Solution per Visual Studio a parte, non qui.
7
8 publicclassAppello_3_6_22{
9
10 publicstatic<T,State>Statefold(Iterable<T>i,finalStatest0,BiFunction<State,T,
State>f){
11 Statest=st0;
12 for(finalTe:i){
13 st=f.apply(st,e);
14 }
15 returnst;
16 }
17
18 // 1.a
19 publicstatic<T>doublesumBy(Iterable<T>i,Function<T,Double>f){
20 returnfold(i,0.,(r,x)->r+f.apply(x));
21 }

22
23 // 1.b
24 publicstatic<T>intcompareBy(Ts1,Ts2,Function<T,Double>f){
25 returnDouble.compare(f.apply(s1),f.apply(s2));
26 }
27
28 publicstaticclassEdgeimplementsComparable<Edge>{
29 privatefinaldoublelen;
30
31 publicEdge(doublelen){
32 this.len=len;
33 }
34
35 publicdoublelength(){
36 returnlen;
37 }
38
39 // 2.a
40 @Override
41 publicintcompareTo(Edges){
42 returncompareBy(this,s,Edge::length);
43 }
44 }
45
46 publicinterfaceSurfaceextendsComparable<Surface>{
47 doublearea();
48
49 doubleperimiter();
50
51 // 2.b
52 @Override
53 defaultintcompareTo(Surfaces){
54 returncompareBy(this,s,Surface::area);
55 }
56 }
57
58 publicinterfacePolygonextendsSurface,Iterable<Edge>{
59 // 2.c
60 @Override
61 defaultdoubleperimiter(){
62 returnsumBy(this,Edge::length);
63 }
64 }
65
66 publicinterfaceSolidextendsComparable<Solid>{
67 doubleouterArea();
68
69 doublevolume();
70
71 // 2.d
72 defaultintcompareTo(Solids){
73 returncompareBy(this,s,Solid::volume);
74 }
75 }
76
77 publicinterfacePolyhedron<PextendsPolygon>extendsSolid,Iterable<P>{
78 // 2.e

79 @Override
80 defaultdoubleouterArea(){
81 returnsumBy(this,P::area);
82 }
83 }
84
85 // 3.a
86 publicstaticclassSphereimplementsSolid{
87 privatefinaldoubleradius;
88
89 publicSphere(doubleradius){
90 this.radius=radius;
91 }
92
93 @Override
94 publicdoubleouterArea(){
95 return4.*Math.PI*radius*radius;
96 }
97
98 @Override
99 publicdoublevolume(){
100 return4./3.*Math.PI*radius*radius*radius;
101 }
102 }
103
104 // 3.b
105 publicstaticclassCilinderimplementsSolid{
106 privatefinaldoubleradius,height;
107
108 publicCilinder(doubleradius,doubleheight){
109 this.radius=radius;
110 this.height=height;
111 }
112
113 @Override
114 publicdoubleouterArea(){
115 doubleb=Math.PI*radius*radius,l=2.*Math.PI*radius*height; // hanno
capito solo laterale
116 return2.*b+l;
117 }
118
119 @Override
120 publicdoublevolume(){
121 returnMath.PI*radius*radius*height;
122 }
123 }
124
125 // 3.c
126 publicstaticclassRectangleimplementsPolygon{
127 privatefinaldoublewidth,height;
128
129 publicRectangle(doublewidth,doubleheight){
130 this.width=width;
131 this.height=height;
132 }
133
134 @Override

135 publicdoublearea(){
136 returnwidth*height;
137 }
138
139 @Override
140 publicIterator<Edge>iterator(){
141 Edgew=newEdge(width),h=newEdge(height);
142 returnList.of(w,h,w,h).iterator();
143 }
144 }
145
146 // 3.d
147 publicstaticclassSquareextendsRectangle{
148 publicSquare(doubleside){
149 super(side,side);
150 }
151 }
152
153 publicstaticclassParallelepipedimplementsPolyhedron<Rectangle>{
154 protecteddoublewidth,height,depth;
155
156 publicParallelepiped(doublewidth,doubleheight,doubledepth){
157 this.width=width;
158 this.height=height;
159 this.depth=depth;
160 }
161
162 // 4.a
163 @Override
164 publicdoublevolume(){
165 returnwidth*height*depth;
166 }
167
168 @Override
169 publicIterator<Rectangle>iterator(){
170 Rectangler1=newRectangle(width,height),r2=newRectangle(width,depth),r3=
newRectangle(height,depth);
171 returnList.of(r1,r2,r3,r1,r2,r3).iterator();
172 }
173 }
174
175 // 4.b
176 publicstaticclassCubeextendsParallelepiped{
177 publicCube(doubleside){
178 super(side,side,side);
179 }
180 }
181
182
183 publicstaticvoidmain(String[]args){
184 // 4.d
185 {
186 intfacet_cnt=1;
187 // questo foreach non compila perché Cube è sottotipo di Iterable<Rectangle>, non di
Iterable<Square>
188 // si badi che NON è possibile co-variare il tipo di ritorno del metodo iterator() di
Cube in modo che si specializzi in Iterator<Square>

189 // perché è sound co-variare il tipo più esterno di un tipo parametrico, ma non il
type argument
190 // for (Square sq : new Cube(10.)) {
191 for(Rectanglesq:newCube(10.)){// così invece compilerebbe
192 intside_cnt=1;
193 for(Edgee:sq){
194 System.out.printf("side #%d/%d = %f\n",side_cnt++,facet_cnt,e.length());
195 }
196 ++facet_cnt;
197 }
198 }
199
200 // 5
201 {
202 Cubec1=newCube(1.),c2=newCube(2.);
203 Parallelepipedp1=newParallelepiped(1.,2.,3.),p2=newParallelepiped(2.,3.,
4.);
204 List<Polyhedron<?extendsRectangle>>polys=newArrayList<>(List.of(c1,c2,p1,
p2));
205
206 // per testare rapidamente i risultati di queste sort, si usi debugger
207 Collections.sort(polys);
// c1
208 Collections.sort(polys,(x,y)->compareBy(x,y,Polyhedron::outerArea));
// c1
209 Collections.sort(polys,(x,y)->compareBy(x,y,(p)->p.outerArea()));
// c1
210 // Collections.sort(polys, (x, y) -> compareBy(x, y, (r) -> r.perimiter()));
// non compila
211 Collections.sort(polys,(x,y)->compareBy(x,y,newFunction<>(){
// c1
212 @Override
213 publicDoubleapply(Polyhedron<?extendsRectangle>r){
214 returnr.volume();
215 }
216 }));
217 // Collections.sort(polys, (x, y) -> Double.compare(sumBy(x, Square::perimiter),
// non compila
218 // sumBy(y, Rectangle::perimiter)));
219 }
220 }
221 }
```

**Spiegazione rapida del ragionamento**

La gerarchia di figure con metodi `area()` e `perimeter()` polimorfici evita if/else sul tipo. Il metodo `max()` generico con `Comparator<Polygon>` mostra l'uso corretto di PECS (`? super Polygon`).

---

#### 2.1.12 Artisti

**Testo dell'esercizio**

```text
[Esame 20/06/2019]S ic o n s i d e r il as e g u e n t ei n t e r f a c c i ap a r a m e t r i c ai nl i n g u a g g i oJ a v a :
1 publicinterfaceEquatable<T>{
2 booleanequalsTo(Tx);
3 }
Essa non sostituisce il meccanismo basato sul metodo equals(Object)della classeObject,t u t t a v i ap e r m e t t ed i
implementare il confronto di uguaglianza in maniera più sicura delegando parte della logica ad un metodo fortemente
tipato.
Si consideri ora la classe parametricaPerson,c h es u p p o r t ai lc o n f r o n t od iu g u a g l i a n z ac o no g g e t t id it i p oP:
1 publicclassPerson<PextendsPerson<P>>implementsEquatable<P>{
2 publicfinalStringname;
3 publicfinalintage;
4
5 publicPerson(Stringname,intage){
6 this.name=name;
7 this.age=age;
8 }
9
10 @Override
11 publicbooleanequals(Objecto){/* da implementare */ }
12
13 @Override
14 publicbooleanequalsTo(Pother){/* da implementare */ }
15
16 @Override
17 publicStringtoString(){returnname;}
18 }
1.Si implementi il metodoequals(Object)della classePersonin modo che, datip1ep2di tipoPerson,l es e g u e n t i
invarianti siano rispettate:
•p1.equals(p1)==true
•p1.equals(null)==false
•p1.equals(e)==falsese l’espressioneeha tipo diverso dal tipo dip17
•p1.equals(p2)==p1.equalsTo(p2)
2.Si implementi il metodoequalsTo(P)della classePersonin modo che due oggetti siano uguali quando hanno il
medesimo nome e la medesima età.
Si consideri ora il seguente codice:
1 publicclassArtistextendsPerson<Artist>{
2 publicfinalHairhair;
3
4 publicArtist(Stringname,intage,Hairhair){
5 super(name,age);
6 this.hair=hair;
7 }
8
9 @Override
10 publicbooleanequalsTo(Artistother){/* da implementare */ }
11 }
12
13 publicclassHairimplementsEquatable<Hair>{
7Ricordiamo che il metodo della classeObjectavente ﬁrmaClass<?extendsObject>getClass()consente di estrarre a runtime il tipo
rawdi un oggetto.

14 publicfinalintlength;
15 publicfinalSet<Color>colors;
16
17 publicHair(intlength,Set<Color>colors){
18 this.colors=colors;
19 this.length=length;
20 }
21
22 @Override
23 publicbooleanequals(Objecto){/* da implementare */ }
24
25 @Override
26 publicbooleanequalsTo(Hairx){/* da implementare */ }
27 }
28
29 publicenumColor{
30 BROWN,DARK,BLONDE,RED,GRAY;
31 }
1.Si implementino i metodiequals(Object)eequalsTo(Hair)della classeHairin modo che due oggetti siano uguali
quando hanno la medesima lunghezza ed i medesimi colori (indipendentemente dal loro ordine). Si rispettino le
stesse invarianti del punto (a) per l’implementazione del metodo equals(Object)es id e l e g h i ,c o m ep e rl ac l a s s e
Person,l ap a r t ef o r t e m e n t et i p a t ad e lc o n f r o n t oa lm e t o d oequalsTo(Hair).
2.Il metodoequalsTo(Artist)èd a v v e r ou noverridedel metodoequalsTo(P)ereditato dalla superclasse?
->No, perché latype erasureelimina il genericPche viene sostuito col suo constraintPersonnella super-
classe, la quale introduce di fatto un metodo equalsTo(Person),d ic u iequalsTo(Artist)non è un
override ma un overload.
->Sì, perché il metodoequalsTo()viene gestito in modo particolare dal compilatore Java, permettendo
override anche con ﬁrme di!erenti.
->No, perché la ﬁrma è diversa e quindi è un overload, non un override.
->Sì, perché il generic Pviene sostituito col tipo Artistnello scope della sottoclasse, pertanto viene
ereditato un metodoequalsTo(Artist),r e n d e n d oq u e s t ou nv e r oo v e r r i d e .
3.Si implementi il metodoequalsTo(Artist)della classeArtistin modo che due oggetti siano uguali quando hanno
im e d e s i m in o m e ,e t àec a p e l l i . S ib a d iar i u s a r eo p p o r t u n a m e n t el ’ i m p l e m e n t a z i o n ee r e d i t a t ad a l l as u p e r c l a s s e .
Si considerino ora i seguenti binding in Java:
1 Personalice=newPerson("Alice",23),
2 david=newArtist("Bowie",69,newHair(75,Set.of(Color.RED,Color.BROWN,
Color.GRAY)));
3 Artistmorgan=newArtist("Morgan",47,newHair(20,Set.of(Color.GRAY,Color.DARK))),
4 madonna=newArtist("Madonna",60,newHair(50,Set.of(Color.BLONDE)));
4.Si prenda in considerazione il seguente metodo della classe java.util.Collectionsdel JDK 7+ per computare
l’elemento massimo in una collection secondo un criterio di ordinamento dato:
1 static<T>Tmax(Collection<?extendsT>c,Comparator<?superT>cmp)
Si considerino ora i seguenti binding in Java:
1 List<Artist>artists=Arrays.asList((Artist)david,morgan,madonna);
2 List<Person>persons=Arrays.asList(alice,david,morgan,madonna);
(a)Si scriva uno statement di invocazione del metodo max()che computi l’oggetto della listaartistsavente il
più alto prodotto tra lunghezza dei capelli e numero di colori.
(b)Si scriva uno statement di invocazione del metodo max()che computi l’oggetto della listapersonsavente il
nome che viene per primo in ordine lessicograﬁco.
(c)Il seguente statement sarebbe accettato dal compilatore Java 7+?

1 Artistc=Collections.max(artists,newComparator<Person>(){
2 publicintcompare(Persona,Personb){
3 returna.age-b.age;
4 }
5 });
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 importjava.util.*;
2
3 publicclassEs1{
4
5 publicinterfaceEquatable<T>{
6 booleanequalsTo(Tx);
7 }
8
9 publicstaticclassPerson<PextendsPerson<P>>implementsEquatable<P>{
10
11 publicfinalStringname;
12 publicfinalintage;
13

14 publicPerson(Stringname,intage){
15 this.name=name;
16 this.age=age;
17 }
18
19 // 1.a
20 @Override
21 publicbooleanequals(Objecto){
22 // self check
23 if(this==o)
24 returntrue;
25 // null check
26 if(o==null)
27 returnfalse;
28 // type check
29 if(getClass()!=o.getClass())
30 returnfalse;
31 // field comparison
32 returnequalsTo((P)o); // deleghiamo al metodo equalsTo(P) il confronto tipato
tra i campi
33 }
34
35 // 1.b
36 @Override
37 publicbooleanequalsTo(Pother){
38 returnname.equals(other.name)&&age==other.age;
39 }
40
41 @Override
42 publicStringtoString(){
43 returnname;
44 }
45
46 }
47
48 publicstaticclassArtistextendsPerson<Artist>{
49
50 publicfinalHairhair;
51
52 publicArtist(Stringname,intage,Hairhair){
53 super(name,age);
54 this.hair=hair;
55 }
56
57 // 1.d RISPOSTA: 4
58 // 1.e
59 @Override
60 publicbooleanequalsTo(Artistother){
61 returnsuper.equalsTo(other)&&hair.equals(other.hair);
62 }
63
64 }
65
66 publicenumColor{
67 BROWN,DARK,BLONDE,RED,GRAY;
68 }
69

70 publicstaticclassHairimplementsEquatable<Hair>{
71 publicfinalintlength;
72 publicfinalSet<Color>colors;
73
74 publicHair(intlength,Set<Color>colors){
75 this.colors=colors;
76 this.length=length;
77 }
78
79 // 1.c
80 @Override
81 publicbooleanequals(Objecto){ // uguale a Person.equals(Object)
82 if(this==o)
83 returntrue;
84 if(o==null)
85 returnfalse;
86 if(getClass()!=o.getClass())
87 returnfalse;
88 returnequalsTo((Hair)o);
89 }
90
91 // 1.c
92 @Override
93 publicbooleanequalsTo(Hairx){
94 returncolors.equals(x.colors)&&length==x.length;
95 }
96 }
97
98
99 publicstaticvoidprint(Stringfmt,Object...o){
100 System.out.println(String.format(fmt,o));
101 }
102
103 publicstaticvoidmain(String[]args){
104 Personalice=newPerson("Alice",23),
105 david=newArtist("Bowie",69,newHair(75,Set.of(Color.RED,Color.BROWN,
Color.GRAY)));
106
107 Artistmorgan=newArtist("Morgan",47,newHair(20,Set.of(Color.GRAY,Color.DARK))),
108 madonna=newArtist("Madonna",60,newHair(50,Set.of(Color.BLONDE)));
109
110 // 1.g (lanciate il programma per printare le risposte booleane)
111 List<Boolean>bs=Arrays.asList(
112 alice.equals(null), // false
113 alice.equals(alice), // true
114 //null.equals(david), // alcuni compilatori rifiutano null.metodo(), altri
lo accettano ma a runtime viene lanciato NullPointerException
115 alice.equals(david), // false
116 alice.equalsTo(morgan), // false
117 morgan.equals(morgan), // true
118 morgan.equals(madonna), // false
119 morgan.equals(david), // false
120 //morgan.equalsTo(david), // non compila
121 david.equalsTo(morgan), // false
122 madonna.equals(3) // false
123 //, madonna.equalsTo("Madonna") // non compila
124 );

125 print("[1.g] %s",bs);
126
127 // 1.h
128 List<Artist>artists=Arrays.asList((Artist)david,morgan,madonna);
129 List<Person>persons=Arrays.asList(alice,david,morgan,madonna);
130
131 // 1.h.i
132 Artista=Collections.max(artists,newComparator<Artist>(){
133 @Override
134 publicintcompare(Artista,Artistb){
135 returna.hair.length*a.hair.colors.size()-b.hair.length*
b.hair.colors.size();
136 }
137 });
138 print("[1.h.i] %s",a);
139
140 // 1.h.ii
141 Personb=Collections.max(persons,newComparator<Person>(){
142 @Override
143 publicintcompare(Persona,Personb){
144 returna.name.compareTo(b.name);
145 }
146 });
147 print("[1.h.ii] %s",b);
148
149 // 1.h.iii: RISPOSTA: 2
150 Artistc=Collections.max(artists,newComparator<Person>(){
151 publicintcompare(Persona,Personb){
152 returna.age-b.age;
153 }
154 });
155
156 }
157
158
159 }
```

**Spiegazione rapida del ragionamento**

La gerarchia `Person -> Artist` con `equalsTo()` parametrico mostra come estendere il confronto di uguaglianza in modo type-safe, evitando cast non sicuri.

---

#### 2.1.13 Iteratori

**Testo dell'esercizio**

```text
[Esame30/05/2019– 22/01/2019]S ii m p l e m e n t ii ls e g u e n t es i s t e m ad ii t e r a t o r iet r a s f o r m a z i o n it r ai t e r a t o r i .
1.Si deﬁnisca unainterfaccia funzionalesimile ajava.util.BiFunctionche rappresenta funzioni binarie, parame-
trica sui 2 tipi degli argomenti e sul tipo di ritorno.
2.Si deﬁnisca un tipo per la coppia eterogenea immutabile, ovvero una classe Pairparametrica su 2 tipi distinti che
rappresentano rispettivamente il tipo del primo e del secondo elemento della coppia.
3.Si deﬁnisca un tipo per la tripla eterogenea immutabile, ovvero una sottoclasse diPairdi nomeTriple,p a r a m e t r i c a
su 3 tipi distinti che rappresentano i tipi dei 3 elementi.
4.Si deﬁnisca un metodo staticoevalIteratorgenerico su 3 tipiA,BeCche, dato un iteratore su coppieA≃Bed
una funzione binariaA≃B↗C,p r o d u c eu nn u o v oi t e r a t o r es ut r i p l eA≃B≃Cche si comporta come unwrapperdi
quello in input. L’iteratore in output applica la funzione binaria ad ogni coppia di elementi letti dall’iteratore in
input e produce una tripla con i due valori appena letti e passati alla funzione assieme al risultato di quest’ultima.
Si utilizzino i tipiPair,TripleeBiFunctiondeﬁniti nei punti precedenti.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 importjava.util.*;
2
3 publicclassEs2{
4
5
6 // 2.a
7 @FunctionalInterface
8 publicinterfaceBiFunction<A,B,C>{
9 Capply(Aa,Bb);
10 }
11
12 // 2.b
13 publicstaticclassPair<A,B>{
14 publicfinalAfst;
15 publicfinalBsnd;
16
17 publicPair(Aa,Bb){
18 this.fst=a;
19 this.snd=b;

20 }
21 }
22
23 // 2.c
24 publicstaticclassTriple<A,B,C>extendsPair<A,B>{
25 publicfinalCtrd;
26
27 publicTriple(Aa,Bb,Cc){
28 super(a,b);
29 this.trd=c;
30 }
31 }
32
33 // 2.d
34 publicstatic<A,B,C>Iterator<Triple<A,B,C>>evalIterator(Iterator<Pair<A,B>>it,
BiFunction<A,B,C>f){
35 returnnewIterator<>(){
36 @Override
37 publicbooleanhasNext(){
38 returnit.hasNext();
39 }
40
41 @Override
42 publicTriple<A,B,C>next(){
43 Pair<A,B>p=it.next();
44 returnnewTriple<>(p.fst,p.snd,f.apply(p.fst,p.snd));
45 }
46 };
47 }
48
49 }
```

**Spiegazione rapida del ragionamento**

Definire un `Iterator<T>` dedicato con stato interno rende la scansione riproducibile e conforme al contratto Java (hasNext/next). Il for-each funziona automaticamente su `Iterable<T>`.

---

#### 2.1.14 Sequenza di Fibonacci

**Testo dell'esercizio**

```text
[Esame 30/05/2019]
1.Si implementi un classeFiboSequencele cui istanze rappresentano sequenze contigue di numeri di Fibonacci di
lunghezza data in costruzione. Tali istanze devono essere iterabilitramite il costruttofor-eachdi Java, devono
pertanto implementare l’interfaccia parametrica del JDKjava.util.Iterable<T>.A d e s e m p i o , i l s e g u e n t e c o d i c e
deve compilare e stampare i primi 100 numeri di Fibonacci:
1 for(intn:newFiboSequence(100)){
2 System.out.println(n);
3 }
Requisito obbligatorio è che i numeri di Fibonacci generati dall’iteratore8sottostiano ad un meccanismo dicaching
che ne allevia il costo computazionale memorizzando il risultato di ogni passo di ricorsione, in modo che ogni
computazione successiva con il medesimo input costi solamente un accesso in lettura alla cache.
2.Si scriva il codicede-zuccheratodello statementfor-eachdi cui al punto precedente.
3.Si riscriva la classeFiboSequencein modo che la cache sia condivisa tra molteplici istanze.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 importjava.util.HashMap;
2 importjava.util.Iterator;
3 importjava.util.Map;
4
5 publicclassEs3{
6
7 // 3.a
8 publicstaticclassFiboSequenceimplementsIterable<Integer>{
9
10 privatefinalintmax;
11 privatefinalMap<Integer,Integer>cache=newHashMap<>();
12
13 publicFiboSequence(intmax){
14 this.max=max;
15 }
16
17 @Override
18 publicIterator<Integer>iterator(){
19 returnnewIterator<>(){
20 privateinti=0;
21
22 @Override
23 publicbooleanhasNext(){
24 returni<max;

25 }
26
27 @Override
28 publicIntegernext(){
29 returnfib(i++);
30 }
31
32 privateintfib(intn){
33 if(n<2)return1;
34 else{
35 Integerx=cache.get(n);
36 if(x!=null)returnx;
37 else{
38 intr=fib(n-1)+fib(n-2);
39 cache.put(n,r); // si commenti questo statement per
40 // disabilitare la cache e vedere la differenza
41 // enorme di tempi di calcolo
42 returnr;
43 }
44 }
45 }
46 };
47 }
48 }
49
50 // si lanci questo main e si osservi quanto velocemente la macchina produce 100 numeri di
fibonacci:
51 // se non ci fosse la cache sarebbe drammaticamente più lento a causa delle continue
ricorsioni
52 publicstaticvoidmain(String[]args){
53 for(intn:newFiboSequence(100)){
54 System.out.println(n);
55 }
56 }
57
58
59 // 3.b
60 publicstaticvoidmain__desugared(){
61 // questo for senza il terzo statement è equivalente ad un while
62 for(Iterator<Integer>it=newFiboSequence(100).iterator();it.hasNext();){
63 intn=it.next();
64 System.out.println(n);
65 }
66 }
67
68
69 // 3.c
70 publicstaticclassGlobalFiboSequenceimplementsIterable<Integer>{
71
72 privatefinalintmax;
73 privatefinalstaticMap<Integer,Integer>cache=newHashMap<>();// è tutto uguale a
74 // FiboSequence
eccetto per
75 //la cache che è
statica
76
77 publicGlobalFiboSequence(intmax){

78 this.max=max;
79 }
80
81 @Override
82 publicIterator<Integer>iterator(){
83 returnnewIterator<>(){
84 privateinti=0;
85
86 @Override
87 publicbooleanhasNext(){
88 returni<max;
89 }
90
91 @Override
92 publicIntegernext(){
93 returnfib(i++);
94 }
95
96 privateintfib(intn){
97 if(n<2)return1;
98 else{
99 Integerx=cache.get(n);
100 if(x!=null)returnx;
101 else{
102 intr=fib(n-1)+fib(n-2);
103 cache.put(n,r); // si commenti questo statement per disabilitare
la cache
104 // e vedere la differenza enorme di tempi di
calcolo
105 returnr;
106 }
107 }
108 }
109 };
110 }
111 }
112
113 }
```

**Spiegazione rapida del ragionamento**

Generare Fibonacci tramite un iteratore (lazy) evita di calcolare tutta la sequenza in anticipo, utile per sequenze lunghe o potenzialmente infinite.

---

#### 2.1.15 Classe Random

**Testo dell'esercizio**

```text
[Esame 04/09/2018]S ip r e n d ai nc o n s i d e r a z i o n eq u e s t as e m p l i ﬁ c a z i o n ed e l l ac l a s s ejava.util.Randomdel JDK:
essenzialmente essa o!re un costruttore senza parametri, un secondo costruttore con il seed per inizializzare il PRNG
ed alcuni metodi per generare valori numerici di tipo di!erente:
1 publicclassRandom{
2 publicRandom(){...}
3 publicRandom(intseed){...}
4 publicbooleannextBoolean(){...}
5 publicintnextInt(){...}
6 publicdoublenextDouble(){...}
7 }
8Nell’esempio l’iteratore è implicitamente utilizzato dal costrutto for-each.

1.Si scriva unwrapperdella classeRandomche si comporta come unsingleton,f a c e n d oa t t e n z i o n ear i p r o d u r r eo g n i
aspetto dell’originale.
2.Si deﬁnisca una classeRandomIteratorche implementa l’interfacciajava.util.Iterator<Integer>del JDK e
che si comporta come un iteratore su interi, generando un numero casuale ad ogni invocazione del metodo next()
anziché scorrendo una vera collection, ﬁno ad esaurire la sequenza di lunghezza speciﬁcata in costruzione. Si
implementino opportunamente il costruttore ed i metodi richiesti dall’interfaccia.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 importorg.jetbrains.annotations.NotNull;
2 importorg.jetbrains.annotations.Nullable;
3
4 importjava.util.Iterator;
5 importjava.util.Random;
6
7 publicclassEs7{
8
9 // 7.b
10 publicstaticclassRandomSingleton{
11 privateRandomSingleton(){
12 }
13
14 @Nullable
15 privatestaticRandominstance;
16
17 @NotNull

18 privatestaticRandominstance(){
19 if(instance==null)
20 instance=newRandom();
21 returninstance;
22 }
23
24 // fare un setter per cambiare il seed del PRNG è uno dei modi possibili per riprodurre
25 // il comportamento originale della classe Random del JDK. Normalmente, per avere un
26 // seed specifico è necessario costruire un oggetto Random tramite l 'apposito
27 // costruttore. Nella nostra conversione a singleton rappresentiamo invece questo
28 // aspetto come un setter che internamente reistanzia il singleton in maniera
29 // trasparente all'utente.
30 publicvoidsetSeed(intseed){
31 instance=newRandom(seed);
32 }
33
34 publicstaticintnextInt(){
35 returninstance().nextInt();
36 }
37
38 publicstaticbooleannextBoolean(){
39 returninstance().nextBoolean();
40 }
41
42 publicstaticdoublenextDouble(){
43 returninstance().nextDouble();
44 }
45
46 }
47
48 // 7.c
49 publicstaticclassRandomIteratorimplementsIterator<Integer>{
50 privateintlen;
51
52 publicRandomIterator(intlen){
53 this.len=len;
54 }
55
56 @Override
57 publicbooleanhasNext(){
58 returnlen>0;
59 }
60
61 @Override
62 publicIntegernext(){
63 --len;
64 returnRandomSingleton.nextInt();
65 }
66 }
67
68 }
```

**Spiegazione rapida del ragionamento**

Incapsulare `Random` in una classe dedicata con interfaccia ristretta migliora testabilita', controllo dello stato e consente mock nei test.

---

#### 2.1.16 FancyArrayList

**Testo dell'esercizio**

```text
[Esame 20/06/2018]S ii m p l e m e n t ii nJ a v au n as o t t o c l a s s eg e n e r i c ad iArrayListdi nomeFancyArrayListche estende
le funzionalità della superclasse con un iteratore più versatile. Il nuovo iteratore deve essere in grado di andare sia
avanti che indietro secondo un valore di incremento intero non nullo; e di processare gli elementi che incontra durante
l’attraversamento applicando una funzione di trasformazione9.
1.Si deﬁnisca unainterfaccia funzionaledi nomeFunctionparametrica sia sul tipo del dominio che sul tipo del
codominio, equivalente a quella deﬁnita dal JDK 8+ nel package java.util.function.
2.Si deﬁnisca la sottoclasseFancyArrayListparametrica su un tipoEes ii m p l e m e n t iu nm e t o d op u b b l i c oa v e n t eﬁ r -
maIterator<E>iterator(intstep,Function<E,E>f)che crea un iteratore con le caratteristiche accennate
sopra tramite unaclasse anonima.P i ù p r e c i s a m e n t e :
•quando il valore del parametrostepèp o s i t i v o ,l ’ i t e r a t o r ep a r t ed a l l ’ i n i z i od e l l ac o l l e z i o n eev aavantiincre-
mentando il cursore distepposizioni ad ogni passo; quando invece stepèn e g a t i v o ,l ’ i t e r a t o r ep a r t ed a l l a
ﬁne della collezione e vaindietrodecrementando il cursore;
•ad ogni passo l’iteratore applica la funzione di trasformazione fall’elemento da restituire.
3.Si aggiungano aFancyArrayListis e g u e n t im e t o d ip u b b l i c i ,b a d a n d oa di m p l e m e n t a r l ii nf u n z i o n ed e lm e t o d o
iterator(int,Function<E,E>)realizzato per l’esercizio precedente,senza replicazionidi codice. Per ciascuno
si speciﬁchi inoltre se è un override oppure no, utilizzando opportunamente l’annotazione @Override.
(a)Si implementi il metodo avente ﬁrmaIterator<E>iterator()che produce un iteratore convenzionale che
procede in avanti di una posizione alla volta.
(b)Si implementi il metodo avente ﬁrmaIterator<E>backwardIterator()che produce un iteratore rovescio
che procede indietro di una posizione alla volta.
4.Si rifattorizzi il metodoiterator(int,Function<E,E>)realizzato per il punto (2)i nm o d oc h en o nu s iu n a
classe anonima, ma una nuova classe innestatastaticaep a r a m e t r i c ad in o m eFancyIterator.S i p r e s t i p a r t i c o l a r e
attenzione all’uso dei generics ed al passaggio esplicito della enclosing instanceal costruttore.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 importjava.util.ArrayList;
2 importjava.util.Iterator;
3
4 publicclassEs10{

5
6 publicstaticclassFancyArrayList<E>extendsArrayList<E>{
7
8 // 10.a
9 @FunctionalInterface
10 publicinterfaceFunction<A,B>{
11 Bapply(Aa);
12 }
13
14 // 10.b
15 publicIterator<E>iterator(intstep,Function<E,E>f){
16 returnnewIterator<>(){
17 privateintpos=step>0?0:size()-1;
18
19 @Override
20 publicbooleanhasNext(){
21 returnpos>=0&&pos<size();
22 }
23
24 @Override
25 publicEnext(){
26 Er=get(pos);
27 pos+=step;
28 returnf.apply(r);
29 }
30 };
31 }
32
33 // 10.c.i
34 @Override
35 publicIterator<E>iterator(){
36 returniterator(1,(x)->x); // si può fare l 'identità con la lambda (solo Java
8+)
37 }
38
39 // 10.c.ii
40 publicIterator<E>backwardIterator(){
41 returniterator(-1,newFunction<>(){// oppure con una anonymous class
semplicissima
42 @Override
43 publicEapply(Ex){
44 returnx;
45 }
46 });
47 }
48
49 // 10.d
50 // bisogna chiamare questo metodo con un nome diverso da iterator()
51 // altrimenti il sorgente non compila
52 publicIterator<E>iterator__refactored(intstep,Function<E,E>f){
53 returnnewFancyIterator<>(this,step,f);
54 }
55
56 // questa classe è il refactoring della anonynmous class dell 'esercizio 10.b:
57 // la differenza è che conserva la enclosing instance esplicitamente
58 privatestaticclassFancyIterator<E>implementsIterator<E>{
59 privateFancyArrayList<E>enclosing;

60 privateFunction<E,E>f;
61 privateintstep,pos;
62
63 publicFancyIterator(FancyArrayList<E>parent,intstep,Function<E,E>f){
64 this.enclosing=parent;
65 this.step=step;
66 this.f=f;
67 pos=step>0?0:parent.size()-1;
68 }
69
70 @Override
71 publicbooleanhasNext(){// questo metodo è uguale
72 returnpos>=0&&pos<enclosing.size();
73 }
74
75 @Override
76 publicEnext(){
77 Er=enclosing.get(pos);
78 pos+=step;
79 returnf.apply(r);
80 }
81 }
82
83 }
84
85 }
```

**Spiegazione rapida del ragionamento**

`FancyArrayList` estende `ArrayList` aggiungendo operazioni funzionali (`map`, `filter`, ecc.) mantenendo compatibilita' con l'API standard.

---

#### 2.1.17 Minimo e massimo in una lista

**Testo dell'esercizio**

```text
[Esame 20/06/2018]S ir e a l i z z ii nJ a v au na l g o r i t m oc h et r o v ai lm i n i m oe di lm a s s i m oi nu n al i s t ag e n e r i c aer e s t i t u i s c a
ir i s u l t a t i ,r i s p e t t i v a m e n t e ,c o m ep r i m oes e c o n d oe l e m e n t od iu n ac o p p i ao m o g e n e a .
1.Si deﬁnisca un tipo per la coppia eterogenea, ovvero una classe Pairparametrica su due tipi distintiAeBche
rappresentano rispettivamente il tipo del primo e del secondo elemento della coppia.
2.Si implementi un metodo statico e generico avente la seguente ﬁrma 10:
static<E>Pair<E,E>findMinAndMax(List<E>l,Comparator<E>c)
L’algoritmo di ricerca del minimo e del massimo deve eseguire una sola traversatadella lista; si assuma che gli
argomenti siano non-nulli e che la lista abbia sempre almeno un elemento.
3.Si deﬁnisca un metodo in overload con il precedente che non usi un Comparatorma aggiunga il constraintComp->
arableal genericE,s e c o n d ol as e g u e n t eﬁ r m a :
static<EextendsComparable<E>>Pair<E,E>findMinAndMax(List<E>l)
Si implementi questo metodo in funzione del precedente, senza replicarel’algoritmo di ricerca.
9Una funzione di trasformazione è una funzione in cui dominio è uguale al codominio, per esempio f:ωper un qualche insiemeω.
10L’interfaccia parametricaComparator<T>include un metodo binario avente ﬁrma intcompare(Ta,Tb), il quale confronta i due
argomenti e ritorna un intero secondo la semantica standard del confronto at r ev i ein Java.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 importjava.util.Comparator;
2 importjava.util.List;
3
4 publicclassEs11{
5
6 // 11.a
7 // versione con campi pubblici immutabili, ma si potevano fare anche i getter volendo
8 publicstaticclassPair<A,B>{
9 publicfinalAa;
10 publicfinalBb;
11 publicPair(Aa,Bb){
12 this.a=a;
13 this.b=b;
14 }
15 }
16
17 // 11.b
18 publicstatic<E>Pair<E,E>findMinAndMax(List<?extendsE>l,Comparator<E>c){
19 Emin=l.get(0),max=min;
20 for(Ex:l){
21 if(c.compare(x,max)>0)max=x;
22 if(c.compare(x,min)<0)min=x;
23 }
24 returnnewPair<>(min,max);
25 }
26
27 // 11.c
28 publicstatic<EextendsComparable<E>>Pair<E,E>findMinAndMax(List<?extendsE>l){
29 returnfindMinAndMax(l,newComparator<>(){

30 @Override
31 publicintcompare(Ea,Eb){
32 returna.compareTo(b);
33 }
34 });
35 }
36
37 }
```

**Spiegazione rapida del ragionamento**

Una singola passata (O(n)) e' sufficiente per trovare sia minimo che massimo. L'uso di `Comparable` o `Comparator` rende il metodo generico su qualsiasi tipo ordinabile.

---

#### 2.1.18 Hanna Barbera

**Testo dell'esercizio**

```text
[Esame 24/05/2018]S ia s s u m a n ol es e g u e n t ic l a s s ie di n t e r f a c c eJ a v a ,i n n e s t a t ep e rb r e v i t ài nu n as o l ac l a s s ea
top-level:
1 publicclassHannaBarbera{
2
3 publicinterfaceFood{
4 intgetWeight();
5 }
6
7 publicinterfaceAnimalextendsFood{
8 voideat(Foodf);
9 }
10
11 publicstaticclassMouseimplementsAnimal{
12 privateintweight;
13
14 publicMouse(intweight){this.weight=weight;}
15
16 @Override
17 publicvoideat(Foodf){weight+=f.getWeight()/2;}
18
19 @Override
20 publicintgetWeight(){returnweight;}
21 }
22
23 publicstaticclassCatimplementsAnimal{
24 privateintweight;
25
26 publicCat(intweight){this.weight=weight;}
27
28 @Override
29 publicvoideat(Foodf){weight+=f.getWeight()/5;}
30
31 @Override
32 publicintgetWeight(){returnweight;}
33 }
34
35 publicstaticclassCheeseimplementsFood{
36 @Override
37 publicintgetWeight(){return50;}
38 }
39 }
1.Come sarebbe possibile rifattorizzare questo codice in modo da ridurre al minimo le ripetizioni e massimizzare il
riuso? Si scriva il codice rifattorizzato, facendo attenzione a non cambiare il comportamento del programma.
2.Si prenda in considerazione il seguente metodo main()per la classeHannaBarbera,s u p p o n e n d oc h el er e s t a n t i
classi ed interfacce innestate siano quelle rifattorizzate secondo quanto richiesto dall’esercizio 1.
1 publicstaticvoidmain(String[]args){
2 Animaltom=newCat(200);
3 Animaljerry=newMouse(10);
4 jerry.eat(newCheese());
5 jerry.eat(newFood(){
6 @Override
7 publicintgetWeight(){return10;}
8 });

9 tom.eat(jerry);
10 System.out.println(String.format("Tom now weights %d",tom.getWeight()));
11 }
(a)Qualora una o più parti del metodo main()non siano compatibili a causa della rifattorizzazione eseguita
precedentemente, si scriva in maniera chiara quali modiﬁche sono necessarie per renderlo compilabile mante-
nendo la semantica dell’originale; se preferibile, si riscriva per chiarezza l’intero codicemain()opportunamente
modiﬁcato.
(b)Qual’è il peso del gatto Tom che viene scritto in output alla ﬁne?
2.2 C++
2.2.1 Pair
[Esame 10/09/2024 –01/06/2023]
1.Vogliamo implementare in C++ una classe pair templatizzata su due tipi generici AeBche rappresentano i tipi
del primo e del secondo elemento della coppia. Si utilizzi la versione del linguaggio che si preferisce, è su"ciente
dichiarare quale.
(a)La classepairdeve rispettare gli stili della generic programming e comportarsi come un valore: si deﬁniscano
gli opportuni costruttori, tra cui quello per copia, e l’operatore di assegnamento.
(b)Si implementino tutti i metodi e gli operatori ritenuti necessari o interessanti.
(c)Si implementi un costruttore per copia templatizzato su due tipi generici CeDche è in grado di costruire una
pair<A,B>dato un argomento di tipoconstpair<C,D>&.
2.2.2 Matrici
[Esame 25/06/2024 – 05/09/2023 –03/06/2022]
1.Si scriva in C++ una classe genericamatrixche rappresenta matrici bidimensionali di valori di tipoT,d o v eTèu n
tipo templatizzato. Tale classe deve comportarsi come un valore,s e c o n d ol os t i l ed e icontainerdi STL, pertanto
deve supportare il costruttore per copia, l’operatore di assegnamento ed il costruttore di default. Si aggiungano
poi metodi e operatori a piacere, tra cui almenois e g u e n t i :
•costruttore con 2 parametri + 1 opzionale: numero di righe, numero di colonne e valore iniziale di tipo T
•distruttore (se necessario);
•member typeiterator,const_iteratorevalue_type;
•accesso tramite riga e colonna implementando 2 overload (const e non-const) di operator();
•2o v e r l o a d( c o n s ten o n - c o n s t )d e im e t o d ibegin()edend()per poter iterare tutta la matrice dall’angolo
superiore sinistro all’angolo inferiore destro come se fosse piatta.
L’implementazione può utilizzare STL liberamente, se lo si desidera.
Si prenda a riferimento il seguente snippet per la speciﬁca dei requisiti del tipo matrix:
1 matrix<double>m1; // non inizializzata
2 matrix<double>m2(10,20);// 10*20 inizializzata col default constructor di double
3 matrix<double>m3(m2); // costruita per copia
4 m1=m2; // assegnamento
5 m3(3,1)=11.23; // operatore di accesso come left-value
6 for(typenamematrix<double>::iteratorit=m1.begin();it!=m1.end();++it){
7 typenamematrix<double>::value_type&x=*it;// de-reference non-const
8 x=m2(0,2);// operatore di accesso come right-value
9 }
10 matrix<string>ms(5,4,"ciao");// 5*4 inizializzata col la stringa passata come terzo
argomento
11 for(typenamematrix<string>::const_iteratorit=ms.begin();it!=ms.end();++it)
12 cout<<*it;// de-reference const

2.2.3 Sommatoria
[Esame 14/06/2024]
1.Si svolgano i serguenti esercizi in linguaggio C++11.
(a)Si implementi una funzione templatizzata su un tipo Cche, dato un container STL avente tipoC,n es o m m a
tutti gli elementi tramite l’operatore di addizione e ritorna il risultato della sommatoria.
(b)Quali vincoli sul template parameter Csono implicitamente richiesti? Si scrivano dettagliatamente tutti i
vincoli per C e per gli eventuali member type utilizzati nell’implementazione.
Esempio:il tipoCdeve avere il tal metodo, deve supportare il tal costruttore, il tal operatore ecc.; poi deve
avere un certo member type e quest’ultimo deve supportare il tal metodo, operatore ecc.
2.2.4 Map
[Esame 10/01/2024]
1.Si implementino in C++11 alcune varianti della funzione map()rispettando la sua semantica tipica: la funzione
passata come argomento allamap()deve essere applicata ad ogni elemento della sequenza di input, producendo
una sequenza di output con tutti i risultati delle applicazioni.
(a)Si implementi la seguente versione dellamap()che opera su iteratori. Si badi che in questo caso la funzione
non ha tipo di ritorno. L’output consiste in un iteratore passato come argomento: lì la funzione dovrà scrivere
ir i s u l t a t i .
1 template<classInputIterator,classOutputIterator>
2 voidmap(InputIteratorfrom,InputIteratorto,OutputIteratorout,
3 function<typenameOutputIterator::value_type(typenameInputIterator::value_type)>f)
(b)Si implementi la seguente versione dellamap()che opera su vector di STL. In questo caso l’output è un vero
ep r o p r i ot i p od ir i t o r n o . L ’ i m p l e m e n t a z i o n ed e v ei n v o c a r el amap()di cui al punto precedente.
1 template<classA,classB>
2 vector<B>map(constvector<A>&v,function<B(A)>f)
(c)Il tipo templatizzatofunctionè deﬁnito da STL a partire dallo standard C++11, così come le espressioni
lambda. Nessuno dei due esisteva in C++vanilla(oggi chiamato C++03).
i.Come sarebbe stato possibile scrivere in C++03 le ﬁrme delle map()di cui ai punti (a) e (b) senza usare
il tipofunction?
ii.Le ﬁrme compatibili con C++03 potrebbero coesistere con le ﬁrme originali di cui ai punti ( a)e(b)? In
altre parole, sarebbero considerati tutti overload validi dal compilatore C++11?
iii.Come sarebbe stato possibile invocare lemap()compatibili con C++03 senza le lambda?
2.2.5 F unzioni
[Esame 30/06/2023]
1.Si implementi in C++ una classe templatizzata fun_seqanaloga a quella dell’esercizio2.1.7(Java), adottando
però i pattern, gli stili e le convenzioni di C++ e di STL. Alcuni suggerimenti:
•si usi il tipostd::pairdeﬁnito da STL per rappresentare le coppie;
•l’iterabilità in C++ è rappresentata delconceptdenominatoContainerin STL;
•la funzione di incremento passata al costruttore in Java non è necessaria in C++ e può essere sostituita dagli
opportuni operatori;
•il vincoloComparablesul type parameterXin Java non ha equivalente in C++, è su"ciente assumere che il
relativo template argument supporti un operatore di confronto.
Si scriva anche uno snippet analogo a quello del punto c(Es.2.1.7Java).
11Non è necessaria nessuna revisione recente del linguaggio, l’esercizio è esprimibile in C++ vanilla, detto C++03.

2.2.6 Curve
[Esame 13/01/2023]
1.Si prenda in considerazione la seguente classe C++14 che rappresenta curve deﬁnite tramite funzioni unarieR↗R
in un certo intervallo di dominio[a, b]⇐R.
1 #include<functional>
2 #include<iostream>
3 #include<utility>
4
5 usingnamespacestd;
6 usingreal=double;
7 usingunary_fun=function<real(constreal&)>;
8
9 #define RESOLUTION (1000) // risoluzione dell'intervallo [a, b]
10
11 classcurve
12 {
13 private:
14 reala,b;
15 unary_funf;
16 public:
17 curve(constreal&a_,constreal&b_,constunary_fun&f_):f(f_),a(a_),b(b_){}
18 curve(constreal&c)=default;
19
20 realget_dx()const{return(b-a)/RESOLUTION;}
21 pair<real,real>interval()const{returnpair<real,real>(a,b);}
22 realoperator()(constreal&x)const{returnf(x);}
23
24 curvederivative()const{/* DA IMPLEMENTARE */ }
25 curveprimitive()const{/* DA IMPLEMENTARE */ }
26 realintegral()const{/* DA IMPLEMENTARE */ }
27
28 classiterator
29 {
30 private:
31 constcurve&c;
32 realx;
33 public:
34 iterator(constcurve&c_,constreal&x_):c(c_),x(x_){}
35 iterator(constiterator&c)=default;
36
37 pair<real,real>operator*()const{/* DA IMPLEMENTARE */ }
38 iteratoroperator++(){/* DA IMPLEMENTARE */ }
39 iteratoroperator++(int){/* DA IMPLEMENTARE */ }
40 booloperator!=(constiterator&it)const{/* DA IMPLEMENTARE */ }
41 };
42
43 iteratorbegin()const{/* DA IMPLEMENTARE */ }
44 iteratorend()const{/* DA IMPLEMENTARE */ }
45 };
(a)Si implementi il metododerivative()che calcola la derivata, cioè la curva con una lambda che calcola il
rapporto di!erenzialef(x+dx)->f(x)
dx
(b)Si implementi il metodoprimitive()che calcola la primitiva, ovvero la curva che rappresenta l’integrale
indeﬁnito con una lambda che calcola il prodotto di!erenziale f(x)·dx.
(c)Si implementi il metodointegral()che calcola l’integrale deﬁnito
∫b
af(x)dx,o v v e r ol as o m m a t o r i ad e i
prodotti di!erenziali calcolati dalla primitiva nell’intervallo[a, b].

(d)Si implementino i metodi relativi all’iteratore tenendo presente che:
•il metodobegin()computa un iteratore all’inizio dell’intervallo[a, b];
•il metodoend()computa un iteratore oltre la ﬁne dell’intervallo[a, b],i n c l u d e n d ol ’ e s t r e m obnell’itera-
zione;
•l’operatore di de-reference dell’iteratore produce una coppia di reali che rappresentano un punto della
curva sul piano cartesiano;
•gli operatori di pre e post incremento incrementano l’ascissa corrente xdella quantitàdx.
Il seguente snippet di test può servire da riferimento per capire il comportamento dell’iteratore:
1 curvec(-10.,10.,[](constreal&x){returnx*x-2*x+1;});
2 for(curve::iteratorit=c.begin();it!=c.end();++it)
3 {
4 constpair<real,real>&p=*it;
5 constreal&x=p.first,&y=p.second;
6 cout<<"c("<<x<<") = "<<y<<endl;
7 }
2.2.7 Alberi binari
[Esame 13/09/2022]
1.Vogliamo realizzare in C++ lo stesso sistema di alberi binari di cui al Quesito 2.1.9(Java). La traduzione del
codice da Java a C++ non deve essere letterale: è necessario adottare gli stili e le convenzioni di C++ e di STL, ad
esempio formulando il confronto tramite l’overloading degli opportuni operatori, deﬁnendo gli opportuni member
type per gli iteratori e implementando gli overload const e non-const dei metodi laddove necessario. La classe tre->
e_nodedeve avere un template parameterTed aderire allo stile dellavalue-oriented programming, comportandosi
come un valore con gli opportuni costruttori e operatori di assegnamento. Segue la speciﬁca dettagliata.
(a)Gli alberi devono essereiterabiliel ’ i t e r a t o r ed e v ea t t r a v e r s a r el ’ a l b e r oi nm a n i e r aa l g o r i t m i c a m e n t ee q u i v a -
lente all’implementazione Java di cui al Quesitoa(Es.2.1.9Java). Si deﬁniscano imember typeper iteratori
const e non-const e le relative coppie di metodi begin()edend().S e n e c e s s a r i o , i m p l e m e n t a r e g l i i t e r a t o r i
tramite classi ausiliarie.
(b)La deﬁnizione di equivalenza di due alberi è semanticamente equivalente a quella deﬁnita nel Quesito b(Es.
2.1.9Java). Si assuma che gli oggetti di tipo Tsiano confrontabili tramite l’operatore==.
(c)Si deﬁniscano gli opportuni costruttori, operatori di assegnamento ed eventuali distruttori secondo lo stile della
value-oriented programming. Se vantaggioso, si forniscano metodi statici che fungano da pseudo-costruttori
ep e r m e t t a n od ic o s t r u i r ea l b e r if a c i l m e n t ei n n e s t a n d or i c o r s i v a m e n t el ec h i a m a t e .
(d)Si implementino uno o più snippet di test.
(e)Si implementi unpretty printertramite un overload globale dell’operatore<<.
Si utilizzi la revisione del linguaggio C++ che si preferisce, purché si speciﬁchi quale. È importante fare buon uso
del type system, riusando il codice laddove possibile.
2.2.8 Smart Pointer
[Esame 01/07/2022]
1.Si deﬁnisca in linguaggio C++ una classe smart_ptrtemplatizzata su un tipoTche implementi la logica di uno
smart pointer.S i i m p l e m e n t i n o t u t t i i c o s t r u t t o r i , i d i s t r u t t o r i e g l i o p e r a t o r i c h e s i r i t e n g o n o n e c e s s a r i e d u t i l i
a"nché uno smart pointer si comporti in maniera compatibile con un pointer C. In altre parole, uno smart pointer
deve implementare non solo ilreference countingma deve anche comportarsi come un puntatore classico, inclusi
gli operatori di incremento/decremento, l’aritmetica dei puntatori ed ovviamente il de-reference.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 publicclassEs12{
2
3 // tipi dati dal testo
4
5 publicinterfaceFood{
6 intgetWeight();
7 }
8
9 publicinterfaceAnimalextendsFood{
10 voideat(Foodf);
11 }
12
13 publicstaticclassMouseimplementsAnimal{
14 privateintweight;
15
16 publicMouse(intweight){
17 this.weight=weight;
18 }
19
20 @Override
21 publicvoideat(Foodf){
22 weight+=f.getWeight()/3;
23 }
24
25 @Override
26 publicintgetWeight(){
27 returnweight;
28 }
29 }
30
31 publicstaticclassCatimplementsAnimal{
32 privateintweight;
33
34 publicCat(intweight){
35 this.weight=weight;
36 }
37
38 @Override
39 publicvoideat(Foodf){
40 weight+=f.getWeight()/10;
41 }
42
43 @Override
44 publicintgetWeight(){
45 returnweight;
46 }
47 }

48
49 publicstaticclassCheeseimplementsFood{
50 @Override
51 publicintgetWeight(){
52 return300;
53 }
54 }
55
56
57 // 12.a
58 // le interfacce Food e Animal non serve rifattorizzarle, sono a posto così
59 publicabstractclassAbstractAnimalimplementsAnimal{
60 privateintweight,div;
61
62 protectedAbstractAnimal(intweight,intdiv){
63 this.weight=weight;
64 }
65
66 @Override
67 publicvoideat(Foodf){
68 weight+=f.getWeight()/div;
69 }
70
71 @Override
72 publicintgetWeight(){
73 returnweight;
74 }
75 }
76
77 publicclassCatextendsAbstractAnimal{
78 protectedCat(intweight,intdiv){
79 super(weight,5); // valori del tema A
80 }
81
82 publicclassMouseextendsAbstractAnimal{
83 protectedMouse(intweight,intdiv){
84 super(weight,2);
85 }
86 }
87
88
89 // 12.b.i
90 publicstaticvoidmain(String[]args){
91 Animaltom=newCat(200); // costanti del tema A
92 Animaljerry=newMouse(10);
93 jerry.eat(newCheese());
94 jerry.eat(newFood(){
95 @Override
96 publicintgetWeight(){
97 return10;
98 }
99 });
100 tom.eat(jerry);
101 System.out.println(String.format("Tom now weights %d",tom.getWeight()));//b.ii 208
102
103 }
104 }
```

**Spiegazione rapida del ragionamento**

La modellazione dei personaggi Hanna Barbera usa la gerarchia per condividere comportamenti comuni, con override per comportamenti specifici di ogni personaggio.

---

### 2.2 C++

#### 2.2.1 Pair

**Testo dell'esercizio**

```text
[Esame 10/09/2024 –01/06/2023]
1.Vogliamo implementare in C++ una classe pair templatizzata su due tipi generici AeBche rappresentano i tipi
del primo e del secondo elemento della coppia. Si utilizzi la versione del linguaggio che si preferisce, è su"ciente
dichiarare quale.
(a)La classepairdeve rispettare gli stili della generic programming e comportarsi come un valore: si deﬁniscano
gli opportuni costruttori, tra cui quello per copia, e l’operatore di assegnamento.
(b)Si implementino tutti i metodi e gli operatori ritenuti necessari o interessanti.
(c)Si implementi un costruttore per copia templatizzato su due tipi generici CeDche è in grado di costruire una
pair<A,B>dato un argomento di tipoconstpair<C,D>&.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 exportmodulepairs;
2
3 import<string>;
4 import<functional>;
5
6
7 export
8 template<classA,typenameB>
9 classpair
10 {
11 template<classC,typenameD>friendclasspair; // necessario per il copy
constructor templatizzato
12
13 private:
14 Afirst;
15 Bsecond;
16
17 public:
18 // 2.a
19 pair():first(),second(){}
20 pair(constA&a,constB&b):first(a),second(b){}
21 pair(constpair<A,B>&p):first(p.first),second(p.second){}
22
23 pair<A,B>&operator=(constpair<A,B>&p)
24 {
25 first=p.first;
26 second=p.second;
27 return*this;
28 }
29
30 // 2.c
31 template<classC,typenameD>
32 pair(constpair<C,D>&p):first(p.first),second(p.second){}
33
34
35 // 2.b
36 pair<A,B>operator++(int)
37 {
38 pair<A,B>tmp(*this);
39 first++;
40 second++;
41 returntmp;
42 }
43
44 pair<A,B>&operator++()
45 {
46 ++first;
47 ++second;
48 return*this;
49 }
50
51 booloperator==(constpair<A,B>&p)const
52 {
53 returnfirst==p.first&&second==p.second;

54 }
55
56 booloperator!=(constpair<A,B>&p)const
57 {
58 return!(*this==p);
59 }
60
61 // altri operatori aritmentici ed in-place sono analoghi
62 pair<A,B>operator+(constpair<A,B>&p)const
63 {
64 returnpair<A,B>(first+p.first,second+p.second);
65 }
66
67 pair<A,B>&operator+=(constpair<A,B>&p)
68 {
69 first+=p.first;
70 second+=p.second;
71 return*this;
72 }
73
74 constA&fst()const
75 {
76 returnfirst;
77 }
78
79 A&fst()
80 {
81 returnfirst;
82 }
83
84 constB&snd()const
85 {
86 returnsecond;
87 }
88
89 B&snd()
90 {
91 returnsecond;
92 }
93
94 };
95
96
97 exportintmain()
98 {
99 pair<int,int>p1(4,5);
100 pair<int,int>p2(p1);
101
102 pair<std::string,bool>p3("ciao",true);
103 pair<double,double>p4(p1);
104
105 p1=p2;
106
107 intn=p1.fst();
108 p1.snd()=p1.snd()*3;
109 p4+=p1; // converte implicitamente il RV in un pair<double, double> tramite un
conversion copy-constructor templatizzato

110
111 return0;
112 }
```

**Spiegazione rapida del ragionamento**

Il template `pair<A,B>` e' un esempio classico di contenitore generico C++: sfrutta la deduzione dei tipi e gli operatori per massimizzare espressivita' e type safety.

---

#### 2.2.2 Matrici

**Testo dell'esercizio**

```text
[Esame 25/06/2024 – 05/09/2023 –03/06/2022]
1.Si scriva in C++ una classe genericamatrixche rappresenta matrici bidimensionali di valori di tipoT,d o v eTèu n
tipo templatizzato. Tale classe deve comportarsi come un valore,s e c o n d ol os t i l ed e icontainerdi STL, pertanto
deve supportare il costruttore per copia, l’operatore di assegnamento ed il costruttore di default. Si aggiungano
poi metodi e operatori a piacere, tra cui almenois e g u e n t i :
•costruttore con 2 parametri + 1 opzionale: numero di righe, numero di colonne e valore iniziale di tipo T
•distruttore (se necessario);
•member typeiterator,const_iteratorevalue_type;
•accesso tramite riga e colonna implementando 2 overload (const e non-const) di operator();
•2o v e r l o a d( c o n s ten o n - c o n s t )d e im e t o d ibegin()edend()per poter iterare tutta la matrice dall’angolo
superiore sinistro all’angolo inferiore destro come se fosse piatta.
L’implementazione può utilizzare STL liberamente, se lo si desidera.
Si prenda a riferimento il seguente snippet per la speciﬁca dei requisiti del tipo matrix:
1 matrix<double>m1; // non inizializzata
2 matrix<double>m2(10,20);// 10*20 inizializzata col default constructor di double
3 matrix<double>m3(m2); // costruita per copia
4 m1=m2; // assegnamento
5 m3(3,1)=11.23; // operatore di accesso come left-value
6 for(typenamematrix<double>::iteratorit=m1.begin();it!=m1.end();++it){
7 typenamematrix<double>::value_type&x=*it;// de-reference non-const
8 x=m2(0,2);// operatore di accesso come right-value
9 }
10 matrix<string>ms(5,4,"ciao");// 5*4 inizializzata col la stringa passata come terzo
argomento
11 for(typenamematrix<string>::const_iteratorit=ms.begin();it!=ms.end();++it)
12 cout<<*it;// de-reference const
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 // Questo sorgente contiene le soluzioni dell 'esame scritto di PO2 del 3/6/2022 per ciò che
riguarda il quesito 6, ovvero la domanda che coinvolge C++.
2 // I quesiti 1-5 riguardanti Java sono in un progetto IntelliJ a parte, non qui.
3 // Il codice C++ qui esposto è standard C++ vanilla (a.k.a. C++03), sebbene il progetto VS sia
configurato con il compilatore di default C++14
4
5 #include<iostream>
6 #include<vector>
7
8 usingnamespacestd;
9
10 // 6
11 template<classT>
12 classmatrix
13 {
14 private:
15 size_tcols;
16 vector<T>v;
17
18 public:
19 matrix():cols(0),v(){}
20 matrix(size_trows,size_tcols_):cols(cols_),v(rows*cols){}
21 matrix(size_trows,size_tcols_,constT&v):cols(cols_),v(rows*cols,v){}
22 matrix(constmatrix<T>&m):cols(m.cols),v(m.v){}
23
24 typedefTvalue_type;
25 typedeftypenamevector<T>::iteratoriterator;
26 typedeftypenamevector<T>::const_iteratorconst_iterator;
27
28 matrix<T>&operator=(constmatrix<T>&m)
29 {
30 v=m.v;
31 return*this;
32 }
33
34 T&operator()(size_ti,size_tj)
35 {
36 returnv[i*cols+j];
37 }
38
39 constT&operator()(size_ti,size_tj)const
40 {
41 return(*this)(i,j);
42 }
43
44 iteratorbegin()
45 {
46 returnv.begin();
47 }
48
49 iteratorend()
50 {

51 returnv.end();
52 }
53
54 const_iteratorbegin()const
55 {
56 returnbegin();
57 }
58
59 const_iteratorend()const
60 {
61 returnend();
62 }
63 };
64
65
66 intmain()
67 {
68 matrix<double>m1; // non inizializzata
69 matrix<double>m2(10,20);// 10*20 inizializzata col default constructor di double
70 matrix<double>m3(m2); // costruita per copia
71 m1=m2; // assegnamento
72 m3(3,1)=11.23; // operatore di accesso come left-value
73
74 for(typenamematrix<double>::iteratorit=m1.begin();it!=m1.end();++it){
75 typenamematrix<double>::value_type&x=*it; // de-reference non-const
76 x=m2(0,
2); //
operatore di accesso come right-value

77 }
78
79 matrix<string>ms(5,4,"ciao");// 5*4 inizializzata col la stringa passata come terzo
argomento
80 for(typenamematrix<string>::const_iteratorit=ms.begin();it!=ms.end();++it)
81 cout<<*it; // de-reference const
82 }
```

**Spiegazione rapida del ragionamento**

La matrice template separa rappresentazione (array bidimensionale) da operazioni (operatori `*`, `[]`). Gli iteratori const/non-const permettono uso in contesti read-only.

---

#### 2.2.3 Sommatoria

**Testo dell'esercizio**

```text
[Esame 14/06/2024]
1.Si svolgano i serguenti esercizi in linguaggio C++11.
(a)Si implementi una funzione templatizzata su un tipo Cche, dato un container STL avente tipoC,n es o m m a
tutti gli elementi tramite l’operatore di addizione e ritorna il risultato della sommatoria.
(b)Quali vincoli sul template parameter Csono implicitamente richiesti? Si scrivano dettagliatamente tutti i
vincoli per C e per gli eventuali member type utilizzati nell’implementazione.
Esempio:il tipoCdeve avere il tal metodo, deve supportare il tal costruttore, il tal operatore ecc.; poi deve
avere un certo member type e quest’ultimo deve supportare il tal metodo, operatore ecc.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 // Questo sorgente contiene le soluzioni dell 'esame scritto di PO2 del 5/9/2023 per ciò che
riguarda il quesito 2, ovvero la domanda che coinvolge C++.
2 // I quesiti 1-5 riguardanti Java sono in un sorgente Java a parte, non qui.
3 // Il codice C++ qui esposto è standard C++ vanilla (a.k.a. C++03), sebbene il progetto VS sia
configurato con il compilatore di default C++14
4
5 #include<iostream>
6 #include<vector>
7
8 usingnamespacestd;
9
10 template<classContainer>
11 ostream&operator<<(ostream&os,constContainer&c)
12 {
13 os<<"[";
14 for(typenameContainer::const_iteratorit=c.begin();it!=c.end();++it)
15 os<<""<<*it;
16 os<<"]";
17 returnos;
18 }

19
20 template<classContainer>
21 typenameContainer::value_typesum(constContainer&c)
22 {
23 typenameContainer::value_typer;
24 for(typenameContainer::const_iteratorit=c.begin();it!=c.end();++it)
25 {
26 r+=*it;
27 }
28 returnr;
29 }
30
31
32 intmain()
33 {
34 }
```

**Spiegazione rapida del ragionamento**

La sommatoria generica usa template + `operator+` per funzionare su qualsiasi tipo numerico. L'identita' additiva (valore zero) deve essere gestita con attenzione.

---

#### 2.2.4 Map

**Testo dell'esercizio**

```text
[Esame 10/01/2024]
1.Si implementino in C++11 alcune varianti della funzione map()rispettando la sua semantica tipica: la funzione
passata come argomento allamap()deve essere applicata ad ogni elemento della sequenza di input, producendo
una sequenza di output con tutti i risultati delle applicazioni.
(a)Si implementi la seguente versione dellamap()che opera su iteratori. Si badi che in questo caso la funzione
non ha tipo di ritorno. L’output consiste in un iteratore passato come argomento: lì la funzione dovrà scrivere
ir i s u l t a t i .
1 template<classInputIterator,classOutputIterator>
2 voidmap(InputIteratorfrom,InputIteratorto,OutputIteratorout,
3 function<typenameOutputIterator::value_type(typenameInputIterator::value_type)>f)
(b)Si implementi la seguente versione dellamap()che opera su vector di STL. In questo caso l’output è un vero
ep r o p r i ot i p od ir i t o r n o . L ’ i m p l e m e n t a z i o n ed e v ei n v o c a r el amap()di cui al punto precedente.
1 template<classA,classB>
2 vector<B>map(constvector<A>&v,function<B(A)>f)
(c)Il tipo templatizzatofunctionè deﬁnito da STL a partire dallo standard C++11, così come le espressioni
lambda. Nessuno dei due esisteva in C++vanilla(oggi chiamato C++03).
i.Come sarebbe stato possibile scrivere in C++03 le ﬁrme delle map()di cui ai punti (a) e (b) senza usare
il tipofunction?
ii.Le ﬁrme compatibili con C++03 potrebbero coesistere con le ﬁrme originali di cui ai punti ( a)e(b)? In
altre parole, sarebbero considerati tutti overload validi dal compilatore C++11?
iii.Come sarebbe stato possibile invocare lemap()compatibili con C++03 senza le lambda?
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 // Scritto PO2 10 9 24.cpp : This file contains the 'main'function. Program execution begins and
ends there.
2 //
3
4 #include<iostream>
5 #include<functional>
6 #include<vector>
7 #include<type_traits>
8
9 usingnamespacestd;
10
11 // 2.a
12 template<classInputIterator,classOutputIterator>
13 voidmap(InputIteratorfrom,InputIteratorto,OutputIteratorout,function<typename
OutputIterator::value_type(typenameInputIterator::value_type)>f)
14 {
15 while(from!=to)
16 *out++=f(*from++);
17 }
18
19 // 2.b
20 template<classA,classB>
21 vector<B>map(constvector<A>&v,function<B(A)>f)
22 {
23 vector<B>r(v.size());
24 map(v.begin(),v.end(),r.begin(),f);
25 returnr;
26 }
27
28 // 2.c
29 namespacecpp03{
30
31 // 2.c.i
32 template<classA,classB,classF>
33 vector<B>map(constvector<A>&v,Ff)
34 {
35 vector<B>r(v.size());
36 for(inti=0;i<v.size();++i)
37 r[i]=f(v[i]);

38 returnr;
39 }
40
41 // 2.c.ii
42 // no, non possono coesistere, perché sarebbero overload ambigui. Infatti ho dovuto metterli
in un sotto-namespace a parte per far compilare questo sorgente.
43
44 // 2.c.iii
45 // bisogna usare i function object, cioè oggetti per cui è definito l 'operatore di
applicazione operator()
46 // esempio:
47 classmyfunction{
48 public:
49 booloperator()(intn){returnn>2;}
50 };
51
52 voidtest()
53 {
54 vector<int>v1{1,2,3,4,5};
55 vector<bool>v2=map<int,bool>(v1,myfunction());// senza le annotazioni esplicite dei
template argument non compila (questo non è richiesto nell 'esame perché è una cosa
molto sottile)

56 }
57 }
58
59 // main for testing
60 //
61
62 template<classT>
63 ostream&operator<<(ostream&os,constvector<T>&v)
64 {
65 os<<"[ ";
66 for(autoit=v.begin();it!=v.end();++it)
67 os<<*it<<", ";
68 os<<"\b\b]";
69 returnos;
70 }
71
72 intmain()
73 {
74 vector<int>v1{1,2,3,4,5};
75 vector<bool>v2(v1.size());
76 map(v1.begin(),v1.end(),v2.begin(),[](intn){returnn>2;});
77
78 v2=map<int,bool>(v1,[](intn){returnn>2;});// anche qui bisogna mettere i template
argument espliciti
79
80 cout<<v1<<endl<<v2<<endl;
81
82 return0;
83 }
```

**Spiegazione rapida del ragionamento**

La `map` template usa `operator[]` per accesso chiave-valore. La gestione dell'allocazione rispetta RAII per evitare leak.

---

#### 2.2.5 F unzioni

**Testo dell'esercizio**

```text
[Esame 30/06/2023]
1.Si implementi in C++ una classe templatizzata fun_seqanaloga a quella dell’esercizio2.1.7(Java), adottando
però i pattern, gli stili e le convenzioni di C++ e di STL. Alcuni suggerimenti:
•si usi il tipostd::pairdeﬁnito da STL per rappresentare le coppie;
•l’iterabilità in C++ è rappresentata delconceptdenominatoContainerin STL;
•la funzione di incremento passata al costruttore in Java non è necessaria in C++ e può essere sostituita dagli
opportuni operatori;
•il vincoloComparablesul type parameterXin Java non ha equivalente in C++, è su"ciente assumere che il
relativo template argument supporti un operatore di confronto.
Si scriva anche uno snippet analogo a quello del punto c(Es.2.1.7Java).
11Non è necessaria nessuna revisione recente del linguaggio, l’esercizio è esprimibile in C++ vanilla, detto C++03.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 exportmodulepairs;
2
3 import<iostream>;
4 import<functional>;

5 import<utility>;
6
7 usingnamespacestd;
8
9 export
10 template<classX,classY>
11 classfun_seq
12 {
13 private:
14 constXa,b,dx;
15 constfunction<Y(constX&)>f;
16
17 public:
18 usingvalue_type=pair<X,Y>;
19
20 fun_seq(constX&_a,constX&_b,constX&_dx,constfunction<Y(constX&)>&_f):
a(_a),b(_b),dx(_dx),f(_f){}
21
22 classiterator
23 {
24 private:
25 Xx;
26 fun_seq<X,Y>that;
27
28 public:
29 explicititerator(constX&_x,constfun_seq<X,Y>&_that):x(_x),that(_that)
{}
30
31 iterator(constiterator&it):x(it.x),that(it.that){}
32
33 pair<X,Y>operator*()const
34 {
35 returnpair<X,Y>(x,that.f(x));
36 }
37
38 booloperator!=(constiterator&it)
39 {
40 return!(x>it.x-that.dx&&x<it.x+that.dx);
41 }
42
43 iteratoroperator++()
44 {
45 iteratorr(*this);
46 x+=that.dx;
47 returnr;
48 }
49 };
50
51 iteratorbegin()const
52 {
53 returniterator(a,*this);
54 }
55
56 iteratorend()const
57 {
58 returniterator(b,*this);
59 }

60
61
62 };
63
64
65 exportintmain()
66 {
67 fun_seq<double,double>sq1(-2.,2.,0.1,[](constdouble&x){returnx*x+2*x-
1;});
68 for(pair<double,double>p:sq1)
69 cout<<"f("<<p.first<<") = "<<p.second<<endl;
70
71 fun_seq<double,int>sq2(-2.,2.,0.1,[](constdouble&x){returnint(x*x+2*x-
1);});
72 for(pair<double,int>p:sq2)
73 cout<<"f("<<p.first<<") = "<<p.second<<endl;
74
75 return0;
76 }
```

**Spiegazione rapida del ragionamento**

Le funzioni come callable (`operator()`) in C++ consentono composizione e stato locale (functors). Le lambda C++11+ sono zucchero sintattico sulla stessa idea.

---

#### 2.2.6 Curve

**Testo dell'esercizio**

```text
[Esame 13/01/2023]
1.Si prenda in considerazione la seguente classe C++14 che rappresenta curve deﬁnite tramite funzioni unarieR↗R
in un certo intervallo di dominio[a, b]⇐R.
1 #include<functional>
2 #include<iostream>
3 #include<utility>
4
5 usingnamespacestd;
6 usingreal=double;
7 usingunary_fun=function<real(constreal&)>;
8
9 #define RESOLUTION (1000) // risoluzione dell'intervallo [a, b]
10
11 classcurve
12 {
13 private:
14 reala,b;
15 unary_funf;
16 public:
17 curve(constreal&a_,constreal&b_,constunary_fun&f_):f(f_),a(a_),b(b_){}
18 curve(constreal&c)=default;
19
20 realget_dx()const{return(b-a)/RESOLUTION;}
21 pair<real,real>interval()const{returnpair<real,real>(a,b);}
22 realoperator()(constreal&x)const{returnf(x);}
23
24 curvederivative()const{/* DA IMPLEMENTARE */ }
25 curveprimitive()const{/* DA IMPLEMENTARE */ }
26 realintegral()const{/* DA IMPLEMENTARE */ }
27
28 classiterator
29 {
30 private:
31 constcurve&c;
32 realx;
33 public:
34 iterator(constcurve&c_,constreal&x_):c(c_),x(x_){}
35 iterator(constiterator&c)=default;
36
37 pair<real,real>operator*()const{/* DA IMPLEMENTARE */ }
38 iteratoroperator++(){/* DA IMPLEMENTARE */ }
39 iteratoroperator++(int){/* DA IMPLEMENTARE */ }
40 booloperator!=(constiterator&it)const{/* DA IMPLEMENTARE */ }
41 };
42
43 iteratorbegin()const{/* DA IMPLEMENTARE */ }
44 iteratorend()const{/* DA IMPLEMENTARE */ }
45 };
(a)Si implementi il metododerivative()che calcola la derivata, cioè la curva con una lambda che calcola il
rapporto di!erenzialef(x+dx)->f(x)
dx
(b)Si implementi il metodoprimitive()che calcola la primitiva, ovvero la curva che rappresenta l’integrale
indeﬁnito con una lambda che calcola il prodotto di!erenziale f(x)·dx.
(c)Si implementi il metodointegral()che calcola l’integrale deﬁnito
∫b
af(x)dx,o v v e r ol as o m m a t o r i ad e i
prodotti di!erenziali calcolati dalla primitiva nell’intervallo[a, b].

(d)Si implementino i metodi relativi all’iteratore tenendo presente che:
•il metodobegin()computa un iteratore all’inizio dell’intervallo[a, b];
•il metodoend()computa un iteratore oltre la ﬁne dell’intervallo[a, b],i n c l u d e n d ol ’ e s t r e m obnell’itera-
zione;
•l’operatore di de-reference dell’iteratore produce una coppia di reali che rappresentano un punto della
curva sul piano cartesiano;
•gli operatori di pre e post incremento incrementano l’ascissa corrente xdella quantitàdx.
Il seguente snippet di test può servire da riferimento per capire il comportamento dell’iteratore:
1 curvec(-10.,10.,[](constreal&x){returnx*x-2*x+1;});
2 for(curve::iteratorit=c.begin();it!=c.end();++it)
3 {
4 constpair<real,real>&p=*it;
5 constreal&x=p.first,&y=p.second;
6 cout<<"c("<<x<<") = "<<y<<endl;
7 }
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 #include<functional>
2 #include<iostream>
3 #include<utility>
4 #include<algorithm>
5
6 usingnamespacestd;
7
8
9 usingreal=double;
10 usingunary_fun=function<real(constreal&)>;
11
12 #define RESOLUTION (10)
13
14
15 classcurve
16 {
17
18 private:
19 reala,b;
20 unary_funf;
21
22 public:
23 curve(constreal&a_,constreal&b_,constunary_fun&f_):f(f_),a(a_),b(b_){}
24
25 realget_dx()const{return(b-a)/RESOLUTION;}
26
27 pair<real,real>interval()const{returnpair<real,real>(a,b);}
28
29 curvederivative()const
30 {
31 returncurve(a,b,[&,dx=get_dx()](constreal&x){ // anche la capture
[=] sarebbe stata sufficiente
32 constrealdy=f(x+dx)-f(x);
33 returndy/dx;
34 });
35 }

36
37 curveprimitive()const
38 {
39 returncurve(a,b,[&,dx=get_dx()](constreal&x){ // oppure la [&]
40 constrealy=f(x);
41 returny*dx;
42 });
43 }
44
45 realintegral()const
46 {
47 constunary_fun&F=primitive();
48 returnF(b)-F(a);
49 }
50
51 realoperator()(constreal&x)const
52 {
53 returnf(x);
54 }
55
56 classiterator
57 {
58 private:
59 constcurve&c;
60 realx;
61
62 public:
63 iterator(constcurve&c_,constreal&x_):c(c_),x(x_){}
64
65 pair<real,real>operator*()const
66 {
67 returnpair<real,real>(x,c.f(x));
68 }
69
70 iterator&operator++() // il pre-incremento modifica sè stesso e non
ritorna una copia
71 {
72 x+=c.get_dx();
73 return*this;
74 }
75
76 iteratoroperator++(int) // il post-incremento fa una copia, modifica sé
stesso e ritorna la copia
77 {
78 autor(*this);
79 ++(*this);
80 returnr;
81 }
82
83 booloperator!=(constiterator&it)const
84 {
85 returnfabs(x-it.x)>=c.get_dx(); // non si confrontano mai i
float direttamente con l'operatore di uguaglianza o disuguaglianza
86 }
87 };
88
89 iteratorbegin()const

90 {
91 returniterator(*this,a);
92 }
93
94 iteratorend()const
95 {
96 returniterator(*this,b+get_dx());
97 }
98
99 };
100
101
102 ostream&operator<<(ostream&os,constcurve&c)
103 {
104 os<<"dom = ["<<c.interval().first<<", "<<c.interval().second<<"] dx = " <<
c.get_dx()<<": "<<endl;
105 /*for (const pair<real, real>& p : c)
106 {
107 const real& x = p.first, & y = p.second;
108 os << "\tf(" << x << ") = " << y << endl;
109 }*/
110 for(curve::iteratorit=c.begin();it!=c.end();++it)
111 {
112 constpair<real,real>&p=*it;
113 constreal&x=p.first,&y=p.second;
114 os<<"\tf("<<x<<") = "<<y<<endl;
115 }
116 returnos<<endl;
117 }
118
119 intmain()
120 {
121 curvef(-1.,1.,[](constreal&x){returnx*x;});
122
123 cout<<f<<endl
124 <<f.derivative()<<endl
125 <<f.primitive()<<endl
126 <<f.primitive().derivative()<<endl
127 <<f.derivative().primitive()<<endl
128 ;
129 return0;
130 }
```

**Spiegazione rapida del ragionamento**

Le curve come gerarchia di classi con `operator<<` per stampa e metodi virtuali per calcolo permettono di trattare uniformemente curve diverse con interfaccia comune.

---

#### 2.2.7 Alberi binari

**Testo dell'esercizio**

```text
[Esame 13/09/2022]
1.Vogliamo realizzare in C++ lo stesso sistema di alberi binari di cui al Quesito 2.1.9(Java). La traduzione del
codice da Java a C++ non deve essere letterale: è necessario adottare gli stili e le convenzioni di C++ e di STL, ad
esempio formulando il confronto tramite l’overloading degli opportuni operatori, deﬁnendo gli opportuni member
type per gli iteratori e implementando gli overload const e non-const dei metodi laddove necessario. La classe tre->
e_nodedeve avere un template parameterTed aderire allo stile dellavalue-oriented programming, comportandosi
come un valore con gli opportuni costruttori e operatori di assegnamento. Segue la speciﬁca dettagliata.
(a)Gli alberi devono essereiterabiliel ’ i t e r a t o r ed e v ea t t r a v e r s a r el ’ a l b e r oi nm a n i e r aa l g o r i t m i c a m e n t ee q u i v a -
lente all’implementazione Java di cui al Quesitoa(Es.2.1.9Java). Si deﬁniscano imember typeper iteratori
const e non-const e le relative coppie di metodi begin()edend().S e n e c e s s a r i o , i m p l e m e n t a r e g l i i t e r a t o r i
tramite classi ausiliarie.
(b)La deﬁnizione di equivalenza di due alberi è semanticamente equivalente a quella deﬁnita nel Quesito b(Es.
2.1.9Java). Si assuma che gli oggetti di tipo Tsiano confrontabili tramite l’operatore==.
(c)Si deﬁniscano gli opportuni costruttori, operatori di assegnamento ed eventuali distruttori secondo lo stile della
value-oriented programming. Se vantaggioso, si forniscano metodi statici che fungano da pseudo-costruttori
ep e r m e t t a n od ic o s t r u i r ea l b e r if a c i l m e n t ei n n e s t a n d or i c o r s i v a m e n t el ec h i a m a t e .
(d)Si implementino uno o più snippet di test.
(e)Si implementi unpretty printertramite un overload globale dell’operatore<<.
Si utilizzi la revisione del linguaggio C++ che si preferisce, purché si speciﬁchi quale. È importante fare buon uso
del type system, riusando il codice laddove possibile.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1
2 // Questo sorgente contiene le soluzioni dell 'esame scritto di PO2 del 1/7/2022 per ciò che
riguarda il quesito 2, ovvero l 'esercizio di C++.
3 // Il quesito 1 riguardante Java è in un progetto IntelliJ a parte, non qui.
4 // Il codice qui esposto è C++14.
5 // ATTENZIONE: il codice qui fornito è ricco di dettagli e complessità, allo scopo di fornire
materiale di studio. La versione richiesta all 'esame è molto più semplice.
6
7 #include<iostream>
8 #include<iterator>
9 #include<cstddef>
10 #include<vector>
11

12 usingstd::vector;
13
14 #define EASY_ITERATOR // commentare questa macro per compilare la versione ottimizzata
degli iteratori
15
16 template<classT>
17 classtree_node
18 {
19 public:
20 Tdata;
21 tree_node<T>*left,*right;
22
23 staticboolare_equal(consttree_node<T>*a,consttree_node<T>*b)
24 {
25 returna==b||(a!=nullptr&&b!=nullptr&&*a==*b);
26 }
27
28 public:
29 // 2.c
30
31 tree_node()=default;
32 tree_node(consttree_node<T>&t)=default;
33 tree_node<T>&operator=(consttree_node<T>&t)=default;
34
35 ~tree_node()
36 {
37 if(left!=nullptr)
38 {
39 deleteleft;
40 left=nullptr;
41 }
42 if(right!=nullptr)
43 {
44 deleteright;
45 right=nullptr;
46 }
47 }
48
49 tree_node(constT&v,tree_node<T>*l,tree_node<T>*r):data(v),left(l),right(r)
50 {
51 #i f d e f E A S Y _ I T E R A T O R
52 prepopulate();
53 #e l s e
54 if(left!=nullptr)left->parent=this;
55 if(right!=nullptr)right->parent=this;
56 #e n d i f
57 }
58
59 // 2.b
60
61 booloperator==(consttree_node<T>&t)const
62 {
63 returndata==t.data&&are_equal(left,t.left)&&are_equal(right,t.right);
64 }
65
66 // 2.a
67

68 usingvalue_type=T;
69
70
71 // implementazione facile degli iteratori, suggerita pubblicamente dal docente durante
l'appello del 13/9/22
72 //
73
74 #i f d e f E A S Y _ I T E R A T O R
75
76 usingconst_iterator=typenamevector<T>::const_iterator;
77 usingiterator=typenamevector<T>::iterator;
78
79 private:
80 // per motivi di semplicità ogni nodo ha un campo di tipo vector che viene popolato al
volo per essere iterator
81 // si faccia attenzione ad un particolare: per poter ritornare begin() ed end() dello
stesso vector, bisogna conservarlo come membro del nodo
82 vector<T>children;
83
84 voiddfs(vector<T>&v)
85 {
86 // perché non popoliamo direttamente il campo children? il motivo è molto
sottile: ogni nodo ha un suo campo children, ma noi non vogliamo che ogni
nodo popoli il proprio vector; noi vogliamo

87 // che quando decidiamo di popolare il campo di children di un certo nodo,
allora viene popolato il vector di QUEL nodo
88 // ogni nodo quindi ha un suo campo children che viene popolato con i SUOI
sottorami: tutto ciò è uno spreco di spazio, certo, però permette di fare
iteratori a partire da QUALUNQUE nodo

89 // in maniera semplice e immutabile
90 v.push_back(data);
91 if(left!=nullptr)left->dfs(v);
92 if(right!=nullptr)right->dfs(v);
93 }
94
95 // questa viene chiamata dal costruttore: ogni nodo prepopola il proprio campo children
96 voidprepopulate()
97 {
98 dfs(children);
99 }
100
101 public:
102
103 const_iteratorbegin()const
104 {
105 returnchildren.begin();
106 }
107
108 const_iteratorend()const
109 {
110 returnchildren.end();
111 }
112
113 iteratorbegin()
114 {
115 returnchildren.begin();
116 }

117
118 iteratorend()
119 {
120 returnchildren.end();
121 }
122
123
124 // implementazione ottimizzata degli iteratori
125 //
126
127 #e l s e
128
129 private:
130 tree_node<T>*parent;
131
132 statictree_node<T>*get_next_node(consttree_node<T>*n){
133 if(n->left!=nullptr)
134 returnn->left;
135 elseif(n->right!=nullptr)
136 returnn->right;
137 else{
138 while(n->parent!=nullptr){
139 consttree_node<T>*last=n;
140 n=n->parent;
141 if(n->right!=nullptr&&n->right!=last)
142 returnn->right;
143 }
144 returnnullptr;
145 }
146 }
147
148 public:
149 // iteratore non-const
150 classmy_iterator
151 {
152 friendclassmy_const_iterator; // questo serve perché altrimenti
my_const_iterator non può accedere al campo current di my_iterator
153
154 private:
155 tree_node<T>*current;
156
157 public:
158 usingiterator_category=std::forward_iterator_tag; // questo member type
indica che si tratta di un ForwardIterator, si veda la doc di STL per i
dettagli

159 usingdifference_type=std::ptrdiff_t;
160 usingvalue_type=T;
161 usingpointer=T*;
162 usingreference=T&;
163
164 my_iterator()=default;
165 my_iterator(constmy_iterator&i)=default;
166 my_iterator&operator=(constmy_iterator&i)=default;
167
168 my_iterator(tree_node<T>*t):current(t){}
169
170 referenceoperator*(){returncurrent->data;}

171 pointeroperator->(){return&current->data;}
172
173 my_iterator&operator++()
174 {
175 current=get_next_node(current);
176 return*this;
177 }
178
179 my_iteratoroperator++(int)
180 {
181 my_iteratorr(*this);
182 current=get_next_node();
183 returnr;
184 }
185
186 booloperator==(constmy_iterator&i)const{returncurrent==i.current;}
187 booloperator!=(constmy_iterator&i)const{return!(*this==i);}
188
189 };
190
191 // interatore const
192 // si noti come sono praticamente uguali a parte il fatto che gesticono un nodo const
oppure no, con conseguente impatto in tutti i tipi di ritorno dei vari operatori
193 // questa replicazione di codice sarebbe evitabile solamente tramite un complesso uso dei
template, troppo complesso per questo corso
194 // chi è interessato a sapere come evitare questa duplicazione di codice dovuta a const
vs. non-const può approfondire qui:
https://stackoverflow.com/questions/765148/how-to-remove-constness-of-const-iterator

195 classmy_const_iterator
196 {
197 private:
198 consttree_node<T>*current;
199
200 public:
201 usingiterator_category=std::forward_iterator_tag;
202 usingdifference_type=std::ptrdiff_t;
203 usingvalue_type=constT;
204 usingpointer=constT*;
205 usingreference=constT&;
206
207 my_const_iterator()=default;
208 my_const_iterator(constmy_const_iterator&i)=default;
209 my_const_iterator&operator=(constmy_const_iterator&i)=default;
210
211 // questo costruttore è molto interessante: permette di costruire un
my_const_iterator dato un my_iterator: in altre parole possiamo convertire un
iteratore non-const in uno const

212 // il motivo per cui è necessario è se chiamiamo begin() su un tree_node
non-const ma vogliamo un const_iterator perché lo leggiamo soltanto
213 my_const_iterator(constmy_iterator&i):current(i.current){}
214
215 my_const_iterator(consttree_node<T>*t):current(t){}
216
217 referenceoperator*()const {returncurrent->data; }
218 pointeroperator->()const{return&current->data; }
219
220 my_const_iterator&operator++()

221 {
222 current=get_next_node(current);
223 return*this;
224 }
225
226 my_const_iteratoroperator++(int)
227 {
228 my_const_iteratorr(*this);
229 current=get_next_node();
230 returnr;
231 }
232
233 booloperator==(constmy_const_iterator&i)const{ returncurrent==
i.current;}
234 booloperator!=(constmy_const_iterator&i)const{ return!(*this==i);
}
235
236 };
237
238 // definiamo questi member type perché sono quelli che i Container STL solitamente
definiscono
239 usingconst_iterator=my_const_iterator; // rebinding della nested class
my_const_iterator definita sopra
240 usingiterator=my_iterator; // rebinding della nested
class my_iterator definita sopra
241 usingvalue_type=T;
242
243 const_iteratorbegin()const
244 {
245 returnconst_iterator(this);
246 }
247
248 const_iteratorend()const
249 {
250 returnconst_iterator(nullptr);
251 }
252
253 iteratorbegin()
254 {
255 returniterator(this);
256 }
257
258 iteratorend()
259 {
260 returniterator(nullptr);
261 }
262
263 #e n d i f
264
265 };
266
267 // 2.c
268 // pseudo-costruttori
269 // invece di fare metodi statici facciamo funzioni templatizzate globali, così il template
argument è inferito e diventano più comode da usare
270
271 template<classT>

272 tree_node<T>*lr(constT&v,tree_node<T>*l,tree_node<T>*r)
273 {
274 returnnewtree_node<T>(v,l,r);
275 }
276
277 template<classT>
278 tree_node<T>*l(constT&v,tree_node<T>*n)
279 {
280 returnnewtree_node<T>(v,n,nullptr);
281 }
282
283 template<classT>
284 tree_node<T>*r(constT&v,tree_node<T>*n)
285 {
286 returnnewtree_node<T>(v,nullptr,n);
287 }
288
289 template<classT>
290 tree_node<T>*v(constT&v)
291 {
292 returnnewtree_node<T>(v,nullptr,nullptr);
293 }
294
295 // 2.e
296
297 template<classT>
298 std::ostream&operator<<(std::ostream&os,consttree_node<T>&t)
299 {
300 os<<t.data;
301 if(t.left!=nullptr)os<<"("<<*t.left<<")";
302 if(t.right!=nullptr)os<<"["<<*t.right<<"]";
303 returnos;
304 }
305
306 usingnamespacestd;
307
308 // 2.d
309
310 intmain()
311 {
312 autot1=
313 shared_ptr<tree_node<int>>( // usiamo gli shared_ptr per non
doverci ricordare di fare delete
314 // con i pseudo-costruttori globali è comodissimo costruire un albero,
basta innestare le chiamate
315 lr(1,
316 lr(2,
317 v(3),
318 v(4)),
319 r(5,
320 lr(6,
321 v(7),
322 v(8)))));
323
324 autot2=
325 shared_ptr<tree_node<int>>(
326 lr(1,

327 r(5, // il sottoalbero
destro di t2 è uguale al sinistro di t1 e viceversa
328 lr(6,
329 v(7),
330 v(8))),
331 lr(2,
332 v(3),
333 v(4))));
334
335 // test dell'operatore di stream (<<)
336 cout<<"pretty printer: "<<endl
337 <<"t1: "<<*t1<<endl // dereferenziamo per stampare perché il nostro
operator<< non vuole un pointer ma un reference
338 <<"t2: "<<*t2<<endl;
339
340 // test dell'operatore di uguaglianza (==)
341 cout<<"equality: "<<(*t1==*t2)<<", "<<(*t1->left==*t2->right)<<
endl; // dereferenziamo gli operandi sinistro e destro del nostro operator==
perchè non accetta pointer ma reference

342
343 // test dell'iteratore non-const
344 cout<<"iterator: ";
345 for(tree_node<int>::iteratorit=t1->begin();it!=t1->end();++it)
346 {
347 int&n=*it; // dereferenziando l'iteratore abbiamo accesso
non-const al dato dentro il nodo
348 n*=2; // il campo data in ogni nodo può quindi
essere modificato
349 cout<<n<<"";
350 }
351 cout<<endl<<"t1 modificato: "<<*t1<<endl; // ristampiamo t1 dopo le
modifiche
352
353 // test dell'iteratore const
354 cout<<"const iterator: ";
355 for(tree_node<int>::const_iteratorit=t1->begin();it!=t1->end();++it) //
t1->begin() ritorna un iterator, che viene convertito in un const_iterator dal
costruttore alla linea 142

356 {
357 constint&n=*it; // dereferenziando l'iteratore abbiamo accesso const
al dato dentro il nodo, quindi non possiamo modificarlo ma solo leggerlo
358 cout<<n<<"";
359 }
360 cout<<endl;
361
362
363
364 }
```

**Spiegazione rapida del ragionamento**

L'albero binario value-oriented gestisce copia profonda, move e RAII tramite costruttori/distruttori. L'iteratore implementa la traversata in-order rispettando il contratto STL.

---

#### 2.2.8 Smart Pointer

**Testo dell'esercizio**

```text
[Esame 01/07/2022]
1.Si deﬁnisca in linguaggio C++ una classe smart_ptrtemplatizzata su un tipoTche implementi la logica di uno
smart pointer.S i i m p l e m e n t i n o t u t t i i c o s t r u t t o r i , i d i s t r u t t o r i e g l i o p e r a t o r i c h e s i r i t e n g o n o n e c e s s a r i e d u t i l i
a"nché uno smart pointer si comporti in maniera compatibile con un pointer C. In altre parole, uno smart pointer
deve implementare non solo ilreference countingma deve anche comportarsi come un puntatore classico, inclusi
gli operatori di incremento/decremento, l’aritmetica dei puntatori ed ovviamente il de-reference.
```

**Soluzione** *(spostata dalla sezione 4 del PDF originale)*

```text
1 // Questo sorgente contiene le soluzioni dell 'esame scritto di PO2 del 1/7/2022 per ciò che
riguarda il quesito 3, ovvero la domanda che coinvolge C++.
2 // I quesiti 1-2 riguardanti Java sono in un progetto IntelliJ a parte, non qui.
3 // Il codice qui esposto è C++14 per qualche piccolo particolare, ma in gran parte è
essenzialmente vanilla.
4
5 #include<iostream>

6 #include<vector>
7
8 template<classT>
9 classsmart_ptr
10 {
11 private:
12 T*pt;
13 ptrdiff_toffset;
14 boolis_array;
15
16 usingcounter_t=unsignedshort;
17
18 counter_t*cnt;
19
20 voiddec()
21 {
22 if(cnt!=nullptr&&--(*cnt)==0)
23 {
24 if(pt!=nullptr) // se non punta a nulla non c 'èn i e n t ed a
liberare
25 {
26 if(is_array)deletept;
27 elsedelete[]pt;
28 }
29 deletecnt;
30 }
31 }
32
33 voidinc()
34 {
35 if(cnt!=nullptr)++(*cnt);
36 }
37
38 public:
39 usingvalue_type=T;
40
41 smart_ptr():pt(nullptr),offset(0),is_array(false),cnt(nullptr){}
42
43 explicitsmart_ptr(T*pt_,boolis_array_=false):pt(pt_),offset(0),
is_array(is_array_),cnt(newcounter_t(1)){}
44
45 smart_ptr(constsmart_ptr<T>&p):pt(p.pt),offset(p.offset),is_array(p.is_array),
cnt(p.cnt)
46 {
47 inc();
48 }
49
50 ~smart_ptr()
51 {
52 dec();
53 }
54
55 smart_ptr<T>&operator=(constsmart_ptr<T>&p)
56 {
57 dec();
58 pt=p.pt;
59 cnt=p.cnt;

60 offset=p.offset;
61 is_array=p.is_array;
62 inc();
63 return*this;
64 }
65
66 // subscript
67 // l'operatore di subscript è l 'unica implementazione reale; tutti gli altri operatori
sono implementati in funzione di questo
68 // questo approccio è poco error-prone perché concentra solamente qui il calcolo esatto
dell'indirizzo usando l'offset
69 // in altre parole, l 'operatore di subscript funge da API interna a basso livello; tutto
il resto è costruito sopra di essa
70 constT&operator[](size_ti)const
71 {
72 returnpt[offset+i];
73 }
74 T&operator[](size_ti)
75 {
76 // capita spesso che l 'implementazione const e l'implementazione non-const siano
identiche
77 // in questi casi è necessario duplicare il codice, che è una pratica inelegante
ed error-prone
78 // questo trucco sfrutta un giro di const cast per rimandare questa
implementazione a quella const appena sopra, che è l 'unica che implementiamo
davvero

79 // ATTENZIONE: tutto questo NON è richiesto dal tema d 'esame, lo mostriamo
solamente a scopo didattico per insegnare una tecnica avanzata di
non-duplicazione del codice

80 returnconst_cast<T&>(const_cast<constsmart_ptr<T>&>(*this).operator[](i));
81 }
82
83 // de-reference
84 constT&operator*()const
85 {
86 // usa l'operatore di subscript
87 returnpt[0];
88 }
89 T&operator*()
90 {
91 // stessa tecnica per non duplicare: chiamiamo la versione const di questo
operatore definita qui sopra, che è l 'unica che implementiamo davvero
92 returnconst_cast<T&>(const_cast<constsmart_ptr<T>&>(*this).operator*());
93 }
94
95 // field access
96 constT*operator->()const
97 {
98 // anche questa implementazione sfrutta altri operatori scritti sopra: in
particolare usa de-reference che a sua volta usa il subscript, in questo modo
non richiede manutenzione

99 return&*(*this);
100 // ^
101 // si faccia attenzione a un particolare: solo l 'operatore * indicato dalla
freccetta invoca l'overload definito da noi
102 // il de-reference di this dentro le parentesi tonde e l 'operatore & più esterno
invocano gli operatori nativi di C++, non i nostri overload

103 }
104 T*operator->()
105 {
106 // altro uso della tecnica avanzata per non duplicare l 'implementazione non-const
107 // cerchiamo di capirla: lo scopo è chiamare l 'implementazione const di questo
operatore senza duplicare il codice
108 // 1) trasformiamo *this (che in questo scope è di tipo smart_ptr<T>&) in un
const smart_ptr<T>&
109 // 2) ora che *this è castato a const, invochiamo l 'operatore o il metodo che ci
interessa: la risoluzione dell'overload risolverà l'implementazione const,
non farà una ricorsione!

110 // 3) siccome stiamo invocando la versione const, il risultato è const: ma il
nostro tipo di ritorno deve essere un T* non-const, pertanto bisogna
const-castare per TOGLIERE il const

111 returnconst_cast<T*>(const_cast<constsmart_ptr<T>&>(*this).operator->());
112 }
113
114 // plus
115 smart_ptr<T>&operator+=(ptrdiff_toff)
116 {
117 offset+=off;
118 return*this;
119 }
120 smart_ptr<T>operator+(ptrdiff_toff)const
121 {
122 // questa implementazione usa il copy-constructor e l 'operator+= definito qui
sopra
123 returnsmart_ptr<T>(*this)+=off;
124 }
125
126 // minus
127 smart_ptr<T>&operator-=(ptrdiff_toff)
128 {
129 offset-=off; // analogo al +=
130 return*this;
131 }
132 smart_ptr<T>operator-(ptrdiff_toff)
133 {
134 returnsmart_ptr<T>(*this)-=off;
135 }
136
137 // pre
138 smart_ptr<T>&operator++()
139 {
140 // questa implementazione usa l 'operator+= e basta
141 return*this+=1;
142 }
143 smart_ptr<T>&operator--()
144 {
145 return*this-=1; // analogo al ++ ma usa il -=
146 }
147
148 // post
149 smart_ptr<T>operator++(int)
150 {
151 smart_ptr<T>r(*this); // copia

152 ++(*this); // pre-incrementa this: usiamo il
pre-incremento affinché questa implementazione dipenda totalmente
dall'implementazione di operator++

153 returnr; // ritorna la copia
154 }
155 smart_ptr<T>operator--(int)
156 {
157 smart_ptr<T>r(*this);
158 --(*this); // analogo a ++
159 returnr;
160 }
161
162 };
163
164 usingstd::string;
165
166 intmain()
167 {
168 int*a=newint[5];
169 smart_ptr<int>a1(a,true),a2;
170 a2=a1+10;
171 a1-=3;
172 a1=++a1-*a2--+a1[3];
173
174 smart_ptr<double>b(newdouble[10],true);
175 for(unsignedinti=0;i<10;++i)
176 b++;
177
178 string*sp=newstring("ciao");
179 smart_ptr<string>s1(sp),s2;
180 s2=s1;
181 size_ti=s2->find('a',0);
182
183
184
185 }
```

**Spiegazione rapida del ragionamento**

Lo smart pointer con reference counting (`shared_ptr`-like) implementa RAII: il distruttore decrementa il contatore e libera la memoria solo quando il count raggiunge zero.

---

## Sezione 3 – Domande aperte

Le seguenti domande verificano le conoscenze generali sugli argomenti del corso.

```text
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
