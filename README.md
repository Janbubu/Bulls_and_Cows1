# Bulls_and_Cows1
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#define ATTEMPT 9
int generate_target_number(void){
        srand(time(0));
        int num=0;
        int i,j;
        int str[3];
        for(i=0; i<3; i++){
                str[i] = rand()%10;
        }
        for(i=0; i<3;){
                for(j=0; j<3; j++);
                if(j<i) str[i] = rand()%10;
                else i++;
        }
        for(i=0; i<3; i++){
                num = num*10+str[i];
        }
        return num;
}

int is_different_digit(int num){
        int digits[3];
        int is_different = 1;
        digits[0] = num%10;
        digits[1] = (num/10)%10;
        digits[2] = num/100;
        if ((digits[0]==digits[1])||(digits[0]==digits[2])||(digits[1]==digits[2]))
                is_different = 0;
        return is_different;
}
int guess_number(void){
        int num;
        while(1){
                printf("Enter your guess: ");
                scanf("%d", &num);
                if(num<1000 && is_different_digit(num)) break;
                printf("Input Error Wrong number format\n");
        }
        return num;
}

int get_match_result(int target, int guessed){
        int n_strike=0, n_ball=0;
        int target_digit[3];
        int guessed_digit[3];

        int i,j;

        target_digit[0] = target/100;
        target_digit[1] = (target/10)%10;
        target_digit[2] = target%10;

        guessed_digit[0] = guessed/100;
        guessed_digit[1] = (guessed/10)%10;
        guessed_digit[2] = guessed%10;

        for (i=0; i<3; i++){
                for(j=0; j<3; j++){
                        if((i==j) && target_digit[i] == guessed_digit[j])
                                n_strike++;
                        else if((i!=j) && target_digit[i] == guessed_digit[j])
                                n_ball++;
                        else;
                }
        }
        return (n_strike*10 + n_ball);
}

void receive_match_result(int result, int guessed) {
        int n_strike = result/10, n_ball = result%10;
        switch (result) {
                case 30 : printf("Congratulation !!!\n");
                        break;
                case 0 : printf("Oops !!!\n");
                        break;
                default : printf("Nice try !!!\n");
        }
        printf("Your guess %d is [%d] strikes and [%d] balls\n",
                guessed, n_strike, n_ball);
}

int main(void)
{
        int n_attempt = 0;
        int target_num, guessed_num, match_result;

        srand(time(0));
        target_num = generate_target_number();
        do{
                printf("Attempt [%d/%d]", ++n_attempt, ATTEMPT);
                guessed_num = guess_number();
                match_result = get_match_result(target_num, guessed_num);
                receive_match_result(match_result, guessed_num);
        }while (n_attempt <= ATTEMPT && match_result !=30);

        if (match_result==30)
                printf("Congratulation!!! You win\n");
        else
                printf("Nice try! But you lose\n");
return 0;
}
