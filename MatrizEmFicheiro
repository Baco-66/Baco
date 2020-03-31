#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>     /*chamadas ao sistema: defs e decls essenciais*/
#include <sys/wait.h>   /*chamadas wait*() e macros relacionadas*/
#include <sys/types.h>
#include <fcntl.h>    /*O_RDONLY, O_WRONLY, O_CREAT, O_* */
 
 
 
void geraMatrizAleatoria (int linhas, int colunas, int fd) {
        time_t t;
        srand((unsigned) time(&t));
        int exp = 10;
        int num = 0;
        char buf[2];
        for (int i=0; i<linhas; i++) {
                for (int j=0; j<colunas;j++) {
                        exp = 10;
                        num = rand() % 50;
                        for(int i = 0; exp != 0; exp /= 10 , i++){
                                buf[i] = num/exp + '0';
                                num = num % exp;
                        }
                        if ((write(fd,&buf,2)) == -1)
                                printf("Erro no print \n");
                        //write(fd," ",1);
                }
                if (write(fd,"\n",1)== -1)
                        printf("Erro no print \n");
        }
}
 
int procuraMatriz(int linhas, int colunas, int fd, int proc){
 
        int pid[linhas];
        int status;
        char buf[3];
        buf[2] = '\0';
        int exp = 10;
        char procura[3];
        procura[2] = '\0';
        for(int i = 0; exp != 0; exp /= 10 , i++){
                procura[i] = proc/exp + '0';
                proc = proc % exp;
        }
 
        printf("%s\n",procura);
        for (int i=0; i<linhas; i++) {
 
                if((pid[i] = fork()) == 0){
 
 
                        lseek(fd,(colunas*i*2)+1*i,SEEK_SET);
 
                        for (int j=0; j<colunas;j++) {
                                if (read(fd,buf,2) == -1)
                                        printf ("Falhou read\n");
                                //printf("[%s]",buf);
                                if (procura[0] == buf[0] && procura[1]==procura[1]) {
                                        _exit(i);
                                }
                        }
                printf("\n");
                _exit(-1);
                }
        }
 
        for (int i = 0; i<linhas; i++) {
                pid_t terminated_pid = waitpid(pid[i],&status,0);
 
                if (WIFEXITED(status)) {
                        if (WEXITSTATUS(status) <255) {
                                printf("[pai] process %d exited, found number at row: %d\n", terminated_pid, WEXITSTATUS(status));
                        }
                        else {
                               printf("[pai] process %d exited. nothing found\n", terminated_pid);
                        }
                }
                else {
                                printf("[pai] process %d exited. something went worng\n", terminated_pid);
                }
        }
 
 
        return 0;
}
 
int main () {
        int colunas = 0;
        int linhas = 0;
        time_t t;
        srand((unsigned) time(&t));
 
        while(linhas>255 || linhas == 0)
                linhas = rand() % 50;
        while(colunas<linhas*10)
                colunas = linhas*(rand() % 50);
 
        printf("matriz de tamanho %d por %d\n",linhas, colunas);
 
        int fd = open("matrizAleatoria.txt",O_CREAT|O_RDWR|O_TRUNC,0600);
 
        geraMatrizAleatoria(linhas,colunas,fd);
 
        int procura = rand() % 50;
        procuraMatriz(linhas,colunas,fd,procura);
 
        close(fd);
        return 0;
}
