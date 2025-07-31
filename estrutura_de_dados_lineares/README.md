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


##Pilha (LIFO)



