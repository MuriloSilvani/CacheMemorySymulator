# CacheMemorySymulator
Simulador de memória cache

Participantes por grupo: 3

Politica de Mapeamento Direto

//Politica de escrita
1. Write - Through
2. Write - Back


//Implementação
 Pode ser implementado em qualquer linguagem, e deve seguir o padrão das estruturas do código em C abaixo, respeitando todos os tamanhos dados. Deve ser implementado com uma interface gráfica que mostre a animação da simulação da cache.


MemCache cache[16];
MemPrincipal principal[256];


//Opções de Menu

1- Ler Memoria
2- Escrever memoria
3- Estatísticas

As estatísticas abaixo devem ser implementadas:

3.1 Numero de Acessos
3.2 Numero de Acertos
3.3 Numero de Faltas
3.4 Numero de Leituras
3.5 Numero de Escritas
3.6 Numero de Acertos na Leitura
3.7 Numero de Acertos na Escrita
3.8 Numero de Faltas na Leitura
3.9 Numero de Faltas na Escrita


//Pontuação
Politica de Mapeamento: 5 pts

Politica de escrita: 2 pts

Estatísticas: 2 pts

Interface Gráfica: 1 pts

//Observações:

Tudo deve ser tratado como binário.
Caso seja necessário adição ou modificação no tamanho dos atributos nas estruturas, deve ser feito.
Mas sempre respeitando o tamanho da cache, da memória principal e das células.
Politicas serão sorteadas em sala.
