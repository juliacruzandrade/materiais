## ArrayList

(Minha revisão de EDA/LEDA)

### Motivações para construção

- Criar uma estrutura que cresça 'dinamicamente'
- Tornar possível a 'remoção' de uma posição 

**IMPORTANTE**

1) A estrutura é uma abstração do Array, o que quer dizer que
esse crescimento dinâmico é, na verdade, a manipulação de um array que ocorre
embaixo dos panos.


2) Não é possível criar ArrayLists de tipos primitivos (int, double, char, etc).
O ArrayList só aceita tipos de referência, isto é, objetos. Se quiser guardar números, por exemplo, 
é preciso usar a wrapper class correspondente, como Integer para inteiros e Character para char.

### Construindo ArrayList

***Atributos do objeto ArrayList***

* Um array do tipo que se deseja armazenar (vamos usar String)
* O tamanho da lista (não é a mesma coisa que o tamanho do array!)
* Tamanho default, para um construtor que não receba a capacidade como parâmetro

```bash
private String[] list;
private int size;
private static final int DEFAULT_CAPACITY;
```

***Construtores***

* Um default e um que recebe a capacidade da lista;

```bash
//Construtor default

public ArrayList(){
    this.list = new String[DEFAULT_CAPACITY];
    //o tamanho já é inicializado com zero
}

//Construtor com atributo

public ArrayList(int capacity){
    this.list = new String[capacity];
    //o tamanho já é inicializado com zero
}

```

***Métodos de adição***

Para os métodos de adição, precisaremos de alguns métodos auxiliares. Eles serão privados, pois sua utilidade é interna (na classe).

Ao adicionar um elemento na lista, é preciso conferir se o array já chegou a sua ocupação máxima. É preciso *assegurar*
que há espaço para mais um elemento. Se não houver, é preciso "aumentar" o espaço - criar um novo array com capacidade
maior (resize) e transferir os elementos para ele.

Além disso, para inserir um objeto em uma posição da lista que não seja a última, é preciso fazer um shift para a direita

```bash
private void ensureCapacity(intendedCapacity){
    if(this.list.length < intendedCapacity){
        resize(Math.max(this.list.length * 2, intendedCapacity));
    }
}

private void resize(newCapacity){
    String newList = new String[newCapacity];
    for(int i = 0; i < this.list.length; i++){
        newList[i] = this.list[i];
    }
    this.list = newList;
}

public void shiftToRight(int index){
    for(int i = this.size - 1; i > index; i--){
        this.list[i] = this.list[i - 1];
    }
}
```


Com esses métodos, é possível partir para os métodos de adição:

1) boolean add(String str)

* Adiciona o objeto passado como parâmetro no final da lista
* É preciso conferir se o array já está totalmente preenchido (se sim, resize)

```bash
public boolean add(String str){
    ensureCapacity(this.size + 1);
    list[this.size++] = str;
    return true;
}

```

2)void add(int index, String str)

* Adiciona, no índice 'index', o objeto passado como parâmetro
* É preciso conferir se o array já está totalmente preenchido (se sim, resize)
* É preciso conferir se o índice passado é válido
* Provavelmente será necessário fazer um shift para a direita

```bash
public void add(int index, String str){
    if(index < 0 || index >= this.size) throw new IndexOutOfBoundsException();
    ensureCapacity(this.size + 1);
    shiftToRight(index);
    this.list[index] = str;
    this.size++;
}

```

3)void set(int index, String str)

* Altera o objeto no índice 'index'
* É preciso conferir se o índice passado é válido

```bash
public void set(int index, String str){
    if(index < 0 || index >= this.size) throw new IndexOutOfBoundsException();
    this.list[index] = str;
}
```
***Custo dos métodos de adição***

Método 1: custo O(1) armotizado - o custo para adicionar n elementos na lista é O(n).
Método 2: é preciso fazer shift para a direita, que no pior caso (índice 0) tem custo O(n).
Método 3: O(1).


---


***Métodos de remoção***

É possível remover um elemento passando como parâmetro de uma função o índice ou o próprio objeto. 
Porém, para removê-lo, é preciso um shiftToLeft(), para não deixar 'buracos' na lista (posições não
utilizadas no array).

```bash
public void shiftToLeft(int index){
    for(int i = index; i < this.size - 1; i++){
        this.list[i] = this.list[i + 1];
    }
}

```

Com esse método em mãos, temos os métodos de remoção:

1)String remove(int index)

* Remove o elemento do índice passado como parâmetro
* É preciso verificar se o índice passado é válido
* É preciso fazer shift para a esquerda
* Retorna o elemento removido

```bash
public String remove(int index){
    if(index < 0 || index > this.size - 1) throw new IndexOutOfBoundsException() //ou retorna null, depende da especificação;
    String retorno = this.list[index];
    shiftToLeft(index);
    this.size--;
    return retorno;
}

```

2) boolean remove(String str)

* Remove o elemento passado como parâmetro
* Se o elemento não estiver na lista (não é possível remover) retorna falso
* Se a remoção ocorrer com sucesso, retorna true

```bash
public boolean remove(String str){
    if(str != null){
        for(int i = 0; i < this.size; i++){
            if(str.equals(this.list[i])){
                shiftToLeft(i);
                this.size--;
                return true;
            }
        }
    }
    
    return false;
}

```

***Custo dos métodos de remoção***

Método 1: no pior caso - remover o primeiro elemento, será preciso fazer shiftToLeft(0), o que tem custo O(n).
Método 2: no pior caso, o elemento não será encontrado depois da iteração por toda a lista, o que tem custo O(n).


---


***Métodos de busca***

Existem algumas funções de busca para: acessar o elemento de um determinado índice, achar o índice de um elemento
e conferir se um elemento está na lista. Esses métodos são:

1)String get(int index)

* Retorna o elemento do índice passado como parâmetro
* É preciso conferir se o índice passado é válido

```bash
public String get(int index){
    if(index < 0 || index > this.size - 1) throw new IndexOutOfBoundsException();
    return this.list[index];
}

```

2)int indexOf(String str)

* Retorna o índice do elemento passado como parâmetro ou -1, se o elemento não estiver na lista
* Se houver mais de uma ocorrência do elemento, retorna o índice da primeira

```bash
public int indexOf(String str){
    if(str != null){
        for(int i = 0; i < this.size; i++) if(str.equals(this.list[i])) return i;        
    } 
    return -1;
}

```

3)boolean contains(String str)

* Retorna true se o elemento estiver na lista e falso, caso não
* Há uma iteração sobre a lista, mesmo que através da chamada de outro método

```bash
public boolean contains(String str){
    return indexOf(str) != -1;
}

```

***Custo dos métodos de busca***

Método 1: O(1), acesso direto a uma posição do array
Método 2: O(n), pois no pior caso (o elemento não está na lista )há uma iteração sobre toda a lista
Método 3: O(n), pois usa o método dois e tem o mesmo pior caso.


## Pilha (LIFO) 

Pilhas são uma abstração de um array (nesse caso) definida por uma política de acesso restrita:

* LIFO - Last In First Out, o último elemento a entrar na pilha é o primeiro a sair

**IMPORTANTE**

Nas operações com pilha, não podemos iterar diretamente sobre o array, pois essa estrutura é uma 
abstração, então, apesar de ser, de fato, um array, não a tratamos como tal.

Por isso, tem algumas operações específicas

* addLast ou push: adiciona um elemento no topo (final) da pilha
* removeLast ou pop: remove o elemento do topo (final) da pilha
* peek: retorna o elemento do topo da pilha

***Exemplos de aplicações de pilha***
 
1) Ctrl + z
2) Validação de parênteses 
3) Execução de algoritmos recursivos

***Atributos de uma Pilha***

* Um array do tipo que se deseja armazenar (vou usar String)
* Uma variável topo 

```bash
private String[] pilha;
private int topo;

```

---

***Construtor***

* Recebe a capacidade da pilha

```bash
public Pilha(int capacidade){
    this.pilha = new String[capacidade];
    this.topo = -1; //Quando a pilha está vazia, o topo é -1
}

```

---

***Verificar se está cheia/vazia***

```bash
public boolean isEmpty(){
    return this.topo == -1;
}

public boolean isFull(){
    return this.topo = this.pilha.length - 1;
}

```

---

***Push***

* Operação de adicionar um elemento ao topo da pilha
* Se a pilha estiver lotada, seguir uma especificação sobre o que fazer

```bash
public void push(String str){
    if(isFull()) throw new RuntimeException();
    this.pilha[++this.topo] = str;
}

```

---

***Pop***

* Remove o elemento do topo da pilha
* Decrementa o topo
* Retorna o elemento removido
* Se a pilha estiver vazia, seguir uma especificação sobre o que fazer

```bash
public String pop(){
    if(isEmpty()) throw new NoSuchElementException();
    return this.pilha[this.topo--];
}

```

---

***Peek***

* Retona o elemento do topo da pilha
* Se a pilha estiver vazia, seguir uma especificação sobre o que fazer

```bash
public String peek(){
    if(isEmpty) throw new NoSuchElementException();
    return this.pilha[this.topo];
}

```

***E os outros métodos, como não iterar?***

Como não podemos itarar sobre os índices do array por trás da pilha, temos apenas as ferramentas
push, pop e peek para fazer funções como contains. Por isso, frequentemente é preciso criar uma pilha 
auxiliar, transferir os elementos para ela de acordo com o propósito do método. Então, retornar os 
elementos para a pilha inicial.

OBS: Se apenas parte dos elementos forem passados para a pilha auxiliar e depois colocados de volta na
pilha original, não ocorre nenhum problema. Porém, na estrutura fila(FIFO), isso causa uma confusão, então é 
preciso transferir todos os elementos para a fila auxiliar e depois trazê-los todos de volta à original.


***Eficiência das operações***

Push: O(1)
Pop : O(1)
Peek: O(1)
Métodos que envolvem estruturas auxiliares e não apenas a manipulação do topo requerem uma análise específica.
O contains, por exemplo, tem complexidade O(n) no pior caso.


## Fila (FIFO)

Filas são uma abstração de um array (nesse caso) definida por uma política de acesso restrita:

* FIFO - First In First Out, o primeiro elemento a entrar na fila é o primeiro a sair

**IMPORTANTE**

Nas operações com fila, também não podemos iterar diretamente sobre o array, pois essa estrutura é uma abstração

As adições **sempre** são feitas no final da fila
As remoções **sempre** são feitas no início da fila
Uma fila circular é mais eficiente (não há necessidade de fazer shift)
Nessa abordagem, se a fila estiver cheia e for preciso adicinar um elemento, removo o primeiro para adicioná-lo

***Exemplos de aplicações de fila***

1. Sistemas de atendimento por ordem de chegada
2. Memória cache 
3. Inverter uma pilha

***Atributos de uma fila***

* Um array do tipo que se deseja armazenar (vou usar String)
* Um inteiro 'head' que marca a posição do início da fila
* Um inteiro 'tail' que marca a posição do final da fila
* Um inteiro 'size' que representa o tamanho da fila

```bash
private String[] fila;
private int head;
private int tail;
private int size;
```

---

***Construtor***

* Recebe a capacidade da fila 

```bash
public Fila(int capacidade){
    this.fila = new String[capacidade];
    //Quando a fila está vazia, head e tail são -1
    this.head = -1;
    this.tail = -1;
    this.size = 0;
}
```

---

***Verificar se está cheia/vazia***

```bash
public boolean isEmpty(){
    return this.size == 0;
}

public boolean isFull(){
    return this.size == this.fila.length;
}

```

---

***addLast***

* Caso 1: a fila está vazia
* Caso 2: a fila não está vazia nem cheia
* Caso 3: a fila está cheia 

OBS: No caso 3, não basta apenas sobrescrever o primeiro elemento. Se isso for feito, o elemento que
acabou de ser adicionado (elemento 1) fica na posição head, então está na posição de mais antigo da fila. Se formos mais 
uma vez adicionar um objeto na lista, um elemento 2, ele vai sobrescrever o elemento 1, em vez de entrar no lugar do
que é, de fato, o mais antigo da fila.

```bash
public void addLast(String str){
    if(isEmpty()){
        this.head = 0;
        this.tail = 0;
        this.fila[this.tail] = str;
        this.size++;
    }

    else if(isFull()){
        this.head = (this.head + 1) % this.fila.length;
        this.tail = (this.tail + 1) % this.fila.length;
        this.fila[this.tail] = str;
    }

    else{
        this.tail = (this.tail + 1) % this.fila.length;
        this.fila[tail] = str;
        this.size++;
    }
}

```

---

***removeFirst***

* A fila pode estar vazia
* A fila pode ter só um elemento. Nesse caso, é preciso mudar o valor de head e tail

```bash
public String removeFirst(){
    if(isEmpty()) throw new NoSuchElementException();

    //O retorno da função é o elemento removido
    String elemento = this.fila[this.head];
    
    if(this.size == 1){
        this.head = -1;
        this.tail = -1;
    }else{
        this.head = (this.head + 1) % this.fila.length;
    }

    this.size--;
    return elemento;

}

```

***Eficiência das operações***

addLast: O(1)
removeFirst: O(1)


## Cache 

Tópico: abordar a aplicação de estruturas de dados na implementação do cache

### Hit rate e Miss rate

Quando se procura um elemento na memória cache, duas coisas podem acontecer

1) O elemento é encontrado (hit)
2) O elemento não é encontrado (miss) 

hit rate é uma métrica que avalia a quantidade operações 'hit' em relação ao total de vezes que o cache é acessado;
miss rate é uma métrica que avalia a quantidade de operações 'miss' em relação ao total de vezes que o cache é acessado

Quanto maior é a hit rate, melhor é a política de cache eviction
Quando maior a miss rate, pior a política de cache eviction

Mas o que é cache eviction?

### Memória cache é limitada

Cache é uma memória de rápido acesso, então, consequentemente, é uma memória cara, e é inviável que tenha uma grande capacidade.
Assim, é preciso procurar uma política que determina qual elemento é removido quando um novo precisar entrar e não houver mais
espaço na memória. Essa política é chamada de cache eviction, e existem várias dela, cada uma mais adequada a um contexto.

### Política 1: FIFO

Nessa política, ao tentar inserir um elemento, se a memória cache estiver cheia:

O novo elemento entra no lugar do mais antigo

* realizar um removeFirst() - tira o elemento mais antigo (o primeiro que entrou) da fila
* realizar um addLast() do novo elemento

Assim, a adição e remoção na memória cache, usando uma fila circular, custam O(1).
(na verdade o miss ainda custa uma ida ao banco de dados)

***Limitação***

A busca por um elemento custa O(n).

Para otimizar essa operação, é possível usar uma estrutura auxiliar que permita buscar por um 
elemento com custo O(1). Exemplos de estrutura são o HashMap e o HashSet.

Apesar do fato de que com essa mudança o uso de memória seria maior, pois todos os elementos da fila precisarão estar também
na estrutura auxiliar, ainda vale a pena, pois todas as operações - adicionar, buscar e remover do cache - passam a ser O(1);
(adiciona-se o custo de adicionar/remover da tabela ou do conjunto também, ao fazer essas operações na fila. Mas adição e 
remoção nessas duas estruturas auxiliares custam O(1), então o custo continua constante).
 

### Política 2: LRU

**LRU - Least Recently Used**

Nessa política, remove-se o elemento que acessado por último há mais tempo. 

Digamos que busquei por "a", "b" e "c" (nessa ordem) no cache e, ao não encontrá-los, houve um acesso ao banco de dados e eles foram adicionados.

Nessa implementação usando fila, sempre que um elemento é acessado, ele é movido para o fim da fila.

Se tenho os elementos:

["a", "b", "c"]

e acesso b, ele vai para o final da fila (moveToTail). Veja:

["a", "c", "b"]

Ao acessar "a", ele vai para o final da fila:

["c", "b", "a"]

Se, agora, for preciso inserir um novo elemento, o "c" será removido, pois foi o que acessei menos recentemente (por isso está no começo da fila).


### Política 3: LFU


## LinkedList

### Motivações para construção

* Evitar o mal uso de memória. Em outras estruturas baseadas em array, algumas posições ficam vazias e o espaço na memória acaba sendo desperdiçado.
  Esse mal uso pode parecer inofensivo, mas quando se trata de grandes entradas, o custo é caro e isso se torna um problema relevante.
* Sempre que for preciso aumentar a capacidade em outras estruturas como ArrayList, Pilha e Fila, além de criar outra estrutura maior, é preciso transferir 
  todos os elementos para ela, o que é uma operação custosa.

### Ideia da Lista Ligada

Cada elemento da lista é um objeto (Node) que possui um valor e referências para seu anterior (prev) e para seu posterior (next).

Quando há referências prev e next, a lista é **duplamente encadeada**. 

Há uma referência para o início da lista (head) e uma para o final (tail). A partir desses objetos, conseguimos
acessar toda a lista, indo para os objetos anteriores ou posterirores.

Além disso, é importante ter uma variável para guardar o tamanho da lista (size).

---

***Atributos de um nó***

```bash
    int value; //Poderia ser qualquer objeto
    Node prev;
    Node next;    
```

---

***Construtor de um nó***

* Quando um nó é criado, seu prev e next são nulos

```bash
public Node(int valor){
    this.value = valor;
    this.prev = null;
    this.next = null;
}

```

---

***Atributos da LinkedList***

```bash
Node head;
Node tail;
int size;

```

---

***Construtor da LinkedList***

```bash
public LinkedList(){
    this.head = null; //Começa vazia
    this.tail = null;
    this.size = 0;
}
```

---

***Conferir se a lista está vazia***

* Conferir se o head é null, se o size é zero. Dá no mesmo.

```bash
public boolean isEmpty(){
    return this.head == null;
}
```

---

### Métodos de inserção

---

***addLast***

* Recebe o objeto a ser adicionado, cria um nó e o adiciona no final da lista
* Se a lista estiver vazia, head e tail vão apontar para esse novo nó
* Sempre será preciso atualizar a referência 'tail'

```bash
public void addLast(int value){
    Node newNode = new Node(value);
    if(isEmpty){
        this.head = newNode;
        this.tail = newNode;
    }else{
        this.tail.next = newNode;
        newNode.prev = this.tail;
        this.tail = newNode;
    }
    this.size += 1;
}
```

---

***addFirst***

* Recebe o objeto a ser adicionado, cria um nó e o adiciona no final da lista
* Se a lista estiver vazia, head e tail vão apontar para esse novo nó
* Sempre será preciso atualizar a referência 'head'

```bash
public void addFirst(int value){
    Node newNode = new Node(value);
    if(isEmpty()){
        this.head = newNode;
        this.tail = newNode;
    }else{
        this.head.prev = newNode;
        newNode.next = this.head;
        this.head = newNode;
    }
    this.size += 1;
}

```

---

***Adicionar em um index***

* Se a lista estiver vazia e for passado o índice 0, não deve ser possível adicionar o elemento
(pode-se pensar que é para adicionar no começo nessa situação, mas não)

* Se for passado um índice maior que a última posição, não deve ser possível adicionar o elemento
(pode-se pensar que é para adicionar no fim nessa situação, mas não)


```bash
public void add(int index, int valor){
        if(index < 0 || index >= this.size) throw new IndexOutOfBoundsException();
        Node aux = this.head;
        int cont = 0;
        while(cont != index){
                aux = aux.next;
                cont++;
        }
        Node newNode = new Node(valor);
        if(this.head == aux && index == 0) this.head = newNode;
        if(aux.prev != null) aux.prev.next = newNode;
        newNode.prev = aux.prev;
        newNode.next = aux;
        aux.prev = newNode;
        this.size++;
    }

```

---

***Custo das inserções***

addFirst e addLast têm custo O(1), pois envolvem apenas operações primitivas, como a manipulação de referências.
Já a operação de adicionar um elemento numa posição arbritária da lista tem custo O(n), pois é preciso iterar sobre ela.

## Métodos de busca

Podemos buscar o primeiro e o último elemento da lista. Como temos as referências 'head' e 'tail', o acesso é direto 
e não é preciso iterar.

Esses métodos não retornam o nó, mas sim seu valor

---

***getFirst***

```bash
public int getFirst(){
    if(isEmpty()) throw new NoSuchElementException(); //Se fosse uma lista de Usuario, por exemplo, retornaria null
    return this.head.value;
}
```

---

***getLast***

```bash
public int getLast(){
    if(isEmpty()) throw new NoSuchElementException();
    return this.tail.value;
}
```

* OBS: uma coisa que percebi é que esses dois métodos são muito bons de usar nos testes dos demais métodos, porque 
sempre que se esquece de atualizar o valor de head ou de tail, fica fácil de saber que esse foi o erro, pois os
asserts que envolvem getFirst() e getLast() dão errado.


---

****get***

* Retorna o valor do elemento de um determinado índice

```bash
public int get(int index){
    if(index < 0 || index > this.size - 1) throw new IndexOutOfBoundsException();
    int cont = 0;
    Node aux = this.head;
    while(cont != index){
        aux = aux.next;
        cont++;
    }
    return aux.value;
}
```

---

***indexOf***

* Retorna o índice da primeira ocorrência do elemento passado como parâmetro
* Se não achar (elemento não está na lista) retorna -1;

```bash
public int indexOf(int valor){
    Node aux = this.head;
    int cont = 0;
    while(aux != null){
        if(aux.value == valor) return cont;
        cont++
        aux = aux.next;
    }
    return -1;
}
```

---

***contains***

* Retona true se o elemento estiver na lista, false caso não





















