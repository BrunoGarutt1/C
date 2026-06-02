#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

#define MAX_PALAVRAS 1000
#define MAX_LEN 50
#define MAX_STOP_WORDS 500

// Estrutura para armazenar a palavra e sua respectiva frequência
typedef struct {
    char palavra[MAX_LEN];
    int frequencia;
} ElementoDicionario;

// Funções 
int obter_proxima_palavra(FILE *f, char *palavra);
int busca_binaria_recursiva(ElementoDicionario dic[], int inicio, int fim, const char *palavra);
int eh_stop_word(char sw[][MAX_LEN], int total_sw, const char *palavra);
void inserir_ordenado(ElementoDicionario dic[], int *total, const char *palavra);

int main(int argc, char *argv[]) {
    // Validação dos argumentos de linha de comando
    if (argc != 3) {
        printf("Uso correto: %s <stopwords.txt> <texto.txt>\n", argv[0]);
        return 1;
    }

    // Abertura do arquivo de Stop Words
    FILE *f_sw = fopen(argv[1], "r");
    if (!f_sw) {
        printf("Erro ao abrir arquivo de stop words: %s\n", argv[1]);
        return 1;
    }

    char stop_words[MAX_STOP_WORDS][MAX_LEN];
    int total_sw = 0;
    char palavra_aux[MAX_LEN];

    // Carrega todas as stop words na memória
    while (obter_proxima_palavra(f_sw, palavra_aux)) {
        if (total_sw < MAX_STOP_WORDS) {
            strcpy(stop_words[total_sw], palavra_aux);
            total_sw++;
        }
    }
    fclose(f_sw);

    // Abertura do arquivo de texto
    FILE *f_texto = fopen(argv[2], "r");
    if (!f_texto) {
        printf("Erro ao abrir arquivo de texto: %s\n", argv[2]);
        return 1;
    }

    ElementoDicionario dicionario[MAX_PALAVRAS];
    int total_dicionario = 0;

    // Texto principal
    while (obter_proxima_palavra(f_texto, palavra_aux)) {
        // Ignora a palavra se ela for uma stop word
        if (!eh_stop_word(stop_words, total_sw, palavra_aux)) {
            // Busca binária recursiva obrigatória
            int idx = busca_binaria_recursiva(dicionario, 0, total_dicionario - 1, palavra_aux);
            
            if (idx != -1) {
                // Palavra encontrada
                dicionario[idx].frequencia++;
            } else {
                // Palavra nova
                if (total_dicionario < MAX_PALAVRAS) {
                    inserir_ordenado(dicionario, &total_dicionario, palavra_aux);
                }
            }
        }
    }
    fclose(f_texto);

    // Impressão dos resultados
    for (int i = 0; i < total_dicionario; i++) {
        printf("%s, %d\n", dicionario[i].palavra, dicionario[i].frequencia);
    }
    printf("total de palavras diferentes no dicionario = %d\n", total_dicionario);

    return 0;
}

/*
 //Lê o arquivo caractere por caractere isolando apenas sequências alfabéticas.
 Converte automaticamente todos os caracteres capturados para minúsculo
 */
int obter_proxima_palavra(FILE *f, char *palavra) {
    int ch;
    int i = 0;

    // Ignora qualquer caractere que não seja uma letra
    while ((ch = fgetc(f)) != EOF && !isalpha(ch)) {
        // Avança o ponteiro do arquivo
    }
    
    if (ch == EOF) return 0;

    // Captura a palavra convertendo para minúsculo
    palavra[i++] = tolower(ch);
    while ((ch = fgetc(f)) != EOF && isalpha(ch)) {
        if (i < MAX_LEN - 1) {
            palavra[i++] = tolower(ch);
        }
    }
    palavra[i] = '\0';
    return 1;
}

/**
 Algoritmo de Busca Binária implementado de forma Recursiva
 Retorna o índice do elemento caso encontrado, ou -1 caso contrário
 */
int busca_binaria_recursiva(ElementoDicionario dic[], int inicio, int fim, const char *palavra) {
    if (inicio > fim) {
        return -1;
    }

    int meio = inicio + (fim - inicio) / 2;
    int comp = strcmp(dic[meio].palavra, palavra);

    if (comp == 0) {
        return meio; 
    } else if (comp > 0) {
        // A palavra procurada está na metade esquerda
        return busca_binaria_recursiva(dic, inicio, meio - 1, palavra);
    } else {
        // A palavra procurada está na metade direita
        return busca_binaria_recursiva(dic, meio + 1, fim, palavra);
    }
}

/**
 Varre a lista de stop words de forma sequencial para validação
 */
int eh_stop_word(char sw[][MAX_LEN], int total_sw, const char *palavra) {
    for (int i = 0; i < total_sw; i++) {
        if (strcmp(sw[i], palavra) == 0) {
            return 1; // É stop word
        }
    }
    return 0; // Não é stop word
}

/**
 Realiza a inserção mantendo o vetor ordenado
 */
void inserir_ordenado(ElementoDicionario dic[], int *total, const char *palavra) {
    int pos = 0;

    // Encontra a posição correta para manter a ordem alfabética
    while (pos < *total && strcmp(dic[pos].palavra, palavra) < 0) {
        pos++;
    }

    // Desloca os elementos subsequentes para a direita para abrir espaço
    for (int i = *total; i > pos; i--) {
        dic[i] = dic[i - 1];
    }

    // Grava a nova palavra na posição correta e inicializa sua contagem
    strcpy(dic[pos].palavra, palavra);
    dic[pos].frequencia = 1;
    
    // Atualiza o contador de palavras do dicionário
    (*total)++;
}