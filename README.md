#First Semester C Project:
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

//COLOR MACROS
#define RESET   "\033[0m"
#define RED     "\033[31m"
#define GREEN   "\033[32m"
#define YELLOW  "\033[33m"
#define BLUE    "\033[34m"
#define MAGENTA "\033[35m"
#define CYAN    "\033[36m"
#define WHITE   "\033[37m"
#define BOLD    "\033[1m"
#define UNDER   "\033[4m"
#define N        100


void typePrint(char *text)
{
    for(int i=0; text[i] != '\0'; i++) 
    {
        printf("%c", text[i]);
        fflush(stdout);
        for(long j=0; j<2000000; j++);
    }
}


void welcome() {
    printf("\n=======================================================\n");
    printf("         C PROGRAMMING PROJECT  \n");
    printf("=======================================================\n");
    printf(" Project Title: E VOTING SYSTEM\n");
    printf("-------------------------------------------------------\n");
    printf(" Submitted To: Mr. Pankaj Badoni\n");
    printf(" Submitted By: Harshita Pandey \n");
    printf(" Batch: 9\n");
    printf("=======================================================\n");
}


void print_heading () {   
    printf("\n" BLUE BOLD);
    printf("============================================================================================================================================================\n");
    printf("                                                               ELECTION COMMISSION OF INDIA\n");
    printf("============================================================================================================================================================\n");
    printf(RESET);
}


// GLOBAL FILE NAME list
char *candidateFile = "candidate.txt";
char *voterFile = "voters.txt";


int option,n;

struct Admin {
    int candidate_id;         //unique ID for every candidate.
    char candidate_name[100];   //name of the candidate.
    int votes;               //the vote counter for the candidate.
    char party[200];           //stores party name.
} candidates;


struct Voter {
    int voter_id; //id of the voter
    char name[50]; //name of the voter
    int hasVoted; //storingv variable whether voter has voted or not.
} voters; 


//declaring Main Module functions:
void Admin_Module();
void Voter_Module();
void Result_Module();


//the working functions for voters and candidates:
int  find_voter(int id);
void register_voter(int id, char name[]); 
void update_voter(int id);


struct Admin* load_candidate (int *count);
void save_candidate (struct Admin *candidate, int n);
void cast_vote (int voter_id);


int count_candidates() {
    FILE *fp = fopen(candidateFile, "r");
    if(!fp) return 0;

    int id, votes;
    char name[100], party[200];
    int count = 0;

    while(fscanf(fp, "%d %s %s %d", &id, name, party, &votes) == 4)
        count++;

    fclose(fp);
    return count;
}

struct Admin* load_candidate(int *count) {
    *count = count_candidates();
    if(*count == 0) return NULL;

    struct Admin *list = malloc(*count * sizeof(struct Admin));
    FILE *fp = fopen(candidateFile, "r");

    for(int i = 0; i < *count; i++)
    {
        fscanf(fp, "%d %s %s %d",
            &list[i].candidate_id,
            list[i].candidate_name,
            list[i].party,
            &list[i].votes);
    }

    fclose(fp);
    return list;
}


//-----VOTER FUNCTIONS-----

    int find_voter(int id) {
       FILE *fp = fopen(voterFile, "r");
       
       if (!fp)
        return -1;      // file is not existing means there are no voters


       struct Voter voters;

       while (fscanf (fp, "%d %s %d", &voters.voter_id, voters.name, &voters.hasVoted) != EOF)
        
        {
            if (voters.voter_id == id) 

                {
                  fclose(fp);
                  return voters.hasVoted; // 0 or 1
                }
        }
         fclose(fp);
         return -1;   // not found
    }
    

    void save_candidate(struct Admin *c, int count) {
        FILE *fp = fopen(candidateFile, "w");
         if(!fp) return;

          for(int i = 0; i < count; i++) 
          {
           fprintf(fp, "%-5d %-15s %-15s %d\n", c[i].candidate_id, c[i].candidate_name, c[i].party, c[i].votes);
          }
        fclose(fp);
    }


    void register_voter(int id, char name[]) {
        FILE *ptr= fopen (voterFile, "a");

        if (ptr==NULL)
        {
            printf("Error opening file.");
            return;
        }
           fprintf(ptr, "%d %s %d\n", id, name, 0);

           fclose(ptr);
    }


    //-----UPDATING VOTER-----

    void update_voter (int id) {
        FILE *fp= NULL;

        struct Voter voters[500];  
        int count = 0;


        fp = fopen("voters.txt", "r");

       if (!fp) 
       {
          printf("Error opening voters file!\n");
          return;
       }


    // read file
      
      while (fscanf(fp, "%d %s %d", &voters[count].voter_id, voters[count].name, &voters[count].hasVoted)!= EOF)
        {
           count++;
        }
       fclose(fp);



    //updating match voter

    for (int i = 0; i < count; i++) 
     {
        if (voters[i].voter_id == id) 

        {
            voters[i].hasVoted = 1;  //exist and has voted.
            break;
        }
     }


    // file from updated array
    fp = fopen(voterFile, "w");

    for (int i = 0; i < count; i++) 

      {
        fprintf(fp, "%d %s %d\n",  voters[i].voter_id,  voters[i].name,  voters[i].hasVoted);
      }
        fclose(fp);

    }



    //----CAST VOTE----

    void cast_vote (int id)
    {
        int count;
        struct Admin *candidate= load_candidate (&count);

        if (count==0)
          {
             printf("\n No candidate found. \n");
             free(candidate);
             return ;
          }

    printf("\n========== CANDIDATE ==========\n");

    for(int i = 0; i < count; i++) 
    {
        printf("%d. %s - %s\n", 
               candidate[i].candidate_id,
               candidate[i].candidate_name,
               candidate[i].party);
    }

    int c_id;

    printf("\n Enter candidate ID to vote: ");
    scanf("%d", &c_id);

    if(c_id < 1 || c_id > count) 
    {
        printf("\nInvalid choice!\n");
        free(candidate);
        return;
    }
    candidate[c_id - 1].votes++;

    save_candidate(candidate, count);
    update_voter(id);

    printf("\n Vote Recorded Successfully!\n");
    free(candidate);

}



void Admin_Module()  {
    printf(BOLD GREEN"                                                                          ADMIN SETUP ELECTION\n" RESET);

    FILE *admin;
    int i;

    admin = fopen("candidate.txt", "w");
    if(admin == NULL)
    {
        printf("Error opening file!");
        return;
    }

    printf("Enter the number of candidates: ");
    scanf("%d", &n);

    struct Admin *candidates = malloc(n * sizeof(struct Admin));

    for(i = 0; i < n; i++)
    {
        candidates[i].candidate_id = i + 1;
        candidates[i].votes = 0;

        printf("Enter Candidate %d name: ", i+1);
        scanf("%s", candidates[i].candidate_name);

        printf("Enter Party Name: ");
        scanf("%s", candidates[i].party);

        fprintf(admin, "%d %s %s %d\n",
                candidates[i].candidate_id,
                candidates[i].candidate_name,
                candidates[i].party,
                candidates[i].votes);
    }

    fclose(admin);
    free(candidates);

    printf("\nELECTION CREATED SUCCESSFULLY.\n");
}


    void Voter_Module() {
        int id;
        char name[30];


        printf("\n Enter Voter ID: ");
        scanf("%d", &id);


        int find= find_voter(id);

        if (find==1)
        {
            printf(RED "You have already VOTED!" RESET); //not allowed to vote again!
            return;
        }

        if (find==0)
        {
            printf("\n Welcome! You may vote now! \n");
            cast_vote(id);
            update_voter(id);
            return;
        }

        if (find==-1)
        {
            printf("\nNew Registration!\n ");
            printf("Please enter your NAME: \n");
            scanf("%s", name);

            register_voter(id, name);
            printf(" \n Registration Successful!!\n");

            cast_vote(id);
            return;
        }
        //for 0, register and vote
        cast_vote(id);
    }



    void Result_Module()
    {
          int count;
          struct Admin *c = load_candidate(&count);

        if(count == 0) 
        {
          printf("\n No candidates found! \n");
          free(c);
          return;
        }


           printf("\n-------------------------------------------------------\n");
           printf(BOLD "%-5s %-20s %-15s %s\n" RESET, "ID", "Candidate", "Party", "Votes");
           printf("-------------------------------------------------------\n");
           

    // Print each candidate row
    for (int i = 0; i < count; i++) 
    {
        printf("%-5d %-20s %-15s %d\n",
               c[i].candidate_id,
               c[i].candidate_name,
               c[i].party,
               c[i].votes );
    }
    printf("-------------------------------------------------------\n");

    //to find the winner candidate with highest votes

    int max_votes=0;
    for (int i=1; i<count;i++)
    {
        if (c[i].votes > c[max_votes].votes)
        max_votes=i;
    }

    printf(BLUE BOLD "\n================================================================== ELECTION RESULTS ==================================================================================\n" RESET);

    printf(GREEN BOLD "\n WINNER: %s (%s)\n" RESET, 
       c[max_votes].candidate_name, 
       c[max_votes].party);
    
       printf(GREEN " Votes: %d\n" RESET,c[max_votes].votes);
    free(c);
}


int main() {
    welcome();
    do
    {

    print_heading(); //printing the election commission of India heading.
    typePrint(CYAN BOLD "\n                                                                      E-VOTING SYSTEM                 \n" RESET);
    printf(YELLOW "1. ADMIN \n");
    printf(YELLOW "2. REGISTER AS VOTER AND VOTE \n");
    printf(YELLOW "3. VIEW RESULTS \n");
    printf(YELLOW "4. EXIT \n");

    printf(CYAN "============================================================================================================================================================\n" RESET);
                 
    printf(GREEN "Please select your choice: ");
    scanf("%d", &option);


    switch(option)
    {
        case 1:
        Admin_Module();
        break; 
        
        case 2:
        Voter_Module();
        break;

        case 3:
        Result_Module();
        break;

        case 4:
        exit(0);
        break;

        default:
        printf("Please choose a valid option!");
    }
     char again;
     printf("\nDo you want to continue? (y/n): ");
     scanf(" %c", &again);

     if (again == 'n' || again == 'N') {
    printf("\nThank you for using E-Voting System!\n");
    break; }
  }
    while(option!=4);
    return 0;
}
