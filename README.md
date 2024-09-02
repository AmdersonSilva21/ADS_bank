# ADS_bank
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_CLIENTES 100

typedef struct {
    char nome[50];
    char cpf[12];
    double saldo;
} Cliente;

Cliente clientes[MAX_CLIENTES];
int num_clientes = 0;

// Funções para o Gerente
void inserirCliente() {
    if (num_clientes >= MAX_CLIENTES) {
        printf("Número máximo de clientes atingido!\n");
        return;
    }

    Cliente novoCliente;
    printf("Nome do cliente: ");
    scanf(" %[^\n]%*c", novoCliente.nome);
    printf("CPF do cliente: ");
    scanf(" %s", novoCliente.cpf);
    printf("Saldo inicial: ");
    scanf("%lf", &novoCliente.saldo);

    clientes[num_clientes] = novoCliente;
    num_clientes++;
    printf("Cliente inserido com sucesso!\n");
}

void listarClientes() {
    if (num_clientes == 0) {
        printf("Nenhum cliente cadastrado.\n");
        return;
    }

    printf("\nLista de Clientes:\n");
    for (int i = 0; i < num_clientes; i++) {
        printf("Cliente %d: Nome: %s, CPF: %s, Saldo: %.2f\n", i+1, clientes[i].nome, clientes[i].cpf, clientes[i].saldo);
    }
}

int encontrarClientePorCPF(char cpf[]) {
    for (int i = 0; i < num_clientes; i++) {
        if (strcmp(clientes[i].cpf, cpf) == 0) {
            return i;
        }
    }
    return -1;
}

void alterarSaldoCliente() {
    char cpf[12];
    printf("CPF do cliente: ");
    scanf(" %s", cpf);

    int indice = encontrarClientePorCPF(cpf);
    if (indice == -1) {
        printf("Cliente não encontrado.\n");
        return;
    }

    printf("Saldo atual: %.2f\n", clientes[indice].saldo);
    printf("Novo saldo: ");
    scanf("%lf", &clientes[indice].saldo);
    printf("Saldo alterado com sucesso!\n");
}

void alterarCPFCliente() {
    char cpf[12];
    printf("CPF do cliente: ");
    scanf(" %s", cpf);

    int indice = encontrarClientePorCPF(cpf);
    if (indice == -1) {
        printf("Cliente não encontrado.\n");
        return;
    }

    printf("Novo CPF: ");
    scanf(" %s", clientes[indice].cpf);
    printf("CPF alterado com sucesso!\n");
}

void alterarNomeCliente() {
    char cpf[12];
    printf("CPF do cliente: ");
    scanf(" %s", cpf);

    int indice = encontrarClientePorCPF(cpf);
    if (indice == -1) {
        printf("Cliente não encontrado.\n");
        return;
    }

    printf("Novo nome: ");
    scanf(" %[^\n]%*c", clientes[indice].nome);
    printf("Nome alterado com sucesso!\n");
}

void excluirCliente() {
    char cpf[12];
    printf("CPF do cliente: ");
    scanf(" %s", cpf);

    int indice = encontrarClientePorCPF(cpf);
    if (indice == -1) {
        printf("Cliente não encontrado.\n");
        return;
    }

    for (int i = indice; i < num_clientes - 1; i++) {
        clientes[i] = clientes[i + 1];
    }

    num_clientes--;
    printf("Cliente excluído com sucesso!\n");
}

// Funções para o Cliente
void visualizarSaldo(int indice) {
    printf("Saldo atual: %.2f\n", clientes[indice].saldo);
}

void depositar(int indice) {
    double valor;
    printf("Valor a depositar: ");
    scanf("%lf", &valor);

    clientes[indice].saldo += valor;
    printf("Depósito realizado com sucesso! Novo saldo: %.2f\n", clientes[indice].saldo);
}

void sacar(int indice) {
    double valor;
    printf("Valor a sacar: ");
    scanf("%lf", &valor);

    if (valor > clientes[indice].saldo) {
        printf("Saldo insuficiente.\n");
    } else {
        clientes[indice].saldo -= valor;
        printf("Saque realizado com sucesso! Novo saldo: %.2f\n", clientes[indice].saldo);
    }
}

void transferir(int indiceOrigem) {
    char cpfDestino[12];
    printf("CPF do destinatário: ");
    scanf(" %s", cpfDestino);

    int indiceDestino = encontrarClientePorCPF(cpfDestino);
    if (indiceDestino == -1) {
        printf("Destinatário não encontrado.\n");
        return;
    }

    double valor;
    printf("Valor a transferir: ");
    scanf("%lf", &valor);

    if (valor > clientes[indiceOrigem].saldo) {
        printf("Saldo insuficiente.\n");
    } else {
        clientes[indiceOrigem].saldo -= valor;
        clientes[indiceDestino].saldo += valor;
        printf("Transferência realizada com sucesso!\n");
    }
}

void menuGerente() {
    int opcao;
    do {
        printf("\nMenu do Gerente:\n");
        printf("1. Inserir cliente\n");
        printf("2. Listar todos os clientes\n");
        printf("3. Alterar saldo do cliente\n");
        printf("4. Alterar CPF do cliente\n");
        printf("5. Alterar nome do cliente\n");
        printf("6. Excluir cliente\n");
        printf("0. Sair\n");
        printf("Escolha uma opção: ");
        scanf("%d", &opcao);

        switch (opcao) {
            case 1:
                inserirCliente();
                break;
            case 2:
                listarClientes();
                break;
            case 3:
                alterarSaldoCliente();
                break;
            case 4:
                alterarCPFCliente();
                break;
            case 5:
                alterarNomeCliente();
                break;
            case 6:
                excluirCliente();
                break;
            case 0:
                printf("Saindo...\n");
                break;
            default:
                printf("Opção inválida.\n");
        }
    } while (opcao != 0);
}

void menuCliente() {
    char cpf[12];
    printf("CPF do cliente: ");
    scanf(" %s", cpf);

    int indice = encontrarClientePorCPF(cpf);
    if (indice == -1) {
        printf("Cliente não encontrado.\n");
        return;
    }

    int opcao;
    do {
        printf("\nMenu do Cliente:\n");
        printf("1. Visualizar saldo\n");
        printf("2. Depositar\n");
        printf("3. Sacar\n");
        printf("4. Transferir\n");
        printf("0. Sair\n");
        printf("Escolha uma opção: ");
        scanf("%d", &opcao);

        switch (opcao) {
            case 1:
                visualizarSaldo(indice);
                break;
            case 2:
                depositar(indice);
                break;
            case 3:
                sacar(indice);
                break;
            case 4:
                transferir(indice);
                break;
            case 0:
                printf("Saindo...\n");
                break;
            default:
                printf("Opção inválida.\n");
        }
    } while (opcao != 0);
}

int main() {
    int opcao;
    do {
        printf("\nBem-vindo ao ADS Bank!\n");
        printf("1. Acessar como Gerente\n");
        printf("2. Acessar como Cliente\n");
        printf("0. Sair\n");
        printf("Escolha uma opção: ");
        scanf("%d", &opcao);

        switch (opcao) {
            case 1:
                menuGerente();
                break;
            case 2:
                menuCliente();
                break;
            case 0:
                printf("Saindo do sistema...\n");
                break;
            default:
                printf("Opção inválida.\n");
        }
    } while (opcao != 0);

    return 0;
}
