Como foi solicitado aqui esta um sistema básico de cadastro de pessoas com capacidade para armazenar até no máximo 100 pessoas, o sistema conta com nome da pessoa, idade,CPF e uma opção da sair(free) a lista foi feita toda pelo Online GDB, nele temos a função "Run" que inicia o programa. Todas as funções estão bem fáceis de entender e todas funcionando corretamente. 


#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_PESSOAS 100 // Limite máximo de cadastros

// Estrutura para armazenar os dados de uma pessoa
typedef struct {
    char nome[50];
    int idade;
    char cpf[12];
} Pessoa;

// Função para adicionar um novo cadastro
Pessoa* adicionarCadastro(Pessoa* cadastros, int* tamanho, int* capacidade) {
    if (*tamanho == MAX_PESSOAS) {
        printf("Limite maximo de cadastros atingido (%d pessoas).\n", MAX_PESSOAS);
        return cadastros;
    }

    if (*tamanho == *capacidade) {
        // Se necessário aumentar a capacidade do array
        *capacidade = (*capacidade == 0) ? 1 : (*capacidade * 2);
        Pessoa* novoArray = realloc(cadastros, (*capacidade) * sizeof(Pessoa));
        if (novoArray == NULL) {
            printf("Erro ao alocar memoria.\n");
            return cadastros;
        }
        cadastros = novoArray;
    }

    // Preencher os dados da nova pessoa
    printf("Digite o nome: ");
    scanf(" %[^\n]", cadastros[*tamanho].nome);
    printf("Digite a idade: ");
    scanf("%d", &cadastros[*tamanho].idade);
    printf("Digite o CPF: ");
    scanf("%s", cadastros[*tamanho].cpf);

    (*tamanho)++;
    printf("Cadastro adicionado com sucesso!\n");

    return cadastros;
}

// Função para listar todos os cadastros
void listarCadastros(Pessoa* cadastros, int tamanho) {
    if (tamanho == 0) {
        printf("Nenhum cadastro para listar.\n");
        return;
    }

    printf("\n=== Lista de Cadastros ===\n");
    for (int i = 0; i < tamanho; i++) {
        printf("Indice: %d\n", i);
        printf("Nome: %s\n", cadastros[i].nome);
        printf("Idade: %d\n", cadastros[i].idade);
        printf("CPF: %s\n", cadastros[i].cpf);
        printf("------------------------\n");
    }
}

// Função para deletar um cadastro pelo índice
Pessoa* deletarCadastro(Pessoa* cadastros, int* tamanho, int* capacidade, int indice) {
    if (indice < 0 || indice >= *tamanho) {
        printf("Indice invalido.\n");
        return cadastros;
    }

    // Deslocar os elementos para preencher o espaço vazio
    for (int i = indice; i < *tamanho - 1; i++) {
        cadastros[i] = cadastros[i + 1];
    }

    (*tamanho)--;

    // Reduzir a capacidade se necessário
    if (*tamanho > 0 && *tamanho <= *capacidade / 4) {
        *capacidade /= 2;
        Pessoa* novoArray = realloc(cadastros, (*capacidade) * sizeof(Pessoa));
        if (novoArray != NULL) {
            cadastros = novoArray;
        }
    }

    printf("Cadastro removido com sucesso!\n");

    return cadastros;
}

int main() {
    Pessoa* cadastros = NULL; // Ponteiro para array dinâmico de cadastros
    int tamanho = 0;         // Número de cadastros atuais
    int capacidade = 0;      // Capacidade atual do array

    int opcao;
    do {
        printf("\n=== Sistema de Cadastro ===\n");
        printf("1. Adicionar Cadastro\n");
        printf("2. Listar Cadastros\n");
        printf("3. Deletar Cadastro\n");
        printf("4. Sair\n");
        printf("Escolha uma opcao: ");
        scanf("%d", &opcao);

        switch (opcao) {
            case 1:
                cadastros = adicionarCadastro(cadastros, &tamanho, &capacidade);
                break;
            case 2:
                listarCadastros(cadastros, tamanho);
                break;
            case 3: {
                int indice;
                printf("Informe o indice do cadastro a deletar: ");
                scanf("%d", &indice);
                cadastros = deletarCadastro(cadastros, &tamanho, &capacidade, indice);
                break;
            }
            case 4:
                printf("Saindo...\n");
                break;
            default:
                printf("Opcao invalida! Tente novamente.\n");
        }
    } while (opcao != 4);

    // Liberar a memória alocada antes de sair
    free(cadastros);

    return 0;
}
