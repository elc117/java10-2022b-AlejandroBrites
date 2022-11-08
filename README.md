# java10-2022b-AlejandroBrites
java10-2022b-AlejandroBrites created by GitHub Classroom


Analisando o código "OperacoesBancareas.java" estudado em aula é possivel afirmar que os threads do código executam duas operações.

O Thread Deposita execututa os seguites passos:

```
1- (1A) Thread Deposita carrega o valor do saldo da memória.
2- (2A) Thread Deposita executa a adição do saldo carregado com o valor depositado.
3- (3A) Thread Deposita atribui o resultado dessa adição ao atributo saldo. 
```

O Thread Retira executa os seguintes passos:

```
1- (1B) Thread Retira carrega o valor do saldo da memória.
2- (2B) Thread Retira executa a subtração entre o saldo carregado e o valor retirado.
3- (3B) Thread Retira atribui o resultado dessa subtração ao atributo saldo. 
```

Existem algumas situações, nas quais a ordem em que essas ações ocorrem acabam gerando resultados equivocados, gerado pela maneira que as Threads são executadas.

Essas situações de erro na execução desse código ocorrem quando as Threads carregam o valor do saldo uma seguida da outra. Com isso a segunda acaba carregando um valor desatualizado do saldo que não leva em consideração a operação da Thread que ainda não foi concluida.


```
1- (1A) Thread Deposita carrega o valor do saldo da memória.
2- (1B) Thread Retira carrega o valor do saldo da memória.
```

ou

```
1- (1B) Thread Retira carrega o valor do saldo da memória.
2- (1A) Thread Deposita carrega o valor do saldo da memória.
```

Caso as situções acima descritas ocorram duas situações podem acontrecer

1° Situação

A primeira situação que pode ocorrer, durante a conclusão das operações, é o Thread Retira atribuir o resultado da subtração ao atributo saldo, e após isso Thread Deposita atribuir o resultado da adição ao atributo saldo.

```
5- (3B) Thread Retira atribui o resultado dessa subtração ao atributo saldo. (Intrução B)
6- (3A) Thread Deposita atribui o resultado dessa adição ao atributo saldo.  (Intrução A)   
```

Com essa situação acontecendo a "intrução B" não tera efeito pois a conta executada pelo Thread Deposita calculou o novo valor do atributo saldo utilizando um valor desatualiza do atributo saldo, que não levava em conta a Instrução A, fazendo com uma ação de subtração seja ignorada fazendo com que o saldo final tenha um valor acima do esperado.    

2° Situação

A segunda situação que pode ocorrer é o Thread Deposita atribuir o resultado da adição ao atributo saldo, e após isso Thread Retira atribui o resultado da subtração ao atributo saldo.

```
5- (3A) Thread Deposita atribui o resultado dessa subtração ao atributo saldo. (Intrução A)
6- (3B) Thread Retira atribui o resultado dessa adição ao atributo saldo.      (Intrução B)  
```

Com essa situação acontecendo a "instrução A" não tera efeito pois a conta executada pelo Thread Retira calculou o novo valor do atributo saldo utilizando um valor desatualiza do atributo saldo, fazendo com uma ação de adição não tenha relevancia, fazendo com que o saldo final tenha um valor abaixo do esperado.  

Já que o código "OperacoesBancareas.java" executa 10 somas e 5 subtrações as situações acima descrita podem acontecer diversas vezes durante a execução fazendo com que o resultado se torne incorreto, com várias somas e subtrações podendo perder seu efeito dependendo do modo de execuação. 

Exemplo de uma adição sendo ignorada:

saldo inicial = 100

```
1- (1A) Thread Deposita carrega o valor do saldo da memoria.                  (saldoA = 100)
2- (1B) Thread Retira carrega o valor do saldo da memoria.                    (saldoB = 100)    
3- (2A) Thread Deposita executa a soma do Saldo com o valor depositado.       (resultadoA = 100 + 100 = 200)_  
4- (2B) Thread Retira executa a subtração do Saldo com o valor retirado.      (resultadoB = 100 - 50 = 50)_     
5- (3A) Thread Deposita atribui o resultado dessa soma ao atributo saldo.     (saldoA = resultadoA = 200) 
6- (3B) Thread Retira atribui o resultado dessa subtração ao atributo saldo.  (saldoB = resultadoB = 50) 
```

saldo final = 50

Oque deveria acontecer: 

100 + 100 - 50 = 150


Exemplo de uma subtração sendo ignorada:

saldo inicial = 100

```
1- (1A) Thread Deposita carrega o valor do saldo da memoria.                  (saldoA = 100)
2- (1B) Thread Retira carrega o valor do saldo da memoria.                    (saldoB = 100)    
3- (2A) Thread Deposita executa a soma do Saldo com o valor depositado.       (resultadoA = 100 + 100 = 200)_  
4- (2B) Thread Retira executa a subtração do Saldo com o valor retirado.      (resultadoB = 100 - 50 = 50)_     
5- (3B) Thread Retira atribui o resultado dessa subtração ao atributo saldo.  (saldoB = resultadoB = 50) 
6- (3A) Thread Deposita atribui o resultado dessa soma ao atributo saldo.     (saldoA = resultadoA = 200) 
```

saldo final = 200

Oque deveria acontecer:

100 + 100 - 50 = 150
 
Se os eventos acima simulados ocorrerem multiplas vezes durante a execução do código o saldo final pode ter um valor acima do valor esperado ou abaixo dependendo de quantas adições e subtrações forem ignoradas e esses valores irão variar de acordo com a ordem em que os Thread são executados e quais as operações perdem a sua relevância.

Utilizando o recurso "synchronized" nos métodos de "deposita" e "retira" da classe "Conta" irá evitar que esses erros aconteçam, pois utilizando o "synchronized" as Threads serão executadas de forma que as nehhuma Thread tenha seu efeito ignorado.
