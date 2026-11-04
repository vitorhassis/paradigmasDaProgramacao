# Aula 06 — Tipos de dados: preveja antes de executar

**Aluno:** VITOR HUGO ASSIS DA SILVA  
**RA:** 241400232  
**Turma:** ESOFT6B  
**Data:** 04/11/2026

Em cada trecho, a questão é prever a saída e identificar se o problema aparece na compilação, durante a execução ou se pode passar sem ser detectado automaticamente.

## 1. JavaScript — precisão numérica

```javascript
console.log(0.2 * 3 === 0.6);
console.log(Number.MAX_SAFE_INTEGER + 4);
```

**Saída:**

```text
false
9007199254740996
```

O primeiro resultado vem da representação aproximada de números decimais em ponto flutuante: `0.2 * 3` não é exatamente igual ao valor representado por `0.6`. A segunda conta ultrapassa o limite de inteiros seguros do tipo `Number`, e o resultado é arredondado. **Detecção:** não há erro automático na compilação nem na execução.

## 2. Python — caracteres e bytes

```python
palavra = "pão"
print(len(palavra), len(palavra.encode("utf-8")))
```

**Saída:**

```text
3 4
```

`len(palavra)` conta três caracteres. Na codificação UTF-8, `ã` ocupa dois bytes; os outros dois caracteres ocupam um byte cada. **Detecção:** não há erro; as duas medidas respondem a perguntas diferentes.

## 3. Go — limite de um byte

```go
var b byte = 250
b += 10
fmt.Println(b)
```

**Saída:**

```text
4
```

`byte` é um inteiro sem sinal de 8 bits. O valor da variável passa de 255 e volta ao início da faixa, como numa conta módulo 256: `260 % 256 = 4`. **Detecção:** essa operação com variável compila e executa; não gera erro de overflow. Já atribuir diretamente uma constante fora da faixa a `byte` seria rejeitado na compilação.

## 4. Java — acesso ao array

```java
int[] valores = new int[4];
System.out.println(valores[2]);
System.out.println(valores[4]);
```

**Resultado:** a primeira linha imprime `0`, valor inicial de um elemento de `int[]`. A segunda lança `ArrayIndexOutOfBoundsException`, pois os índices válidos são `0` a `3`. **Detecção:** o acesso fora dos limites falha durante a execução.

## 5. Rust — propriedade de uma `String`

```rust
let mensagem = String::from("até logo");
let outra = mensagem;
println!("{}", mensagem);
```

**Resultado:** não há saída. A atribuição transfere a propriedade da `String` para `outra`; depois disso, `mensagem` não pode mais ser usada. **Detecção:** erro de compilação. Para conservar os dois valores, seria necessário criar uma cópia com `clone()` ou usar um empréstimo apropriado.

## 6. C — interpretação dos bits de uma `union`

```c
union {
    int inteiro;
    float real;
} dado;

dado.real = 2.5f;
printf("%d\n", dado.inteiro);
```

**Resultado:** não há um número decimal portátil para prever. A leitura de `inteiro` interpreta a representação armazenada para `real` como um `int`; o resultado depende das representações usadas pela implementação. **Detecção:** o programa pode compilar e executar sem apontar esse problema. Se a intenção for obter o número inteiro `2`, deve-se fazer uma conversão numérica explícita, como `(int)dado.real`.

## Conclusão

Os seis trechos mostram falhas e diferenças de tipos em momentos distintos: algumas são barradas pelo compilador, outras só aparecem quando a instrução é executada, e outras produzem uma saída válida para a linguagem, mas diferente da intenção de quem programou. Para prever o comportamento, é preciso observar a faixa, a representação e as regras de cada tipo.
