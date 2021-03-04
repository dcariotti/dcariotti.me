---
layout: post
title: DMI Coding Contest 04/04/2020, "La guantiera di dolcetti" versione con coda di priorità
---

La versione dell'esercizio qui di seguito de **"La guantiera di dolcetti"** (esercizio proposto dal DMI per il [Coding Contest](http://www.dmi.unict.it/~prog2/home.php) del 04/04/2020) è spiegata utilizzando una [coda di priorità](https://en.wikipedia.org/wiki/Priority_queue), ma, ovviamente, può esser risolta in modo più semplice e, forse, più ottimizzato.

### Specifiche dell'esercizio
_Si assuma che Francesco e Montalbano assegnino, indipendentemente, a ciascun dolcetto della guantiera un punteggio. I punteggi assegnati da Montalbano non saranno quindi necessariamente uguali a quelli assegnati da Francesco.
Si aiuti Francesco e il commissario Montalbano a dividersi i dolcetti alla ricotta in maniera che la somma complessiva dei punteggi assegnata ad ognuno dei dolcetti scelti sia massima. Più formalmente si dovrà massimizzare la somma dei punteggi assegnati da Montalbano ai dolcetti da lui scelti e la somma dei punteggi assegnati da Francesco ai dolcetti da lui scelti._

### Dati in input
_L'input è costituito da 100 righe, una per ogni task. Ogni riga del file di input contiene un valore N che indica il numero di dolcetti presenti nel vassoio, seguito da N coppie composte da due interi positivi. I valori dell'i-esima coppia indicano i punteggi assegnati da Montalbano e Francesco, rispettivamente, per l'i-esimo dolcetto._

### Dati in output
_Il file di output è composto da 100 righe, una per ogni task presente nel file di input. Ogni riga conterrà la massima somma possibile dei punteggi ottenuta dalla divisione dei dolcetti._

#### Facciamo attenzione alle note
1. Il valore di N è sempre compreso tra 4 e 250 ed è sempre pari.
2. I voti dei dolcetti sono sempre interi positivi compresi tra 0 e 30.
3. Lo stesso dolcetto non può essere scelto sia da Francesco che da Montalbano.
4. **ma soprattutto, una nota non scritta: Francesco e Montalbano devono avere entrambi lo stesso numero di dolcetti**

# Algoritmo
Prendiamo come esempio l'input
> 4 9 2 6 7 1 0 30 4

sappiamo che l'output sarà
> 48

questo perché, gli elementi cui somma è massima sono le coppie _(9, 30)_ e _(7, 0)_.

#### Ma come ci si arriva a dire questo?
###### Forza bruta
Un caso possibile è andare col metodo per [forza bruta](https://en.wikipedia.org/wiki/Brute-force_attack) ma che, oltre ad essere complesso da implementare, risulterebbe tedioso per input larghi. Questo perché andrebbe a controllare una ad una tutte le somme possibili. In questo caso sarebbe lungo anche l'output per il debug.

###### Coppie di valori
Controlliamo una ad una le coppie **(9|2)**, **(6|7)**, **(1|0)**, **(30|4)** considerando il primo elemento delle coppie come **i dolcetti di Montalbano** e il secondo per **Francesco** (o vice versa).
Ora controllare l'ememento della coppia e inserirlo in una lista d'appoggio.

![pseudo code](https://i.imgur.com/WzHaqIO.png)
Gli elementi di suddette liste sarebbero **[9, 1, 30]** per Francesco e **[7]** per Montalbano. Vi si evince che non abbiamo rispettato il punto _4_ delle note. E allora come si risolve? Dovremmo controllare la lista con più elementi e inserire, al posto degli elementi più piccoli, l'elemento a quell'indice.
Prendiamo l'1 (elemento più piccolo della lista) e aggiungiamo all'altra il valore nell'indice di 1 (che in questo caso è 2). Il valore è 0.
Cioè, **[9, 30]** e **[7, 0]**. E guarda caso il risultato è giusto: 46!

----

In questa tabella si vedono gli elementi selezionati e i loro indici (● sono gli elementi selezionati).
* _tabella 1_

{:.table.table-striped.table-bordered}
|  indice  | francesco  | montalbano  |
|---|---|---|
| 0  | 9 ●  | 2  |
| 1  | 6  | 7 ● |
| 2  | 1 (vecchio valore salvato)  | 0 ● |
| 3  | 30 ● | 4  |

----
Questa soluzione non funziona per tutti, perché non sempre il valore è quello che cerchiamo. Prendiamo ad esempio
> 8 10 6 2 6 4 1 6 0 1 3 7 8 3 5 4 7

Rendiamolo _zuccherino_ per far vedere le coppie
* _tabella 2_

{:.table.table-striped.table-bordered}
|  indice  | francesco  | montalbano  |
|---|---|---|
| 0 | 10 ●| 6 |
| 1 | 2 | 6 ●|
| 2 | 4 ● | 1 |
| 3 | 6 ● | 0 |
| 4 | 1 | 3 ● |
| 5 | 7 | 8 ● |
| 6 | 3 | 5 ●|
| 7 | 4 | 7 ● |

Qui abbiamo due liste con lunghezze diverse **[10, 4, 6]** e **[6, 3, 8, 5, 7]**. Seguendo l'algoritmo di prima dovremmo prendere il valore **3** della seconda lista ma, il valore che andrebbe ad inserire sarebbe 1. Il risultato sarebbe 46! **ERRATO**.
Infatti dovremmo togliere il valore **8** ed aggiungere quindi **7** nell'altra lista.

---

Ecco la tabella reale e corretta: somma 48.
* _tabella 2.1_

{:.table.table-striped.table-bordered}
|  indice  | francesco  | montalbano  |
|---|---|---|
| 0 | 10 ●| 6 |
| 1 | 2 | 6 ●|
| 2 | 4 ● | 1 |
| 3 | 6 ● | 0 |
| 4 | 1 | 3 ● |
| 5 | 7 ● | 8 |
| 6 | 3 | 5 ●|
| 7 | 4 | 7 ● |

---

Ma che fa, dovremmo tentare alla fortuna o andare per brute force?!

# Soluzione
<p>Inseriamo gli elementi nella tabella utilizzando una coda di priorità. Essa usa come struttura una coda ma con la particolarità che gli elementi verranno inseriti seguendo un ordine con complessità pari a <span>\(O(\log n)\)</span>. Prendiamo d'esempio la _tabella 1.0_ e aggiungiamo una colonna che sarà uguale alla differenza tra francesco e montalbano.</p>
* _tabella 3_

{:.table.table-striped.table-bordered}
|  indice  | differenza| francesco  | montalbano  |
|---|---|---|---|
| 0  | 7  | 9  | 2  |
| 1  | -1 | 6  | 7  |
| 2  | 1 | 1  | 0 |
| 3  | 26 | 30 | 4  |

gli elementi verranno inseriti nella coda di priorità ordinatamente in modo decrescente per **differenza**.
* _tabella 3.1_

{:.table.table-striped.table-bordered}
|  indice  | differenza| francesco  | montalbano  |
|---|---|---|---|
| 3  | 26 | 30 | 4  |
| 0  | 7  | 9  | 2  |
| 2  | 1 | 1  | 0 |
| 1  | -1 | 6  | 7  |

<p>Dato <span>\(N=4\)</span> dove <span>\(N\)</span> è il numero delle righe, sappiamo che dovremmo sommare per Francesco i primi <span>\(N/2\)</span> e per Montalbano gli ultimi <span>\(N/2\)</span>.</p>
* _tabella 3.2_

{:.table.table-striped.table-bordered}
|  indice  | differenza| francesco  | montalbano  |
|---|---|---|---|
| 3  | 26 | 30 ● | 4  |
| 0  | 7  | 9 ● | 2  |
| 2  | 1 | 1  | 0 ● |
| 1  | -1 | 6  | 7 ● |

---
Prendiamo come esempio invece il vecchio
> 8 10 6 2 6 4 1 6 0 1 3 7 8 3 5 4 7

Andiamo a modificare la _tabella 2.1_:
* _tabella 3.3_

{:.table.table-striped.table-bordered}
|  indice  | differenza | francesco  | montalbano  |
|---|---|---| ---|
| 3 | 6 | 6 ● | 0 |
| 0 | 4 | 10 ●| 6 |
| 2 | 3 |  4 ● | 1 |
| 5 | -1 | 7 ● | 8 |
| 6 | -2 | 3 | 5 ●|
| 4 | -2 |  1 | 3 ● |
| 7 | -3 | 4 | 7 ● |
| 1 | -4 | 2 | 6 ●|

Come possiamo vedere fin da subito, la somma dei valori segnati è 48.

---

# Implementazione
Dell'indice non ci facciamo più nulla, quindi non lo salveremo.
La classe [priority_queue](https://en.cppreference.com/w/cpp/container/priority_queue) di STL implementa quasi sempre **heap** come struttura dati, la quale sposta nel suo albero il valore più alto (o che rispetta la comparazione) sempre in cima.

```cpp
using tii = tuple<int, int, int>;

for(int ts = 0; ts < 100; ++ts) {
    int N;
    cin >> N;

    priority_queue<tii, vector<tii>> pq;
    tii qq;
    for(int i = 0; i < N; ++i) {
        int e1, e2;
        cin >> e1 >> e2;
        get<0>(qq) = e1-e2;
        get<1>(qq) = e1;
        get<2>(qq) = e2;

        pq.push(qq);
    }

    int counter = N;
    int sum{};

    while(counter-- > N/2) {
        qq = pq.top();
        sum += get<1>(qq);
        pq.pop();
    }

    while(!pq.empty()) {
        qq = pq.top();
        sum += get<2>(qq);
        pq.pop();
    }

    cout << sum << endl;
}
```
