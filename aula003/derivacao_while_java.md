# Derivação sintática: while em Java

Linguagem escolhida: Java.

Fonte consultada: Java Language Specification (JLS), Java SE 26 Edition, seção 14.12 - The while Statement. A gramática foi reduzida apenas para mostrar a estrutura do comando.

## Código que será derivado

```java
public class ExemploWhile {
    public static void main(String[] args) {
        int contador = 1;
        while (contador <= 3) {
            System.out.println(contador);
            contador++;
        }
    }
}
```

A derivação será feita somente para este trecho:

```java
while (contador <= 3) {
    System.out.println(contador);
    contador++;
}
```

## Regras de produção

Os símbolos entre `< >` são não terminais. Os outros símbolos são terminais da linguagem.

```text
<while_statement> ::= while ( <condition> ) <statement>
<condition> ::= <identifier> <= <integer_literal>
<statement> ::= <block>
<block> ::= { <statement_list> }
<statement_list> ::= <print_statement> <increment_statement>
<print_statement> ::= System.out.println ( <identifier> ) ;
<increment_statement> ::= <identifier> ++ ;
<identifier> ::= contador
<integer_literal> ::= 3
```

## Derivação

```text
<while_statement>
=> while ( <condition> ) <statement>
=> while ( <identifier> <= <integer_literal> ) <statement>
=> while ( contador <= <integer_literal> ) <statement>
=> while ( contador <= 3 ) <statement>
=> while ( contador <= 3 ) <block>
=> while ( contador <= 3 ) { <statement_list> }
=> while ( contador <= 3 ) { <print_statement> <increment_statement> }
=> while ( contador <= 3 ) { System.out.println ( <identifier> ) ; <increment_statement> }
=> while ( contador <= 3 ) { System.out.println ( contador ) ; <identifier> ++ ; }
=> while ( contador <= 3 ) { System.out.println ( contador ) ; contador ++ ; }
```

## Forma concreta em Java

```java
while (contador <= 3) {
    System.out.println(contador);
    contador++;
}
```

## Terminais e não terminais

Terminais: `while`, `(`, `)`, `contador`, `<=`, `3`, `{`, `}`, `System.out.println`, `;` e `++`.

Não terminais: `<while_statement>`, `<condition>`, `<statement>`, `<block>`, `<statement_list>`, `<print_statement>`, `<increment_statement>`, `<identifier>` e `<integer_literal>`.

O comando `while` executa o bloco enquanto a condição for verdadeira. Neste caso, o contador começa em 1, é mostrado na tela e aumenta uma unidade por vez. Quando chega a 4, a condição deixa de ser verdadeira e o laço termina.
