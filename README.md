# Atividade-IS-6

O objetivo dessa questão era criar uma implementação do problema do produtor consumidor utilizando threads. Para que a solução funcione corretamente é necessário garantir que ao inserir os dados no buffer os produtores não possam inserir os dados na mesma posição e eles também precisamproduzir apenas em posições vazias.

### Como executar:
Através dos comandos:
 - ```make```
 - ```make run```
 - ```make clean```

### Código:
Incluindo bibliotecas e declarando funções e variáveis globais:
```c
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>

void *fproducer(int id);
void *fconsumer(int id);
int isEmpty();
int isFull();

int P, C, N, B;
int buffer[2048];
int x = 0;;
```

Função int main() Cria as threads, verifica se foram criadas corretamente, e dá o "join".
```c
int main(void) {
  
  printf("Insert P C N B:\n");
  scanf("%d %d %d %d", &P, &C, &N, &B);

  pthread_t producer[P], consumer[C];
  
  for(int i = 0; i<P; i++){
		pthread_create(&(producer[i]), NULL, fproducer, i);
	}

  for(int i = 0; i<C; i++){
		pthread_create(&(consumer[i]), NULL, fconsumer, i);
	}

	for(int i = 0; i<C; i++){
		pthread_join(consumer[i], NULL);
	}

	for(int i = 0; i<P; i++){
		pthread_join(producer[i], NULL);
	}
  
  return 0;
}
```

Função fproducer (executa a função f(x) = 2x + 1 caso as condições estejam cumpridas):
```c
void *fproducer(int id){
  int y = 0;
  while(1){
    if (x >= 0 && N >= x){
      y = 2*x + 1;
    }
    if (x == N) {
      x = 0;
      x = 2*x + 1;
    }
    for (int j = 0; j < N; j++) {
        if (buffer[j] == NULL) {
          printf("Produtor %d produzindo %d na posição %d\n", id, y, j);
          buffer[j] = y;
          break;
        }
    }
    x++;
  }
}
```

Função fconsumer (printa o estado do consumidor):
```c
void *fconsumer(int id){

    for (int l = 0; l < N; l++) {
        if (buffer[l] != NULL) {
            printf("Consumidor %d consumindo %d na posição %d\n", id, buffer[l], l);
            buffer[l] = NULL;
            break;
        }
    }
}
```

