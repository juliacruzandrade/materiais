## ArrayList

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

1) boolean add(String str)

* Adiciona o objeto passado como parâmetro no final da lista
* É preciso conferir se o array já está totalmente preenchido (se sim, resize)

```bash
```



2)void add(int index, String str)

* Adiciona, no índice 'index', o objeto passado como parâmetro
* É preciso conferir se o array já está totalmente preenchido (se sim, resize)
* É preciso conferir se o índice passado é válido
* Provavelmente será necessário fazer um shift para a direita

```bash
```


3)void set(int index, String str)

* Altera o objeto no índice 'index'
* É preciso conferir se o índice passado é válido

```bash
```




