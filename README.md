[mochila_projeto.zip](https://github.com/user-attachments/files/23733287/mochila_projeto.zip)
/* main.c
   Sistema de mochila (nível novato)
   - struct Item { char nome[30]; char tipo[20]; int quantidade; }
   - vetor de até 10 itens
   - operações: inserir, remover (por nome), listar, buscar (sequencial)
*/

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_ITENS 10
#define NOME_LEN 30
#define TIPO_LEN 20

typedef struct {
    char nome[NOME_LEN];
    char tipo[TIPO_LEN];
    int quantidade;
} Item;

Item mochila[MAX_ITENS];
int total = 0;

void remove_newline(char *s) {
    size_t len = strlen(s);
    if (len > 0 && s[len-1] == '\n') s[len-1] = '\0';
}

void inserirItem() {
    if (total >= MAX_ITENS) {
        printf("Mochila cheia.\n");
        return;
    }
    Item it;
    printf("Nome: ");
    fgets(it.nome, NOME_LEN, stdin);
    remove_newline(it.nome);
    for (int i=0;i<total;i++){
        if(strcmp(mochila[i].nome,it.nome)==0){
            printf("Item já existe.\n");
            return;
        }
    }
    printf("Tipo: ");
    fgets(it.tipo, TIPO_LEN, stdin);
    remove_newline(it.tipo);

    printf("Quantidade: ");
    if (scanf("%d",&it.quantidade)!=1){
        printf("Valor inválido.\n");
        while(getchar()!='\n');
        return;
    }
    while(getchar()!='\n');

    mochila[total++] = it;
    printf("Item inserido.\n");
}

int buscarIndicePorNome(const char *nome){
    for(int i=0;i<total;i++){
        if(strcmp(mochila[i].nome,nome)==0) return i;
    }
    return -1;
}

void removerItem(){
    char nome[NOME_LEN];
    printf("Nome a remover: ");
    fgets(nome,NOME_LEN,stdin);
    remove_newline(nome);
    int idx = buscarIndicePorNome(nome);
    if(idx==-1){
        printf("Não encontrado.\n");
        return;
    }
    for(int i=idx;i<total-1;i++){
        mochila[i]=mochila[i+1];
    }
    total--;
    printf("Removido.\n");
}

void listarItens(){
    if(total==0){
        printf("Vazio.\n");
        return;
    }
    for(int i=0;i<total;i++){
        printf("%d) %s | %s | %d\n",i+1,mochila[i].nome,mochila[i].tipo,mochila[i].quantidade);
    }
}

void buscarItem(){
    char nome[NOME_LEN];
    printf("Nome a buscar: ");
    fgets(nome,NOME_LEN,stdin);
    remove_newline(nome);
    int idx = buscarIndicePorNome(nome);
    if(idx==-1){
        printf("Não encontrado.\n");
        return;
    }
    Item *it = &mochila[idx];
    printf("Encontrado: %s | %s | %d\n",it->nome,it->tipo,it->quantidade);
}

void menu(){
    printf("\n1 Inserir\n2 Remover\n3 Listar\n4 Buscar\n0 Sair\nEscolha: ");
}

int main(){
    int op;
    do{
        menu();
        if(scanf("%d",&op)!=1){
            while(getchar()!='\n');
            op=-1;
            continue;
        }
        while(getchar()!='\n');
        switch(op){
            case 1: inserirItem();break;
            case 2: removerItem();break;
            case 3: listarItens();break;
            case 4: buscarItem();break;
            case 0: printf("Saindo.\n");break;
            default: printf("Inválido.\n");
        }
    }while(op!=0);
    return 0;
}

