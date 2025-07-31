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




