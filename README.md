# ARITMETICA_PERGUNTAERESPOSTA
Este código na linguagem C (exigido pela faculdade esta linguagem) está em desenvolvimento para um programa em pergunta e resposta sobre aritmética para alunos do 6º ao 9º ano. Acompanhem a evolução ; )

#include <stdio.h>
#include <stdlib.h>
#include <locale.h>
#include <string.h>
#include <time.h>
#include <math.h>
#include <windows.h>

#define MAX_PARTICIPANTS 50
#define MAX_NAME_LENGTH 50

typedef struct {
    char name[MAX_NAME_LENGTH];
    int score;
    int series;
} Participant;

// Função para exibir o placar dos participantes.
void showScoreboard(Participant participants[], int numParticipants) {
    printf("=== Placar dos participantes ===\n");
    for (int i = 0; i < numParticipants; i++) {
        printf("%s (Série %d): %d pontos\n", participants[i].name, participants[i].series, participants[i].score);
    }
    printf("\n");
}

// Função para gerar um número aleatório no intervalo, limitados por: min, max.
int Gerador_Numeros(int min, int max) {
    return rand() % (max - min + 1) + min;
}

// Função para gerar uma pergunta que será baseada na série e retornar o resultado.
int Gerador_Questoes(int series, int *operand1, int *operand2, char *operatorSymbol) {
    switch (series) {
        case 6:
            *operand1 = Gerador_Numeros(1, 100);
            *operand2 = Gerador_Numeros(1, 100);
            switch (Gerador_Numeros(1, 3)) {
                case 1:
                    *operatorSymbol = '+';
                    return *operand1 + *operand2;
                case 2:
                    *operatorSymbol = '-';
                    return *operand1 - *operand2;
                case 3:
                    *operatorSymbol = '*';
                    return *operand1 * *operand2;
            }
            break;
        case 7:
            *operand1 = Gerador_Numeros(1, 100);
            *operand2 = Gerador_Numeros(1, 100);
            switch (Gerador_Numeros(1, 4)) {
                case 1:
                    *operatorSymbol = '+';
                    return *operand1 + *operand2;
                case 2:
                    *operatorSymbol = '-';
                    return *operand1 - *operand2;
                case 3:
                    *operatorSymbol = '*';
                    return *operand1 * *operand2;
                case 4:
                    *operatorSymbol = '/';
                    return *operand1 / *operand2;
            }
            break;
        case 8:
            *operand1 = Gerador_Numeros(-10, 100);
            *operand2 = Gerador_Numeros(-10, 10);
            switch (Gerador_Numeros(1, 5)) {
                case 1:
                    *operatorSymbol = '+';
                    return *operand1 + *operand2;
                case 2:
                    *operatorSymbol = '-';
                    return *operand1 - *operand2;
                case 3:
                    *operatorSymbol = '*';
                    return *operand1 * *operand2;
                case 4:
                    *operatorSymbol = '/';
                    return *operand1 / *operand2;    
                case 5:
                    *operatorSymbol = '^';
                    return pow(*operand1, *operand2);
            }
            break;
        case 9:
            *operand1 = Gerador_Numeros(-10, 100);
            *operand2 = Gerador_Numeros(-10, 10);
            switch (Gerador_Numeros(1, 5)) {
                 case 1:
                    *operatorSymbol = '+';
                    return *operand1 + *operand2;
                case 2:
                    *operatorSymbol = '-';
                     return *operand1 - *operand2;
                case 3:
                    *operatorSymbol = '*';
                     return *operand1 * *operand2;
                case 4:
                    *operatorSymbol = '/';
                    return *operand1 / *operand2;
                case 5:
                    *operatorSymbol = '^';
                    return pow(*operand1, *operand2);
            }
            break;
    }
    return 0;
}

// Função para obter o tempo limite para responder a uma pergunta com base na série e número de questões que foram acertadas.
int getQuestionTime(int series) {
    int TempoMax;

    switch (series) {
        case 6:
            TempoMax = 30;
            break;
        case 7:
            TempoMax = 30;
            break;
        case 8:
            TempoMax = 25;
            break;
        case 9:
            TempoMax = 25;
            break;
        default:
            TempoMax = 10;
            break;
    }
    // Irá gerar um valor aleatório do tempo mantendo as prioridades.
    int TempoRandom = Gerador_Numeros(5, TempoMax);
    return TempoRandom;
}


int main() {
    setlocale(LC_ALL, "UTF-8");
    SetConsoleOutputCP(CP_UTF8);// Define o código de página para UTF-8, permitindo que os caracteres acentuados sejam exibidos corretamente no terminal do Windows.

    // Gerador de números aleatórios.
    srand(time(NULL));

    // Variavéis.
    Participant participants[MAX_PARTICIPANTS];
    int numParticipants = 0;
    int Questao;
    int operand1, operand2;
    char operatorSymbol;
    int result;
    int timer;
    char name[MAX_NAME_LENGTH];

    printf("=== Bem-vindo ao programa de perguntas e respostas ===\n\n");

    //Loop principal do jogo.
    while (1) {
        // Nome do participante.
        printf("Digite o nome do participante (ou 'sair' para encerrar): ");
        fgets(name, sizeof(name), stdin);
        name[strcspn(name, "\n")] = '\0';
        //Se for pressionado 'Sair'
        if (strcmp(name, "sair") == 0) {
            printf("Venha testar seu conhecimento");
            getchar();
            break;
        }
        // Série do participante.
        printf("Digite a série do participante (6 a 9): ");
        scanf("%d", &participants[numParticipants].series);
        getchar();
        // String onde armazenara o nome, série e pontuação do participante.
        strcpy(participants[numParticipants].name, name);
        participants[numParticipants].score = 0;

        printf("\nJogador(a): %s (Série %d)\n", participants[numParticipants].name, participants[numParticipants].series);
        printf("Boa sorte!\n\n");

        int errors = 0;
        int consecutiveCorrect = 0;
        int difficulty = 1;

        // Loop das perguntas.
        while (errors < 3) {
            operand1 = Gerador_Numeros(1, 10 * difficulty);
            operand2 = Gerador_Numeros(1, 10 * difficulty);
            result = Gerador_Questoes(participants[numParticipants].series, &operand1, &operand2, &operatorSymbol);
            timer = getQuestionTime(participants[numParticipants].series);

            printf("Questão:\n");
            printf("Quanto é %d %c %d?\n", operand1, operatorSymbol, operand2);
            printf("Você tem %d segundos para responder.\n", timer);

            time_t start_time = time(NULL);
            time_t current_time;

            while (1) {
                current_time = time(NULL);
                double elapsed_time = difftime(current_time, start_time);

                if (elapsed_time >= timer) {
                    printf("Tempo esgotado! Resposta incorreta!\n\n");
                    errors++;
                    consecutiveCorrect = 0;
                    break;
                }

                if (scanf("%d", &Questao) == 1) {
                    if (Questao == result) {
                        participants[numParticipants].score++;
                        printf("Resposta correta!\n\n");
                        consecutiveCorrect++;

                        if (consecutiveCorrect % 3 == 0 && difficulty < 10) {
                            difficulty++;
                            printf("Parabéns! A dificuldade aumentou para %d.\n", difficulty);
                        }

                        break;
                    } else {
                        printf("Resposta incorreta!\n\n");
                        errors++;
                        consecutiveCorrect = 0;
                        break;
                    }
                }
            }

            if (errors >= 3)
                break;
        }
        // Final e pontuação do jogo.
        printf("==== Fim do jogo para %s! ====\n", participants[numParticipants].name);
        printf("==== Pontuação final: %d   ====\n\n", participants[numParticipants].score);

        numParticipants++;
        showScoreboard(participants, numParticipants);
    }

    return 0;
}
