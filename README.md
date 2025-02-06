#include <stdlib.h>
#include <stdio.h>
#include <math.h>
#include <ctype.h>
#include <time.h>

/*
 *  Paper Title
 *  	- "Implementation of Microelectrode Implants and AI as as Effective Method of Improving Motor Skills and Sense of Touch in Prosthetic Hands"
 *
 *  Main Ideas of my Paper
 *  	- Microelectrode array implants, accompanied by machine learning artificial intelligence can be used to read and translate nerve inputs in upper arm amputees. 
 * 	    - This technology would allow for more intuitive control of upper limb prostheses and help users connect to the able bodies and natural world.
 *
 *  Brief explanation of my app
 *  	- I ask the user if they would like to use one of my features
 * 		- If they choose the first one, they are placed in a game where they have to find the doctor to replace their prosthetic before it breaks. (There is more information if you start it up.)
 * 		- In the second one I first ask the user if they would like me tell them more info about our subject. I then give them options and they choose if they want to make a prosthetic or test out the prosthetic they already have.
 * 		- In the third one I quiz the user on our subject and give them a score based on how well they did.
 * 		- Lastly I ask the user if they would like to run again
 */
		 
		// include at least 7 functions
			
			/* 1 */  int randomInRange(int, int);                             
			/* 2 */  int checkPowerUp();                                      
			/* 3 */  int checkMonster();                                      
			/* 4 */  char MapAndThings(char[5][9], int, int, int, int, int);  
			/* 5 */  int Game();                                              
			/* 6 */  void CheckUp();  // (only no input or output)
			/* 7 */  int checkAnswer(char, char);                        
			
			/* extra */  void Quiz();                                          



///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
/*|||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||




	MAIN FUNCTION CODE                                                                                                                                                               */

		// main
			
			int main(){
					
				int userChoice, q;
				char loopFull;
				do
				{
					printf("\033[2J\033[H");  //  looked up how to clear the screen and it doesn't completely clear but is close enough
					do
					{
						
						printf("Hello what would you like to do today?\n\t1. Prosthetic Game\n\t2. Prosthetic Check Up\n\t3. Quiz\n\t4. Quit\n");
						scanf("%d",&userChoice);
					
						switch (userChoice)
						{
							// game
								case 1:
									Game();
									q=1;
									break;
								
							// checkup
								case 2:
									CheckUp(); 
									q=1;
									break;
								
							// quiz
								case 3:
									Quiz();
									q=1;
									break;
									
							// quit
								case 4:
									return 0;
								
							// none
								default:
									printf("\nInvalid input. Try again.\n\n");
									q=0;
						}
						
					}while(q==0);
					
					printf("\n\nWould you like to choose another feature? (y or n)\n");
					scanf(" %c",&loopFull);
					
				
				}while(tolower(loopFull)=='y');  //  loop
					
			return 0;}




/*|||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||




	GAME FUNCTION CODE                                                                                                                                                               */

		// make random numbers
			
			int randomInRange(int min, int max){
				return min + rand() % (max - min + 1); // looked this up (needed to create a random number whithin the boundries given)
			}

		// check if there is a power-up

			int checkPowerUp(){
				int chance = randomInRange(1, 100);
				
				// user does encounter power-up so it does enter the if statement in Game()
					if (chance <= 20) // 20% chance of power-up occurrence
						{return 1;} 
				
				// user doesn't encounter power-up so it doesnt enter the if statement in Game()
					else
						{return 0;} 
			}

		// check if there is a monster

			int checkMonster(){
				
				int chance = randomInRange(1, 100);
				
				// user does encounter monster so it does enter the if statement in Game()
					if (chance <= 20) // 20% chance of monster encounter
						{return 1;} 
						
				// user doesn't encounter monster so it doesn't enter the if statement in Game()
					else
						{return 0;} 
			}
		
		// displays all of the choices and information for the user and records their choice
		
			char MapAndThings(char map[5][9], int prostheticDurability, int steps, int points, int x, int y){
				
				// print map
					for (int i=0;i<5;i++){
						for (int j=0;j<9;j++){
							printf(" %c",map[i][j]);
						}
					printf("\n");
					}
				
				// valuable information
					printf("\nCurrent Prosthetic Durability: %d\n", prostheticDurability);
					printf("Number of Steps Taken: %d\n", steps);
					printf("Number of Points: %d\n", points);
					printf("Your Position: (%d, %d)\n\n", x, y);

				// asks user what they decide
					char choice;
					
					printf("\nChoose your action:\n\n");
					printf("\t\'w\'. Move up\n");
					printf("\t\'s\'. Move down\n");
					printf("\t\'a\'. Move left\n");
					printf("\t\'d\'. Move right\n");
					printf("\t\'r\'. Rest and repair the prosthetic\n");
					printf("\t\'q\'. Quit the game\n");
						
					scanf(" %c", &choice);
				
				return(choice);
			}

		// main game

			int Game(){
				
				printf("\033[2J\033[H");  //  looked up clear screen
				
				// get and display map
					FILE *pMap;
					pMap=fopen("map.txt","r");
					char map[5][9];
					int pR=2,pC=4;
					for (int i=0;i<5;i++){
						for (int j=0;j<9;j++){
							fscanf(pMap," %c",&map[i][j]);
						}
					}
				
				srand(time(NULL)); // Looked this up (is needed so it isn't the same random number every time)

				int please=1;
				int steps = 0; 
				int points = 0;
				int x = 0, y = 0; 
				int prostheticDurability = 100; 
				int finishX = randomInRange(-2, 2); 
				int finishY = randomInRange(-2, 2); 
				char randlet;
				
				// instrunctions
					printf("Welcome to Prosthetic Adventure!\n\n");
					printf("You are stuck on a 5x5 area and need to get to the doctors but don't know where it is!\n");
					printf("This is so you can get a new prosthetic that doesn't wear down as easily!\n");
					printf("Get there without breaking your current prosthetic while also avoiding monsters and picking up power-ups!\n");
					printf("You are the O on the map and where you have been are the Xs\n");
					printf("\nEnter any single letter to start: ");
					scanf(" %c", &randlet);
					printf("\033[2J\033[H");  //  looked up clear screen
				
				char choice;
				
				// while not dead
					while (prostheticDurability > 0){
						choice = MapAndThings(map, prostheticDurability, steps, points, x, y);
						
						printf("\033[2J\033[H");
						
							switch(choice){
								
								// forward
									case 'w':
										if (y < 2){
											y++;
											steps++;
											points++;
											map[pR][pC]='X';
											pR-=1;
											map[pR][pC]='O';
											please=1;
										} 
										else{
											printf("\nYou can't move up further!\n");
											please=0;
										}
										break;
								
								// backward
									case 's':
										if (y > -2){
											y--;
											steps++;
											points++;
											map[pR][pC]='X';
											pR+=1;
											map[pR][pC]='O';
											please=1;
										}
										else{
											printf("\nYou can't move down further!\n");
											please=0;
										}
										break;
									
								// left
									case 'a':
										if (x > -2){
											x--;
											steps++;
											points++;
											map[pR][pC]='X';
											pC-=2;
											map[pR][pC]='O';
											please=1;
										}
										else{
											printf("\nYou can't move left further!\n");
											please=0;
										}
										break;
									
								// right
									case 'd':
										if (x < 2){
											x++;
											steps++;
											points++;
											map[pR][pC]='X';
											pC+=2;
											map[pR][pC]='O';
											please=1;
										}
										else{
											printf("\nYou can't move right further!\n");
											please=0;
										}
										break;
									
								// rest and repair
									case 'r':
										prostheticDurability=prostheticDurability+30; // Repair the prosthetic
										if(prostheticDurability>=100){
											prostheticDurability=100;
										}
										
										printf("\nYou rested and repaired the prosthetic.\n\n");
										please=1;
										
										break;
										
								// quit
									case 'q':
										printf("\033[2J\033[H");  //  looked up clear screen
										
										for (int i=0;i<5;i++)
										{
											for (int j=0;j<9;j++)
											{
												printf(" %c",map[i][j]);
											}
										printf("\n");
										}
										
										printf("\n\nThanks for playing! Your journey ended after %d steps and %d points.\n", steps, points);
									
										return 0;
										
								// invalid choice
									default:
										printf("\nInvalid choice! Please choose again.\n\n");
										please=0;
							}

						if(please!=0){
							prostheticDurability=prostheticDurability-randomInRange(5, 15); // Simulate wear and tear

							// Check for power-up occurrence
								if (checkPowerUp()){
									int powerUpType = randomInRange(1, 2); // 1 for extra points, 2 for self-healing prosthetic
									switch(powerUpType){
										
										case 1:
											printf("\nYou found a power-up! It added 5 extra points to your journey!\n");
											points=points+5;
											break;
											
										case 2:
											printf("\nYou found a power-up! Your prosthetic heals itself!\n");
											prostheticDurability += randomInRange(10, 20);
											if (prostheticDurability > 100) // Cap the durability at 100
												prostheticDurability = 100;
											break;
									}
								}

							// Check for monster encounter
								if (checkMonster()){
									int damage = randomInRange(20, 30); // Damage caused by the monster
									printf("\nOh no! You encountered a monster! It damaged your prosthetic by %d points!\n", damage);
									prostheticDurability=prostheticDurability-damage;
								}

							// Check if player reached the finish point
								if (x == finishX && y == finishY){
									for (int i=0;i<5;i++){
										for (int j=0;j<9;j++){
											printf(" %c",map[i][j]);
										}
									printf("\n");
									}
									
									printf("\n\nCongratulations! You've reached the doctors!\n");
									printf("Your journey ended after %d steps and %d points.\n", steps, points);
									
									return 0;
								}
							
							// check if player prosthetic dies
								if (prostheticDurability <= 0){
									for (int i=0;i<5;i++){
										for (int j=0;j<9;j++){
											printf(" %c",map[i][j]);
										}
										
									printf("\n");
									}
									
									printf("\n\nOh no! Your prosthetic broke. Game over!\n");
									printf("You managed to take %d steps and get %d points before the prosthetic broke.\n", steps, points);
								
									return 0;
								}
						}
					}

				return 0;
			}




/*|||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||




	PROSTHETIC CHECKUP FUNCTION CODE*/

		// main checkup
			
			void CheckUp(){
						
				char Know, randlet;
				
				printf("\033[2J\033[H");		
				printf("Welcome to your check up!\n\n");
				
				// ask if want to know more about microelectrodes
					do{
						printf("Before we begin, would you like to know more about microelectrode prosthetics (y or n):\n");
						scanf(" %c", &Know);
						
					}while(tolower(Know)!='y' && tolower(Know)!='n');

				// print if they say yes or no
					switch (tolower(Know)){
						
						case 'y':
							printf("Microelectrode prostheses use a series of microelectrode arrays to read nerve signals from a user and create a more lifelike prosthetic.\n");
							printf("The electrodes (implanted in the arm) read electrical pulses from the brain, and transmit the signal to an AI interface.\n");
							printf("This interface uses a constantly expanding library to predict the intended movement and create a movement mapping, which is sent back to the prosthetic for execution via bluetooth signal.\n");
							printf("In terms of sustainability, microelectrode prosthetics focus on following the UN sustainability goal of enhancing quality of life for users. Prosthetic abandonment rates prior to development of microelectrode prosthetics was above 50%%.\n");
							printf("Microelectrode prosthetics have a higher ease of use, are more adaptable, and give the users a lifelike, quickly responsive prosthetic arm.\n\n");
						
						case 'n':
							printf("\nEnter any single letter to begin your appointment: ");
							scanf(" %c", &randlet);
							printf("\033[2J\033[H");
							
							break;
					}
				
				int option;

				do{
					// display menu options
						printf("1. Build a new prosthetic\n");
						printf("2. Evaluate prosthetic reaction performance\n");
						printf("3. Test range of motion\n");
						printf("4. Exit\n");
						
						printf("\nChoose an option: ");
						scanf("%d", &option);
						printf("\033[2J\033[H");

					// perform actions based on user's choice
						switch (option) {
							
							// create new prosthetic
								case 1:
									printf("Let's create a new prosthetic.\n");
									
									char isBeforeElbow;
									double armLength, stumpDiameter, stumpLength;

									// ask user questions
										do{
											printf("Was your arm amputated before the elbow? (y or n): ");
											scanf(" %c", &isBeforeElbow);
										}while(tolower(isBeforeElbow)!='y' && tolower(isBeforeElbow)!='n');
										
										printf("Enter your ideal arm length (in inches): ");
										scanf("%lf", &armLength);
										
										printf("Enter the length of your stump (in inches): ");
										scanf("%lf", &stumpLength);
										
										printf("Enter the diameter of your stump (in inches): ");
										scanf("%lf", &stumpDiameter);
										
										printf("\033[2J\033[H");
									
									printf("Prosthetic created successfully!\n");

									// display a representation of the prosthetic
										if (tolower(isBeforeElbow)=='y'){
											printf("  ||----------||                           \n");
											printf("  ||   ____   ||                           \n");
											printf("  ||  /    \\  ||                          \n");
											printf("  ||  |    |  ||                           \n");
											printf("  ||  \\____/  ||                          \n");
											printf("  ||          ||                           \n");
											printf("  ||          ||                           \n");
											printf("  ||          ||                           \n");
											printf("  ||          ||            _______        \n");
											printf("  ||          ||___________|   ____|___    \n");
											printf("  ||   _                  _      ______|   \n");
											printf("  ||  / \\                / \\     ______| \n");
											printf("  ||  \\_/                \\_/     ______| \n");
											printf("  ||___________________________________| \n\n");
										}
										
										else if (tolower(isBeforeElbow)=='n'){
											printf("                           _______         \n");
											printf("    ______________________|   ____|___     \n");
											printf("   |                    _       ______|    \n");
											printf("   |                   / \\      ______|   \n");
											printf("   |                   \\_/      ______|   \n");
											printf("   |__________________________________|  \n\n");
										}

									double prostheticLength;
									
									// display prosthetic length
										prostheticLength = armLength-stumpLength;
										printf("\nLength of prosthetic: %.2f inches\n\n\n", prostheticLength);
										
									break;
								
							// see reaction time
								case 2:
										printf("Let's evaluate your prosthetic performance.\n");
										char currentWord[100];
										int currentTime;

										printf("Have someone time you or set up a timer yourself!\n");
										
										printf("\nType the word \"Awesome\" using your prosthetic hand:\n");
										scanf("%s", currentWord);
										
										printf("\nHow long did it take you in seconds?\n");
										scanf("%d", &currentTime);

										if (currentTime>=10){
											printf("\nThat was way too slow.\n");
											printf("We're going to need to check on the microelectrodes.\n\n\n");
										}
										
										else{
											printf("That was great!\nThe reaction time seems to be at a pretty good level.\n\n\n");
										}
										
									break;

							// test rom
								case 3:
									printf("Let's test the range of motion of your prosthetic arm.\n");
									double maxAngle;

									printf("Enter the maximum angle you can comfortably reach (in degrees): ");
									scanf("%lf", &maxAngle);

									if (maxAngle >= 90){
										printf("Your range of motion is excellent!\n\n\n");
									}
									
									else if (maxAngle >= 45){
										printf("Your range of motion is good, but there's room for improvement. We'll work on it.\n\n\n");
									}
									
									else{
										printf("Your range of motion needs improvement. We'll work on it.\n\n\n");
									}
									
								break;
							
							// if quit
								case 4:
									printf("Thank you for using the prosthetic checkup system.\n");
									break;
							// default
								default:
									printf("Invalid option. Please try again.\n");
						}
						
				}while (option != 4);

				char Satisfaction;
				
				// ask if user is satisfied
					do{
						printf("\nYour appointment is complete! Are you satisfied with today's tests and results (y or n)\n");
						scanf(" %c", &Satisfaction);
					}while (tolower(Satisfaction)!='y' && tolower(Satisfaction)!='n');
				
				// if answer is yes or no
					if(tolower(Satisfaction)=='y'){
						printf("\033[2J\033[H");
						printf("Glad to hear it! Have a nice day!");
					}
					
					else{
						printf("\033[2J\033[H");
						printf("Please let us know how we can improve your experience for your next visit on our website www.MicroPros.com. Have a nice day!");
					}
			}




/*|||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||




	QUIZ FUNCTION CODE                                                                                                                                                               */


		// right or wrong
		
			int checkAnswer(char userAnswer, char correctAnswer){
				return toupper(userAnswer) == toupper(correctAnswer);
				
			}

		// main quiz
		
			void Quiz(){
				
				printf("\033[2J\033[H");		
				printf("Welcome to the quiz!\n\n");
				
				int score = 0; // Variable to store user's score
				char userAnswer, correctAnswer;

				// question 1        
					printf("\nQuestion 1:\n");
					printf("How many amputees abandon their prostheses?\n\n");
					printf("A) 40-50%%\nB) 0-10%%\nC) 20-30%%\nD) 80-90%%\n\n");

					correctAnswer = 'a';

					// users answer
						printf("Your answer: ");
						scanf(" %c", &userAnswer);

					printf("\033[2J\033[H");

					// check if the answer is correct
						if (checkAnswer(userAnswer, correctAnswer)) {
							printf("Correct!\n\n\n");
							score++; 
						} else {
							printf("Incorrect!\n\n\n");
						}
					
				// question 2        
					printf("Question 2:\n");
					printf("Are microelectrode prosthetics currently available for general use?\n\n");
					printf("A) True\nB) False\n\n");

					correctAnswer = 'b';

					// users answer
						printf("Your answer: ");
						scanf(" %c", &userAnswer);

					printf("\033[2J\033[H");

					// check if the answer is correct
						if (checkAnswer(userAnswer, correctAnswer)) {
							printf("Correct!\n\n\n");
							score++; 
						} else {
							printf("Incorrect!\n\n\n");
						}
					
				// question 3        
					printf("Question 3:\n");
					printf("What is the purpose of the \"Data Glove\" in the \"Hand Gesture Matching Task\"?\n\n");
					printf("A) To serve as a temporary hand prosthetic\nB) To only record muscle movements for the ai agent\nC) To only record neural movements for the ai agent\nD) To record nerve and muscle movements on a subject\'s able hand in order to code the AI agent.\n\n");

					correctAnswer = 'd';

					// users answer
						printf("Your answer: ");
						scanf(" %c", &userAnswer);

					printf("\033[2J\033[H");

					// check if the answer is correct
						if (checkAnswer(userAnswer, correctAnswer)) {
							printf("Correct!\n\n\n");
							score++; 
						} else {
							printf("Incorrect!\n\n\n");
						}
					
				// question 4        
					printf("Question 4:\n");
					printf("What is the average human response time for visual stimuli?\n\n");
					printf("A) 1.17 seconds\nB) 0.17 seconds\nC) 0.25 seconds\nD) 0.83 seconds\n\n");

					correctAnswer = 'c';

					// users answer
						printf("Your answer: ");
						scanf(" %c", &userAnswer);

					printf("\033[2J\033[H");

					// check if the answer is correct
						if (checkAnswer(userAnswer, correctAnswer)) {
							printf("Correct!\n\n\n");
							score++; 
						} else {
							printf("Incorrect!\n\n\n");
						}
					
				// question 5        
					printf("Question 5:\n");
					printf("How many DOF (degrees of freedom) are available with the microelectrode hand prostheses?\n\n");
					printf("A) 2\nB) 6\nC) 10\nD) 5\n\n");

					correctAnswer = 'b';

					// users answer
						printf("Your answer: ");
						scanf(" %c", &userAnswer);

					printf("\033[2J\033[H");

					// check if the answer is correct
						if (checkAnswer(userAnswer, correctAnswer)) {
							printf("Correct!\n\n\n");
							score++; 
						} else {
							printf("Incorrect!\n\n\n");
						}
					
				// question 6        
					printf("Question 6:\n");
					printf("What components warranted a \"success\" in the \"Hand Gesture Matching Task\"?\n\n");
					printf("A) 4 DOF within 4 seconds\nB) 6 DOF within 3 seconds\nC) 4 DOF within 1 second\nD) 10 DOF within 2 seconds\n\n");

					correctAnswer = 'b';

					// users answer
						printf("Your answer: ");
						scanf(" %c", &userAnswer);

					printf("\033[2J\033[H");

					// check if the answer is correct
						if (checkAnswer(userAnswer, correctAnswer)) {
							printf("Correct!\n\n\n");
							score++; 
						} else {
							printf("Incorrect!\n\n\n");
						}
					
				// question 7        
					printf("Question 7:\n");
					printf("How does the machine learning algorithm communicate with the prosthetic?\n\n");
					printf("A) Electrical wiring\nB) Vibrations\nC) WiFi\nD) Bluetooth\n\n");

					correctAnswer = 'd';

					// users answer
						printf("Your answer: ");
						scanf(" %c", &userAnswer);

					printf("\033[2J\033[H");

					// check if the answer is correct
						if (checkAnswer(userAnswer, correctAnswer)) {
							printf("Correct!\n\n\n");
							score++; 
						} else {
							printf("Incorrect!\n\n\n");
						}
				   
				// display final score
					printf("Your final score is: %d/7\n", score);
				
			}
   
