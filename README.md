# java10-2022b-AlejandroBrites
java10-2022b-AlejandroBrites created by GitHub Classroom


Existem duas possibilidades de perdas de dados no conta bancaria

O Thread Deposita faz o seginte processo:
```
1- 1A° Thread Deposita carrega o valor do saldo da memoria.
2- 2A° Thread Deposita executa a soma do Saldo com o valor depositado.
3- 3A° Thread Deposita atribui o resultado dessa soma ao atributo saldo. 
```
O Thread Retira faz o seginte processo:
```
1- 1B° Thread Retira carrega o valor do saldo da memoria.
2- 2B° Thread Retira executa a subtração do Saldo com o valor retirado.
3- 3B° Thread Retira atribui o resultado dessa subtração ao atributo saldo. 
```
Existem algumas situações na ordem que essas ações podem ocorrer que fazem resultados erroneos surgirem tanto para mais tanto para menos.


Caso o a primeira ação de cada Thread ocorra uma seguida da outra ambos irão carregar o mesmo valor de saldo, não o valor após o resultado de um deposito ou ou retirada e isso gera alguns problemas.

1- 1A° Thread Deposita carrega o valor do saldo da memoria.
2- 1B° Thread Retira carrega o valor do saldo da memoria.

ou

1- 1B° Thread Retira carrega o valor do saldo da memoria.
2- 1A° Thread Deposita carrega o valor do saldo da memoria.

Caso as situções acima descritas ocorram duas situações podem acontrecer

1° Situação

A primeira situação que pode ocorrer é o Thread Retira atribuir o resultado da subtração ao atributo saldo, e após isso Thread Deposita atribui o resultado da adiçao ao atributo saldo.

5- 3B° Thread Retira atribui o resultado dessa subtração ao atributo saldo. (Intrução A)
6- 3A° Thread Deposita atribui o resultado dessa soma ao atributo saldo.    (Intrução B)   

Com essa situação acontecendo a "intrução A" não tera efeito pois a conta executada pelo Thread Deposita calculou o novo valor do atributo saldo utilizando um valor desatualiza do atributo saldo, fazendo com uma ação de subtração seja ignorada fazendo com que o saldo final tenha um valor acima do esperado.    

2° Situação

A segunda situação que pode ocorrer é o Thread Deposita atribuir o resultado da adição ao atributo saldo, e após isso Thread Retira atribui o resultado da subtração ao atributo saldo.

5- 3A° Thread Deposita atribui o resultado dessa subtração ao atributo saldo. (Intrução A)
6- 3B° Thread Retira atribui o resultado dessa soma ao atributo saldo.    (Intrução B)  

Com essa situação acontecendo novamente a "intrução A" não tera efeito pois a conta executada pelo Thread Retira calculou o novo valor do atributo saldo utilizando um valor desatualiza do atributo saldo, fazendo com uma ação de adição seja ignorada fazendo com que o saldo final tenha um valor abaixo do esperado.  


Já que o código ContaBancareas executa 10 somas e 5 subtrações as situações acima descrita podem acontecer diversas vezes durante a execução fazendo com que o resultado se torne incorreto.

Exemplo de uma adição sendo ignorada:

saldo inicial = 100

1- 1A° Thread Deposita carrega o valor do saldo da memoria.                  (saldoA = 100)
2- 1B° Thread Retira carrega o valor do saldo da memoria.                    (saldoB = 100)    
3- 2A° Thread Deposita executa a soma do Saldo com o valor depositado.       (resultadoA = 100 + 100 = 200)_  
4- 2B° Thread Retira executa a subtração do Saldo com o valor retirado.      (resultadoB = 100 - 50 = 50)_     
5- 3A° Thread Deposita atribui o resultado dessa soma ao atributo saldo.     (saldoA = resultadoA = 200) 
6- 3B° Thread Retira atribui o resultado dessa subtração ao atributo saldo.  (saldoB = resultadoB = 50) 


saldo final = 50

Oque deveria acontecer 

 100 + 100 - 50 = 150


Exemplo de uma subtração sendo ignorada:

saldo inicial = 100


1- 1A° Thread Deposita carrega o valor do saldo da memoria.                  (saldoA = 100)
2- 1B° Thread Retira carrega o valor do saldo da memoria.                    (saldoB = 100)    
3- 2A° Thread Deposita executa a soma do Saldo com o valor depositado.       (resultadoA = 100 + 100 = 200)_  
4- 2B° Thread Retira executa a subtração do Saldo com o valor retirado.      (resultadoB = 100 - 50 = 50)_     
5- 3B° Thread Retira atribui o resultado dessa subtração ao atributo saldo.  (saldoB = resultadoB = 50) 
6- 3A° Thread Deposita atribui o resultado dessa soma ao atributo saldo.     (saldoA = resultadoA = 200) 

saldo final = 200

Oque deveria acontecer 

 100 + 100 - 50 = 150
 
Se os eventos acima simulados ocorrerem multiplas vezes durante a execução o saldo final pode ter um valor acima do valor esperado ou abaixo dependendo de quantas adições e subtrações forem ignoradas.
