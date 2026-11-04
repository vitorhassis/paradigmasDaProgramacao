# Aula 009 — TADs e orientação a objetos

VITOR HUGO ASSIS DA SILVA · RA 241400232 · Turma ESOFT6B  
Gerado em 04/11/2026

## 1. Para cada trecho (a) qual é a saída (b) a decisão foi tomada na compilação ou na execução

### Java — método sobrescrito

```java
class Animal {
    String som() { return ...; }
}

class Cachorro extends Animal {
    String som() { return au; }
}

Animal x = new Cachorro();
System.out.println(x.som());
```

(a) Saída `au`.

(b) Decisão execução. A referência `x` foi declarada como `Animal`, mas aponta para um objeto `Cachorro`. Como `som()` é um método de instância sobrescrito, Java executa a implementação correspondente ao objeto real `Cachorro.som()`.

### C++ — método não virtual

```cpp
struct A {
    void f() { cout  A; }
};

struct B  A {
    void f() { cout  B; }
};

B b;
A p = &b;
p-f();
```

(a) Saída `A`.

(b) Decisão compilação. `f()` não foi declarado `virtual` em `A`. Por isso, a chamada feita por `p` usa o tipo do ponteiro, `A`, e seleciona `Af()`. Se o método de `A` fosse virtual, a chamada poderia escolher `Bf()` em tempo de execução.

---

## 2. Para cada trecho (a) qual é a saída (b) a decisão foi tomada na compilação ou na execução

### Java — campo e método

```java
class A {
    String nome = A;

    String getNome() {
        return nome;
    }
}

class B extends A {
    String nome = B;

    String getNome() {
        return nome;
    }
}

A x = new B();
System.out.println(x.nome +   + x.getNome());
```

(a) Saída `A B`.

(b) Há duas decisões. `x.nome` é resolvido na compilação, de acordo com o tipo declarado de `x` (`A`), então fornece `A`. Já `x.getNome()` é resolvido na execução, pois o método foi sobrescrito e o objeto é um `B`. Dentro de `B.getNome()`, o campo `nome` é o de `B`, resultando em `B`.

### Python — atributo da classe e da instância

```python
class Contador
    total = 0

    def __init__(self)
        Contador.total += 1
        self.id = Contador.total

a = Contador()
b = Contador()
print(a.id, b.id, a.total)
```

(a) Saída `1 2 2`.

(b) Busca de atributos execução. Quando `a` é criado, `Contador.total` passa a 1 e `a.id` recebe 1. A criação de `b` aumenta o total compartilhado para 2 e define `b.id` como 2. Como `a` não tem um atributo próprio chamado `total`, a busca encontra o atributo da classe, que nesse momento vale 2. Este exemplo trata de busca de atributos, não de despacho polimórfico.

---

## 3. Para cada trecho (a) qual é a saída (b) a decisão foi tomada na compilação ou na execução

### Java — método estático

```java
class A {
    static String quem() {
        return A;
    }
}

class B extends A {
    static String quem() {
        return B;
    }
}

A x = new B();
System.out.println(x.quem());
```

(a) Saída `A`.

(b) Decisão compilação. `quem()` é estático o método em `B` oculta o de `A`, sem sobrescrevê-lo. Como `x` está declarado como `A`, a chamada seleciona `A.quem()`. Escrever `A.quem()` diretamente deixaria essa escolha mais clara.

### Go — composição e método promovido

```go
type Animal struct{}

func (Animal) Som() string {
    return ...
}

func (a Animal) Falar() string {
    return faz  + a.Som()
}

type Cao struct {
    Animal
}

func (Cao) Som() string {
    return au
}

fmt.Println(Cao{}.Falar(), Cao{}.Som())
```

(a) Saída `faz ... au`.

(b) Decisão neste trecho compilação. `Cao` incorpora `Animal` e pode chamar o método promovido `Falar()`. Dentro dele, o receptor `a` tem tipo concreto `Animal`; portanto, `a.Som()` chama `Animal.Som()` e forma `faz ...`. A expressão separada `Cao{}.Som()` seleciona `Cao.Som()` e devolve `au`. Incorporar `Animal` não faz a chamada interna de `Falar()` ser redirecionada ao método de `Cao`.

---

## Fontes técnicas

- [Java Language Specification, capítulo 8](httpsdocs.oracle.comjavasespecsjlsse26htmljls-8.html) — sobrescrita e ocultação de métodos e campos.
- [C++ Working Draft, virtual functions](httpseel.isc++draftclass.virtual) — métodos virtuais.
- [Python Tutorial, Classes](httpsdocs.python.org3tutorialclasses.html) — atributos de classe e de instância.
- [The Go Programming Language Specification](httpsgo.devrefspec) — métodos promovidos e seleção de métodos.
