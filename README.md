# Binary-Search-Tree-project-Dictionary-
Data structure (COMP2421) 
----------

##### Binary Search Tree in C Language 

###### In this project the main problem is the way to read the file suplied with problem


```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

//#include "ANSI-color-codes.h"
//https://gist.github.com/RabaDabaDoba/145049536f815903c79944599c6f952a
int TEXT[10000];


#define BLK "\e[0;30m"
#define RED "\e[0;31m"
#define GRN "\e[0;32m"
#define YEL "\e[0;33m"
#define BLU "\e[0;34m"
#define MAG "\e[0;35m"
#define CYN "\e[0;36m"
#define WHT "\e[0;37m"
#define reset "\e[0m"

struct dict{  // define the BST
    char word[100];
    char meaning[100];
    struct dict* left, *right;
}*Root=NULL;

typedef struct dict dictionary;

// functions ============================
void insert(dictionary* temp);
void Start_with_specific();
void view(dictionary *ptr);
void list();
void Search();
int check(char a[],char b[]);
void write_on_file_cmd(dictionary *ptr, char text[]);
void read_file();
void write_on_file();
//=======================================
int check(char a[],char b[]){
    int i,j,c;
    for(i=0,j=0; a[i]!='\0'&& b[j]!='\0'; i++,j++){
        if(a[i]>b[j]){
            c=1;
            break;
        }else if(a[i]<b[j]){
            c=-1;
            break;
        }else{
            c=0;
        }
    }
    if(c==1)
        return 1;
    else if (c==-1)
        return -1;
    else
        return 0;
}
void Search(){
    char w[10];
    dictionary *ptr;
    int flag=0;
    ptr=Root;
    printf("\n Enter the word..\n");
    scanf("%s",w);
    while(ptr!=NULL && flag==0){
        if(check(w,ptr->word)==1)
            ptr=ptr->right;
        else if(check(w,ptr->word)==-1)
            ptr=ptr->left;
        else{
            flag=1;
            printf("----------------------------------------");
            printf("\nYour Word: %s \tMeaning: %s",ptr->word,ptr->meaning);
            printf("\n---------------------------------------- :-) ");
            break;
        }
    }
    if(flag==0 && ptr==NULL)
        printf("\nWord not found!!");
}
void traversal(dictionary *T,char c)
{
    if ( T == NULL)
        return ;
    traversal(T->left,c);
    if(T->word[0]==c)
        printf("word [%s] >> meaning [%s] \n",T->word,T->meaning );
    traversal(T->right,c);
}
void search_with_spacific_char(){
    printf("charachter ..");
    char null;
    scanf("%c",&null);
    char c;
    scanf("%c",&c);
    traversal(Root,c);
}
dictionary *find_min( dictionary *T)
{
    if ( T == NULL ) //empty tree
        return NULL;
    else
    if ( T->left == NULL ) //node itself
        return ( T );
    else
        return ( find_min ( T->left )); //find min recursive
}
dictionary* delete ( dictionary* T, char word[50])
{
    dictionary* tmp_cell, *child;
    if ( T == NULL )
        printf("Element not found");
    else if ( word < T->word)
        T->left = delete(T->left, word);
    else if ( word > T->word)
        T->right = delete(T->right, word);
    else if ( T->left && T->right ) //found element and has (right ,left) elements
    {
        tmp_cell = find_min(T->right);
        strcpy(T->word,tmp_cell->word);
        T->right = delete(T->right, T->word);
    }
    else
    {
        tmp_cell = T;
        if ( T->left == NULL)
            child = T->right;
        if (T->right == NULL)
            child = T->left;
        free ( tmp_cell);
        return child;
    }
    return T;
}
void Update(){
    char w[10];
    char meaning[100];
    strcpy(meaning,"");
    dictionary *ptr;
    int flag=0;
    ptr=Root;
    printf("\n Enter the word you want to change..");
    scanf("%s",w);
    while(ptr!=NULL && flag==0){
        if(check(w,ptr->word)==1)
            ptr=ptr->right;
        else if(check(w,ptr->word)==-1)
            ptr=ptr->left;
        else{
            flag=1;
            printf("\n (Update) Enter the meaning ..");
            scanf("%s",meaning);
            strcpy(ptr->meaning,meaning);

            printf("----------------------------------------");
            printf("\nYour Word Updated: %s \tMeaning: %s",ptr->word,ptr->meaning);
            printf("\n---------------------------------------- :-) ");
            break;
        }
    }
    if(flag==0 && ptr==NULL)
        printf("\nWord not found!!");
}
void insert(dictionary* temp){
    dictionary *ptr,*par;
    int flag=0;
    ptr=Root;
    if(Root==NULL)
        Root=temp;
    else{
        while(ptr!=NULL){
            if(check(temp->word,ptr->word)==1){
                par=ptr;
                ptr=ptr->right;
            }
            else if(check(temp->meaning,ptr->word)==-1){
                par=ptr;
                ptr=ptr->left;
            }
            else{
                flag=1;
                printf("\nWord Exists!!\n" );
                break;
            }
        }
        if(flag==0){
            if(check(par->word,temp->word)==1)
                par->left=temp;
            else if(check(par->word,temp->word)==-1)
                par->right=temp;
        }
    }
}
void view(dictionary *ptr){
    if(Root==NULL){
        printf("\nEmpty Dictionary\n");
    }
    else{
        if(ptr!=NULL){
            view(ptr->left);
            printf("==========================");
            printf("\nword: %s",ptr->word);
            printf("\nmeaning: %s",ptr->meaning);
            printf("\n==========================");
            view(ptr->right);
        }
    }
}
void list(){
    int ch;
    dictionary  *temp;
    while(ch!=10){
        printf("\n|================================================================|\n");
        printf("\t\t\t MENU \t\t\n");
        printf("|================================================================|\n");

        printf("\t1.Read File\n");
        printf("\t2.Search\n");
        printf("\t3.Update\n");
        printf("\t4.Insert\n");
        printf("\t5.view all in  alphabetic order\n");
        printf("\t6.words that start with a specific character\n");
        printf("\t7. Delete a word from the dictionary.\n");
        printf("\t8. Delete all words that start with a specific letter.\n");
        printf("\t9.Save all words back in file dictionary.txt.\n");
        printf("\t10.Quit\n");
        printf("\n\t Your choice please...");



        scanf("%d",&ch);
        switch(ch){
            case 1:
                read_file();
                break;
            case 2:
                Search();
                break;
            case 3:
                Update();
                break;
            case 4:
                // initialize the Binary Tree of the Dictionary
                temp=(dictionary *) malloc(sizeof(dictionary));
                temp->left=NULL;
                temp->right=NULL;
                printf("\nInsert word:");
                scanf("%s",temp->word);
                printf("\nInsert meaning:");
                scanf("%s",temp->meaning);
                insert(temp);
                break;
            case 5:
                view(Root);
                break;
            case 6:
                search_with_spacific_char();
            break;
            case 7:
            {
//                Delete a word from the dictionary.
                char word[50];
                printf("Enter word to delete ..\n");
                scanf("%s",word);
                Root=delete(Root,word);
            }
                break;
            case 8:
//                Delete all words that start with a specific letter.
                printf("Sorry it was a bizzey week\n  ");
                printf("basicly just use the search_for_character function and delete every time, it matches\n");
                break;
            case 9:
                write_on_file();
                break;

            case 10:
                exit(0);
        }
    }
}
void read_file(){

    FILE* fptr = fopen("C:\\Users\\Eng. Hammam\\CLionProjects\\untitled1\\dictionary.txt", "r");
    char word[50];
    char meaning[200];
    char line[600];
    while(fgets(line, sizeof(char) * 600, fptr)){
        int chars_now;
        int chars_consumed = 0;
        int read = sscanf( line + chars_consumed, "%*d. %[^:]: %[^\t\n0123456789]%n", word, meaning, &chars_now);

        while (read > 0)
        {
            chars_consumed += chars_now;

            printf("insert word [%s] >> meaning [%s]\n", word, meaning);
            printf("  ");
            dictionary *temp;
            temp = (dictionary *) malloc(sizeof(dictionary));
            temp->left = NULL;
            temp->right = NULL;
            strcpy(temp->word,word);
            strcpy(temp->meaning,meaning);
            insert(temp);
            read = sscanf( line + chars_consumed, "%*d. %[^:]: %[^\t\n0123456789]%n", word, meaning, &chars_now);
        }
    }

    fclose(fptr);



}

void write_on_file_cmd(dictionary *ptr ,char text[]){
    if(Root==NULL){
        printf("\nEmpty Dictionary\n");
    }
    else{
        if(ptr!=NULL){
            view(ptr->left);

            char line[400];
            strcpy(line,ptr->word);
            strcat(line,": ");
            strcat(line,ptr->meaning);
            strcat(line,"\t\n");
            strcat(text,line);
            FILE *dictionary  = fopen("C:\\Users\\Eng. Hammam\\CLionProjects\\untitled1\\dic_out.txt", "w");
            fputs(text, dictionary);


            view(ptr->right);
        }
    }
}
void write_on_file(){
    char text[10000];
    write_on_file_cmd(Root,text);

}

int main(){
    list();






     return 0;
}

```

## The file which I shouold read from :
```
1. foe: enemy		2. vast: huge		3. purchase: buy	4. drowsy: sleepy

5. absent: missing	6. prank: trick		7. feeble: weak	8. annual: yearly

9. sturdy: strong	10. reply: answer	11. voyage: trip	12. shiver: shudder

13. slumber: sleep	14. meadow: field	15.banner: flag	16. loyal: faithful

17. ill: sick		18. vacant: empty	19. stalk: stem		20. wild: untamed

21.slosh: splash	22. frayed: worn	23. overcast: cloudy	24. mammoth: large

25. furious: angry	26. assist: help		27. task: job	28. bothersome: annoying

29. lurk: wait		30. orbit: circle	31. deep: many feet of water

32. shallow: not much water			33. flexible: bends easily

34. rigid: stiff		35. pain: what you feel when something bad happens

36. pleasure: a happy feeling			37. repair: fix		38. break: shatter

39. infant: baby	40. adult: grownup	41. bright: light	42. neat: tidy

43. dim: not much light			44. sloppy: messy	45. gracious: polite

46.attic: at the top of the house		47. cellar: at the bottom of the house

48. rude: not polite	49. borrow: you get something

50. lend: to give something			51. birdbath: bath for birds

52. eyelid: protects your eye			53. waterfall: when water drops over a cliff

54. keyboard: part of a piano that you play	55. lunchtime: middle meal of the day

56. hairbrush: what you use to brush hair	57. springboard: flexible board to jump from

58. scorekeeper: someone who keeps score	59. rainbow: a band of colors

60. catfish: a fish with whiskers		61. beehive: where bees make honey

62. sandbox: where kids play with sand	63. hillside: steep, sloping land

64. spaceship: what an astronaut rides in	65. applesauce: made when cooking apples

66. homework: assignment done at home	67. crosswalk: where you should cross street

68. railroad: what trains travel on		69. turtleneck: a long neck

70. rowboat: small boat  moved by rowing	71.fur: covering of most animals

72. fir: a kind of evergreen tree		73. principle: a rule

74. principal: the head of a school		75. paws: feet

76. pause: take a break	77. wail: cry	78. whale: a large animal living in the sea

79. berry: a fruit		80. bury: to cover up

81. stake: a stick that you hammer into ground	82. steak: meat you eat

83. peak: top of a mountain		84. peek: to look

85. council: a group of people making plans		86. counsel: to give advice

87. ant: an insect	88. aunt: female relative	89. threw: past tense of throw

90. through: from here to there	91. dove: bird/past tense of dive

92. record: to write down/something important  	93. live: living/to dwell

94. lead: to be at the beginning/a type of metal	95. wind: air that moves/to turn

96. grandchildren: children of your children		97. echoes: repeated sounds

98. halves: two equal parts	99. geese: large birds that make honking sound

100. mysteries: things that are secret or hard to explain

101. oxen: large farm animals in the cattle family	102. sketches: quick drawings

103. feet: at  he end of our legs 104. sheep: animals whose fur is used for wool

105. coast: land along the sea 106. host: one who gives the party

107. limb: a branch of a tree	108. fern: a kind of plant	109. glee: joy

110. yearn: to really long for something	111. plea: to beg for something

112. shriek: a loud sound	113. creek: a smaller, moving body of water

114. moose: a large animal with antlers

115. chipmunk: a small animal resembling a squirrel

116. noodle: made of flour, water, and eggs		117. kindergarten: before 1st grade

118. loft: a room just under the roof of a building

119. bungalow: a small, one story house	120. dinghy: a small boat

121.boss: the person in charge of a job    122. drum: something you hit to make sounds

123. patio: a paved area near a house	124: plaza: an open space in a city or town

125. ballet: a form of dance	126. garage: a place to park cars

127. pizza: a kind of pie with cheese and tomatoes on a crust

128. �bravo�: what an audience yells when they like the performance

129. bike: something you pedal to make its wheels move

130. exam: a kind of test	131. mitt: what baseball players catch with

132. bus: what some kids ride to school 133. lab: where scientists do their research

134. zoo: a place where animals are kept	135. sub: something that travels under water

136. auto: a form of transportation	137. kit: a baby fox 	138. piglet: a baby pig

139. math: the study of numbers, shapes, and measurements 140. fawn: a baby deer

141. gosling: a baby goose	142. cub: a young bear, lion, or tiger

143. calf: a baby cow, elephant, or whale	144. foal: a young horse or donkey

145. cygnet: a young swan	146. kid: a young goat		147. joey: a baby kangaroo

148. freighter: a ship that carries cargo 149. helm: the wheel used for steering the ship
```
