# EL PATRÓ "ITERATOR"

------------


El patró de disseny Iterator és un dels patrons més utilitzats en la programació orientada a objectes i és part dels patrons de disseny de comportament. Aquest patró proporciona una manera eficient de recórrer una col·lecció d'elements, sense exposar la seva implementació interna.

### PROPÒSIT
El propòsit principal del patró Iterador és proporcionar una manera estandarditzada d'accedir seqüencialment als elements d'una col·lecció sense exposar els detalls de la seva implementació interna. Això permet a les col·leccions ser recorregudes de manera uniforme, independentment de la seva estructura interna, i facilita la creació de clients que puguin iterar sobre aquestes col·leccions sense necessitat de conèixer els detalls de la seva implementació. A més, el patró Iterador fa que sigui més senzill afegir nous tipus de col·leccions al codi, ja que només cal proporcionar un iterador adequat per a la nova col·lecció i el codi client ja serà compatible amb aquesta. En resum, el propòsit del patró *Iterator* és proporcionar una **interface** unificada i flexible per recórrer col·leccions de manera eficient i senzilla.

### SOLUCIÓ
La solució proporcionada pel patró *Iterator* consisteix en separar l'accés als elements d'una col·lecció de la seva implementació interna. Aquesta separació es realitza mitjançant l'ús d'una `interface Iterator` que defineix un conjunt d'operacions estàndard per recórrer els elements de la col·lecció. D'aquesta manera, els clients poden utilitzar l'iterador per recórrer els elements d'una col·lecció sense necessitat de conèixer els detalls de la seva implementació interna.

La solució proporcionada pel patró *Iterator* consta dels següents elements clau:

1. **Iterador**: Defineix una "interface" comuna que especifica un conjunt d'operacions estàndard per accedir i recórrer els elements d'una col·lecció. Aquesta interfície sol habitualment incloure operacions com primer, seguent, haSeguent, actual i esFinal, per exemple.

2. **Col·lecció**: Defineix una interface comuna que especifica un mètode per crear un iterador per recórrer els elements de la col·lecció. Així, les diferents implementacions de col·leccions (com ara llistes, arbres, etc.) poden oferir un iterador adequat per recórrer els seus elements.

3. **Iterador Concret**: Implementa l'interface d'iterador i proporciona una implementació específica per recórrer els elements d'una col·lecció concreta.

4. **Col·lecció Concreta**: Implementa l'interface de col·lecció i proporciona una implementació específica per crear un iterador per recórrer els seus elements.

### TIPOS DE COL·LECCIONS
El patró Iterador és dissenyat per a ser utilitzat amb qualsevol tipus de col·lecció o estructura de dades que contingui múltiples elements i que es pugui recórrer seqüencialment. Això inclou una àmplia varietat de col·leccions, com ara:

- Llistes: Qualsevol tipus de llista, ja sigui una llista enllaçada, una llista d'arrays, una llista ordenada, etc.

- Mapes: Mapes que associen claus amb valors, com ara mapes basats en arbres, hashmaps, etc.

- Arrays: Arrays unidimensionals o multidimensionals.

- Altres estructures de dades: Qualsevol altra estructura de dades que contingui una col·lecció d'elements, com ara cues, arbres, grafos, etc.

Bàsicament, qualsevol col·lecció que proporcioni una manera d'accedir seqüencialment als seus elements pot ser recorreguda amb el patró *Iterator*. La flexibilitat del patró rau en el fet que permet als clients accedir als elements d'una col·lecció sense necessitat de conèixer la seva implementació interna, sempre que es proporcioni un iterador adequat per recórrer la col·lecció.

### ESTRUCTURA DEL PATRÓ
[![UML patró ITERATOR](https://refactoring.guru/images/patterns/diagrams/iterator/structure.png "UML patró ITERATOR")](https://refactoring.guru/images/patterns/diagrams/iterator/structure.png "UML patró ITERATOR")

En aquest diagrama:

**Iterator**: És una *interface* que declara els mètodes per recórrer una col·lecció.
**ConcreteIterator**: És una implementació concreta de la *interface* Iterator que proporciona la lògica per recórrer una col·lecció específica.
**IterableCollection**: És una interface que declara un mètode per crear un iterador.
**ConcreteCollection**: Retorna noves instàncies d'una classe iteradora concreta particular cada vegada que el client en sol·licita una.

En aquest diagrama, *Iterator* i *IterableCollection* són interfícies que defineixen els mètodes necessaris per recórrer una col·lecció. Les classes *ConcreteIterator* i *ConcreteCollection* proporcionen implementacions específiques per recórrer una col·lecció concreta.

El patró Iterator permet als clients recórrer una col·lecció d'objectes sense necessitat de conèixer els detalls de la seva implementació interna, ja que utilitzen la `interface Iterator` per accedir als elements de la col·lecció.

### EXEMPLE D'IMPLEMENTACIÓ DE PATRÓ ITERATOR 


Es declara la *interface* **Iterator**

    interface Iterator {
        seguent(): number | null;
        finalDeIteracio(): boolean;
    }

Es declara la *interface* de **IterableCollection** i es descriu un mètode per a buscar iteradors

    interface IterableCollection {
        crearIterador(): Iterator;
    }

**ConcreteIterator** implementa la *interface* Iterator. Guarda**** una referència als elements de la col·lecció i manté un índex intern per recórrer-los. El mètode *seguent()* avança l'índex i retorna l'element següent de la col·lecció. El mètode *finalDeIteracio()* comprova si l'índex ha arribat al final de la col·lecció.

    class ConcreteIterator implements Iterator {
        private index: number = 0;
        private elements: number[];
    
        constructor(elements: number[]) {
            this.elements = elements;
        }
    
        seguent(): number | null {
            if (this.index < this.elements.length) {
                return this.elements[this.index++];
            } else {
                return null;
            }
        }
    
        finalDeIteracio(): boolean {
            return this.index >= this.elements.length;
        }
    }

**ConcreteCollection**: Implementa la interface *IterableCollection*. Guarda els elements de la col·lecció. El mètode *crearIterador()* crea una instància de ConcreteIterator amb els elements de la col·lecció i la retorna.

    class ConcreteCollection implements IterableCollection {
        private elements: number[];
    
        constructor(elements: number[]) {
            this.elements = elements;
        }
    
        crearIterador(): Iterator {
            return new ConcreteIterator(this.elements);
        }
    }

Finalment utilitzem el patró creant una instància de ConcreteCollection amb una llista de nombres, obtenim un iterador cridant el mètode crearIterador() de la mateixa classe, i fem una iteració sobre els elements de la col·lecció cridant repetidament el mètode seguent()

    const numeros: number[] = [1, 2, 3, 4, 5];
    const colleccio: IterableCollection = new ConcreteCollection(numeros);
    
    const iterador: Iterator = colleccio.crearIterador();
    
    while (!iterador.finalDeIteracio()) {
        const element: number | null = iterador.seguent();
        if (element !== null) {
            console.log(element);
        }
    }


### AVANTATGES I DESAVANTATGES DEL PATRÓ
Els principals avantatges d'aquest patró son els següents:
- Desassociar el codi client de la implementació de la col·lecció.
- Simplificar el mateix codi client.
- Els iteradors poden proporcionar diversos mètodes per recórrer la col·lecció, com ara avançar al següent element, retrocedir al precedent, obtenir l'element actual, comprovar si s'ha arribat al final, etc

Per altra banda els desavantatge son: 
- La possibilitat d'afegir una capa d'abstracció extra que pot generar més complexitat 
- Un possible cost de memòria addicional, especialment en col·leccions grans i en casos de molts iteradors diferents.


##### FONTS

https://refactoring.guru/es/design-patterns/iterator
https://welcomedevelopers.es/patrones-diseno/el-patron-iterator/
https://danielggarcia.wordpress.com/2014/04/14/patrones-de-comportamiento-i-patron-iterator/
https://reactiveprogramming.io/blog/es/patrones-de-diseno/iterator
https://es.wikipedia.org/wiki/Iterador_(patr%C3%B3n_de_dise%C3%B1o)
