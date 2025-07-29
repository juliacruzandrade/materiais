# 🚀 Aumente a produtividade do seu ambiente Linux

*Bons programadores dominam o terminal*

## 📑 Sumário

1. [Objetivo](#-objetivo)
2. [Preparação: Instalando o Linux](#-preparação-instalando-o-linux)
3. [Terminal e Comandos Básicos](#%EF%B8%8F-terminal-e-comandos-básicos)
4. [Vim Sem Medo](#%EF%B8%8F-vim-sem-medo)
5. [Exercícios Propostos](#-exercícios-propostos)
6. [O que Explorar Além](#-o-que-explorar-além)
7. [Materiais Para Consulta](#-materiais-para-consulta)


## 📌 Introdução

Este repositório reúne materiais para estudantes de Computação que desejam melhorar sua produtividade utilizando o terminal Linux, com foco em:

- Manipulação de arquivos e diretórios
- Edição de texto com o editor VIM


## 🎯 Objetivo
Capacitar alunos a dominar o uso do Linux como ambiente de desenvolvimento produtivo, com foco em terminal, edição de texto, automação e boas práticas. Ao final, o aluno terá autonomia para usar o Linux no dia a dia, editar arquivos com Vim, e explorar cursos mais avançados com o *The Missing Semester*.

### 🖥️ Por que usar o terminal?

No mundo da ciência da computação, saber usar o terminal não é uma habilidade extra, mas sim uma parte essencial
da formação de um bom programador. Embora IDEs modernas como VSCode, Eclipse e IntelliJ ofereçam interfaces gráficas 
amigáveis, depender delas é uma limitação, especialmente em ambientes mais exigentes como servidores e contribuições
em projetos open source.

Saber manipular aquivos e diretórios no terminal oferece muita agilidade e, além disso, o terminal pode ser o único 
meio disponível em muitos cenários reais, como em servidores remotos acessados por SSH (para acessar o ambiente dos LCCs 
em casa, por exemplo). Saber navegar e modificar diretórios e arquivos sem depender de uma IDE é indispensável. 

Outro ponto crítico é o uso do GitHub e do Git via terminal. Bons programadores não apenas sabem versionar códigos com 
os principais comandos - git add, git commit, git push, git pull , git rebase - mas entendem o que está acontecendo por 
trás dos botões das IDEs, o que permite controle sobre o histórico de um projeto e evita erros graves. 

Pensando nisso, esse material funciona como um guia que permite aos estudantes de Ciência da Computação melhorar
a produtividade não apenas do ambiente linux, mas também do VIM - editor de textos.


## 📍 Preparação: Instalando o Linux

### 🔍 O que é Linux e por que usar?
- Linux é um **kernel de código aberto**, base de várias distribuições (distros) como Ubuntu, Debian, Fedora, Arch, etc.
- O **kernel** faz a ponte entre hardware e software — como o motor de um carro.
- O criador do Linux é **Linus Torvalds** — veja o código: [github.com/torvalds/linux](https://github.com/torvalds/linux)
- É voltado para programadores, altamente personalizável, estável, seguro, e com vasta comunidade.
- Inclui ferramentas GNU, suporte a projetos open-source e ampla compatibilidade com ferramentas de desenvolvimento.

### 🧠 Entendendo suas opções: Dual Boot vs WSL2

#### ✅ Máquina Virtual (VM) – WSL2
- Executa ambiente GNU/Linux no Windows, com linha de comando, utilitários e apps Linux.
- Compatibilidade total com chamadas de sistema.

**Limitações:**
- Performance inferior em disco
- Algumas system calls ainda apresentam restrições

**Links úteis:**
- [Como instalar o WSL2 no Windows 10 e 11](https://www.oficinadanet.com.br/windows/40885-instalar-wsl2-windows-10-windows-11)
- [Comandos básicos do WSL](https://learn.microsoft.com/pt-br/windows/wsl/basic-commands?source=recommendations)

#### ✅ Dual Boot
**Vantagens:**
- Desempenho total
- Uso real do Linux com todos os recursos nativos

**Desvantagens:**
- Requer particionar disco
- Pode afetar o sistema principal se mal configurado
- Exige reiniciar para trocar de SO

### 🔧 Recomendação do curso
Use o **Linux Mint** via WSL2 ou VM. Ele é leve, estável, usado nos LCCs e ideal para iniciantes.

### 🌿 Por que escolhemos o Linux Mint?
- Utilizado nos **LCCs**
- Leve, simples, pronto para uso
- Baseado em **Debian/Ubuntu**, com `apt` e suporte a Flatpaks
- Interface amigável, boa documentação e comunidade ativa
- Vem com Codecs e Flash por padrão
- Visual personalizável (temas, widgets)
- Seguro, com updates automáticos e sem necessidade de antivírus

### 📌 Outras distros populares
- **Ubuntu** – base sólida, documentado, amigável
- **Pop!_OS** – focado em desenvolvedores e criadores
- **Fedora** – cutting edge, estável
- **Arch Linux** – máximo controle (avançado)
- **Debian** – ideal para servidores e produção estável

### 🛠️ Tópicos técnicos incluídos nesta etapa
- Diferenças entre Dual Boot, VM e WSL
- Criação de pendrive bootável (Ventoy, balenaEtcher)
- Instalação segura: dual boot ou VM (VirtualBox/WSL2)
- Escolha de distro (recomendado: Linux Mint)

### 🔗 Recursos úteis
- [Guia oficial do Linux Mint](https://linuxmint-installation-guide.readthedocs.io/)
- [Instalar WSL2](https://www.oficinadanet.com.br/windows/40885-instalar-wsl2-windows-10-windows-11)

---

## ⚙️ Terminal e Comandos Básicos

### 🐚 O que é Shell?
O **Shell** é o software que faz a comunicação entre o usuário e o sistema operacional (SO), permitindo o acesso aos recursos do SO por meio de comandos no terminal.

### 🌀 O que é Bash?
**Bash** é uma versão de shell, especificamente um **CLI Shell**, amplamente utilizada em máquinas Linux e macOS. Além de ser uma interface de linha de comando, o Bash também é uma **linguagem de programação**, capaz de criar scripts poderosos para automação de tarefas.

### 💻 O que é CLI Shell?
Shells do tipo **CLI (Command Line Interface)** são aqueles que utilizam a linha de comando como interface. Um exemplo é o Bash, que usa o terminal como meio principal para executar comandos e controlar o sistema.

### 🛠️ Programando com o Bash
Você pode criar scripts `.sh` com Bash, automatizando tarefas e organizando rotinas no sistema.

---

### 📂 Comandos importantes

#### 🔎 Navegação e orientação
```bash
$ whoami         # mostra o nome do usuário atual
$ pwd            # mostra o diretório atual
$ ls             # lista arquivos e diretórios
$ ls -a          # mostra arquivos ocultos (os que começam com .)
$ ls -l          # lista detalhada (permissões, dono, tamanho, etc)
$ clear          # limpa o terminal (Ctrl + L também funciona)
$ cd             # muda de diretório
```

#### 🧱 Manipulação de arquivos e diretórios
```bash
$ touch arquivo.txt       # cria um arquivo vazio
$ mkdir nova_pasta        # cria um diretório
$ cat arquivo.txt         # exibe conteúdo de um arquivo
$ less arquivo.txt        # paginador para leitura de arquivos longos
$ rm arquivo.txt          # remove arquivos
$ rmdir pasta_vazia       # remove diretórios vazios
$ mv origem destino       # move ou renomeia arquivos
$ cp origem destino       # copia arquivos ou diretórios (-r para recursivo)
```

🧠 Para explorar os comandos e suas opções, use:
```bash
man nome_do_comando
```

---

### 🧪 Mãos no teclado!

**Exercício 1 – Básico:**
1. Crie uma pasta chamada `primeiro_teste` dentro da sua home (`mkdir ~/primeiro_teste`).
2. Acesse essa pasta com `cd ~/primeiro_teste`.
3. Crie um arquivo vazio chamado `hello.txt` com o comando `touch hello.txt`.
4. Liste o conteúdo da pasta com `ls` e `ls -l`.

**Exercício 2 – Explorando arquivos ocultos e remoção:**
1. Crie um arquivo oculto chamado `.segredo.txt` com `touch .segredo.txt`.
2. Use `ls -a` para visualizar todos os arquivos, incluindo os ocultos.
3. Remova o arquivo `hello.txt` com `rm hello.txt`.

---

## ✍️ Vim Sem Medo

**Vim** é um editor de texto em linha de comando, rápido, leve e altamente personalizável. É usado amplamente por desenvolvedores que querem produtividade e controle total pelo teclado.

### Por que aprender Vim?
- **Movimentações eficientes** com o teclado (vim motions).
- **Modo de edição modal**, separando navegação, edição, comandos e seleção.
- **Foco total no terminal**, útil em ambientes remotos ou servidores.
- **Alta personalização** com plugins e configurações no `vimrc`.
- **Presente em praticamente todos os sistemas UNIX/Linux**.

### Modos principais do Vim:
- `Normal`: navegação e edição estrutural.
- `Insert`: inserção de texto.
- `Visual`: seleção.
- `Command-line`: salvar, sair, buscar etc.

### Comandos básicos para começar:
```bash
ESC            # volta ao modo normal
i              # modo insert
o / O          # nova linha abaixo/acima (modo insert)
:w             # salva
:q             # sai
:q!            # força saída sem salvar
:wq ou :x      # salva e sai
```

### Movimentos úteis (modo normal):
```bash
h j k l        # esquerda, baixo, cima, direita
w / b / e      # começo, volta, fim de palavra
0 / $          # início / fim da linha
gg / G         # topo / final do arquivo
```

### Edição rápida:
```bash
d{movimento}   # deleta
c{movimento}   # muda texto
y / p          # copia / cola
u / Ctrl+r     # desfaz / refaz
```

### Para praticar:
```bash
vimtutor pt.utf-8
```
Siga o tutorial oficial dentro do terminal.

### Referência complementar (com repositório):
[Minicurso Vim por Isaac Vicente](https://github.com/isaacvicente/minicurso_vim)

---

## 🧾 Exercícios propostos

Após revisar os tópicos acima, pratique os seguintes exercícios. Eles são simples, mas ajudam a reforçar os comandos mais usados do curso:

1. **Histórico e alias**
   - Crie aliases para 2 ou 3 comandos que você usa com frequência usando `alias nome='comando'`.
   - Liste os últimos 10 comandos executados com `history | tail -n 10`.

2. **Busca e filtragem**
   - Crie um arquivo `.txt` com algumas palavras e use `grep` para encontrar uma delas.
   - Use `find . -name '*.txt'` para localizar arquivos de texto no diretório atual.

3. **Redirecionamento e pipes**
   - Use `ls -l > lista.txt` para salvar a listagem em um arquivo.
   - Use `cat lista.txt | grep txt` para filtrar arquivos com `.txt`.

4. **Crontab**
   - Crie uma entrada no `crontab -e` que escreva a data atual em um arquivo a cada minuto:
     ```bash
     * * * * * date >> ~/cronlog.txt
     ```

5. **Shell script básico**
   - Escreva um script `organiza.sh` que mova arquivos `.txt` para uma pasta `textos/` e `.sh` para `scripts/`.

> Dica: documente seus comandos e scripts em um arquivo `.md` na pasta de exercícios. Isso ajuda a revisar e compartilhar depois.

---

## 📚 O que Explorar Além

Se você chegou até aqui, parabéns. A partir de agora, o aprendizado se torna cada vez mais pessoal. Aqui estão alguns caminhos para seguir:

### ✨ The Missing Semester (MIT)
O [The Missing Semester of Your CS Education](https://missing.csail.mit.edu/) é um curso do MIT voltado para ferramentas essenciais que muitos cursos de computação ignoram: terminal, shell, automação, versionamento e produtividade real no ambiente Linux. 

Explore aos poucos, revise os tópicos, e tente adaptar os exemplos ao seu próprio uso.

> Continue evoluindo. Aprenda uma coisa nova por semana. Compartilhe com os colegas. O terminal é seu playground.

### Configuração do Vim

🔗 [Acesse configuraçoes para o tmux/vim](https://github.com/rafaelserey/tmux-vim-p1)

---

## 📚 Materiais Para Consulta

🔗 [Acesse os slides no Google Apresentações](https://docs.google.com/presentation/d/1Y3FBVqCnUjD428_60088UYpdKhM0iJJcip0w8h0O8l0/edit)
🔗 [The Missing Semester of Your CS Education](https://missing.csail.mit.edu/)
🔗 [Vim Adventures](https://vim-adventures.com/)
🔗 [ExplainShell](https://explainshell.com/)
🔗 [Cheat.sh](https://cheat.sh/)
🔗 Livro: *The Linux Command Line*, William Shotts – disponível [aqui](http://linuxcommand.org/tlcl.php)




