# Aula 008 — Subprogramas

**VITOR HUGO ASSIS DA SILVA** · **RA 241400232** · **Turma ESOFT6B**  
Gerado em 04/11/2026

## 1. Python — argumento padrão mutável

```python
def adicionar(item, lista=[]):
    lista.append(item)
    return lista

print(adicionar(1))
print(adicionar(2))
```

**Saída:**

```text
[1]
[1, 2]
```

**Explicação:** A lista usada como valor padrão é criada quando a função é definida, não a cada chamada. Como a primeira chamada modifica essa mesma lista, a segunda encontra o `1` já inserido e acrescenta o `2`. Para obter uma lista nova por chamada, pode-se usar `lista=None` e criar a lista dentro da função.

## 2. Java — passagem de argumentos

```java
static void zera(int[] v, int n) {
    v[0] = 0;
    n = 0;
}

int[] v = {5, 5};
int n = 5;
zera(v, n);
System.out.println(v[0] + " " + n);
```

**Saída:** `0 5`.

**Explicação:** Java passa os argumentos por valor. No caso de `v`, o valor copiado é a referência ao array; a função e o chamador continuam apontando para o mesmo array, então `v[0] = 0` é visível fora dela. Já `n` é um inteiro copiado para o parâmetro local. A atribuição `n = 0` muda apenas essa cópia, e o `n` do chamador permanece 5.

## 3. Python — fechamento e ligação tardia

```python
fs = [lambda: i for i in range(3)]
print([f() for f in fs])
```

**Saída:** `[2, 2, 2]`.

**Explicação:** As três funções guardam uma referência à mesma variável `i`. Quando elas finalmente são chamadas, o laço terminou e `i` vale 2. Se a intenção fosse guardar o valor de cada iteração, uma forma seria `lambda i=i: i`, que produziria `[0, 1, 2]`.

## 4. C — variável local `static`

```c
int contador(void) {
    static int n = 0;
    return ++n;
}

/* em main */
contador();
contador();
printf("%d\n", contador());
```

**Saída:** `3`.

**Explicação:** Apesar de estar declarada dentro da função, `n` tem duração de armazenamento estática. Ela é inicializada uma vez e conserva o valor entre chamadas: a primeira devolve 1, a segunda devolve 2 e a terceira, que é impressa, devolve 3.

## 5. Rust — posse do valor

```rust
fn dobra(v: Vec<i32>) -> Vec<i32> {
    v.iter().map(|x| x * 2).collect()
}

let v = vec![1, 2, 3];
let d = dobra(v);
println!("{:?} {:?}", v, d);
```

**Resultado:** erro de compilação `E0382` (`borrow of moved value: v`); portanto, o programa não imprime os vetores.

**Explicação:** O vetor não implementa `Copy`. Ao chamar `dobra(v)`, a posse de `v` passa para a função. Depois dessa chamada, a variável original não pode ser usada no `println!`. Para manter `v`, seria possível passar uma referência e mudar a assinatura da função, ou chamar `dobra(v.clone())` quando uma cópia independente for desejada.

## 6. Python — escopo local

```python
total = 0

def adiciona(x):
    total = total + x
    return total

print(adiciona(5))
```

**Resultado:** `UnboundLocalError`; o código não imprime `5`.

**Explicação:** Como há uma atribuição a `total` dentro da função, Python considera esse nome local em todo o corpo de `adiciona`. Na expressão à direita, `total` é lido antes de receber um valor local, causando o erro. Uma solução simples é receber o total como parâmetro e retornar a soma. Também seria possível declarar `global total`, se a intenção for alterar a variável global.

## Fontes técnicas

- [Python — valores padrão de argumentos](https://docs.python.org/3/tutorial/controlflow.html#default-argument-values).
- [Python — perguntas frequentes sobre funções em laços e escopo](https://docs.python.org/3/faq/programming.html).
- [Java Language Specification — parâmetros de métodos](https://docs.oracle.com/javase/specs/jls/se26/html/jls-8.html).
- [ISO/IEC 9899:2024 — duração de armazenamento em C, seção 6.2.4](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf).
- [Rust — explicação do erro E0382](https://doc.rust-lang.org/stable/error_codes/E0382.html).
