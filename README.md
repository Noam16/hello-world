#include <iostream>
#include <ctime> // Needed for the true randomization
#include <windows.h> // בשביל ההשהיה
using namespace std;
enum Status {empty, ix, igul};


// הצהרות //

Status victory(Status board[3][3]);

/// // /// /






class location{
public:
	int x, y;
};

location get_From_user(Status stat, Status board[3][3]){
	location l;
	do{
		if (stat == ix) cout<<"Ix turn. Enter x, y:  ";
		else cout<<"Igul turn. Enter x, y:  ";
		cin >>l.x >>l.y;

	} while(board[l.x][l.y] !=empty || l.x>2 || l.y>2 || l.x<0 || l.y<0);

	return l;
}



location random_empty_corner(Status board[3][3]){
	int randC;
	location l;
	/*
	if (board[0][0]!=empty && board[2][0]!=empty && board[2][2]!=empty && board[0][2]!=empty){ // בדיקה שאם אין פינה ריקה שלא ימשיך לנצח
		l.x=-1;
		l.y=-1;
		return l;
	}
	*/

	do{
		randC = rand() % 4;
		switch(randC){
		case 0: {l.x=0; l.y=0;    break;}
		case 1: {l.x=2; l.y=0;    break;}
		case 2: {l.x=2; l.y=2;    break;}
		case 3: {l.x=0; l.y=2;    break;}
		}
	}while(board[l.x][l.y]!=empty);

		return l;
	}

location random_empty(Status board[3][3],Status stat){
	// ראשית מחפשרנדומלי עםפוטנציאל לניצחון, אם אין אז מביא רנדומלי כלשהו
	location rand_location;

	Status DemoBoard[3][3];
	for(int y=0; y<3; y++){
		for(int x=0; x<3; x++){
			DemoBoard[x][y] = board[x][y];
		}
	}

	// בדיקת מקום שיש לו פוטנציאל לניצחון - אם נשים איפה שהוא ואח"כ יש מקום שאם נשים בו ננצח
	bool find=false;
	for(int y=0; y<3; y++){
		for(int x=0; x<3; x++){
			if(DemoBoard[x][y]!=empty) continue;
			DemoBoard[x][y]=stat;

			for(int y1=0; y1<3; y1++){
				for(int x1=0; x1<3; x1++){
					if(DemoBoard[x1][y1]!=empty) continue;
					DemoBoard[x1][y1]=stat;
					if(victory(DemoBoard)==stat){
						rand_location.x=x;
						rand_location.y=y;
						return rand_location;
					}
					DemoBoard[x1][y1]=empty;
				}
			}
			DemoBoard[x][y]=empty;
		}
	}


	do{
		rand_location.x = rand() % 3;
		rand_location.y= rand() % 3;
	}while(board[rand_location.x][rand_location.y]!=empty);
	return rand_location;
}

location if_there_is_a_key_location(Status board[3][3], Status stat){ // מיקום מפתח זה מיקום שאם צד מסוים ישים בו יהיו לו שתי מקומות לנצח בהם
	// הנחה: אין ניצחון מיידי אפשרי לאף צד
	// אם אין מקום מפתח - יחזיר -1 -1      - הכוונה למינוס


	location loc;
	bool find=false;
	for(loc.y=0; loc.y<3; loc.y++){
		for(loc.x=0; loc.x<3; loc.x++){ // נבדוק עבור כל מיקום האם הוא מיקום מפתח
			if(board[loc.x][loc.y]!=empty) continue;
			/// יש שלוש בדיקות- בדיקת אלכסונים, שורות ועמודות

			if(loc.x==0 && loc.y==0){ //00
				if(((board[1][0]==stat&&board[2][0]==empty) || (board[2][0]==stat&&board[1][0]==empty))   &&
					((board[0][1]==stat&&board[0][2]==empty) || (board[0][2]==stat&&board[0][1]==empty))){
					find = true;
					break;
				}
				continue;
			}
			if(loc.x==1 && loc.y==0){ //10
				if(((board[0][0]==stat&&board[2][0]==empty) || (board[2][0]==stat&&board[0][0]==empty)) &&
					((board[1][1]==stat&&board[1][2]==empty) || (board[1][2]==stat&&board[1][1]==empty))){
					find = true;
					break;
				}
				continue;
			}
			if(loc.x==2 && loc.y==0){ //20
				if(((board[0][0]==stat&&board[1][0]==empty) || (board[1][0]==stat&&board[0][0]==empty)) &&
					((board[2][1]==stat&&board[2][2]==empty) || (board[2][2]==stat&&board[2][1]==empty))){
					find = true;
					break;
				}
				continue;
			}
			if(loc.x==0 && loc.y==1){  //01
				if(((board[1][1]==stat&&board[2][1]==empty) || (board[2][1]==stat&&board[1][1]==empty)) &&
					((board[0][0]==stat&&board[0][2]==empty) || (board[0][2]==stat&&board[0][0]==empty))){
					find = true;
					break;
				}
				continue;
			}
			if(loc.x==1 && loc.y==1){  //11
				if(((board[0][1]==stat&&board[2][1]==empty) || (board[2][1]==stat&&board[0][1]==empty)) &&
					((board[1][0]==stat&&board[1][2]==empty) || (board[1][2]==stat&&board[1][0]==empty))){
					find = true;
					break;
				}
				continue;
			}
			if(loc.x==2 && loc.y==1){  //21
				if(((board[0][1]==stat&&board[1][1]==empty) || (board[1][1]==stat&&board[0][1]==empty)) &&
					((board[2][0]==stat&&board[2][2]==empty) || (board[2][2]==stat&&board[2][0]==empty))){
					find = true;
					break;
				}
				continue;
			}
			if(loc.x==0 && loc.y==2){ //02
				if(((board[1][2]==stat&&board[2][2]==empty) || (board[2][2]==stat&&board[1][2]==empty)) &&
					((board[0][0]==stat&&board[0][1]==empty) || (board[0][1]==stat&&board[0][0]==empty))){
					find = true;
					break;
				}
				continue;
			}
			if(loc.x==1 && loc.y==2){  //12
				if(((board[0][2]==stat&&board[2][2]==empty) || (board[2][2]==stat&&board[0][2]==empty)) &&
					((board[1][0]==stat&&board[1][1]==empty) || (board[1][1]==stat&&board[1][0]==empty))){
					find = true;
					break;
				}
				continue;
			}
			if(loc.x==2 && loc.y==2){  //22
				if(((board[0][2]==stat&&board[1][2]==empty) || (board[1][2]==stat&&board[0][2]==empty)) &&
					((board[2][0]==stat&&board[2][1]==empty) || (board[2][1]==stat&&board[2][0]==empty))){
						
					find = true;
					break;
				}
				continue;
			}





		}
		if (find==true) break;
	}
	if(find==false){
		loc.x=-1;
		loc.y=-1;
	}
	return loc;
}


location win_location_for_stat(Status board[3][3], Status stat){
	// אם הצבע האמור לא יכול לנצח, יחזיר  -1 -1
	location l;
	Status DemoBoard[3][3];
	for(int y=0; y<3; y++){
		for(int x=0; x<3; x++){
			DemoBoard[x][y] = board[x][y];
		}
	}

	bool stop=false;
	for(int y=0; y<3; y++){
		for(int x=0; x<3; x++){
			if(DemoBoard[x][y]!=empty) continue;
			DemoBoard[x][y]=stat;
			if(victory(DemoBoard)==stat){
				stop=true;
				l.x=x; l.y=y;
				// DemoBoard[x][y]=empty;  // להוסיף השורה הזאת אם רוצים לוותר על הצורך במערך העזרולעבוד ישר על המערך הישיר
				break; 
			}
			DemoBoard[x][y]=empty;
		}
		if (stop==true) break;
	}
	if (stop==false){
		l.x=-1;
		l.y=-1;
	}
	return l;
}


location get_From_AI(int turn, Status board[3][3], Status stat){ // מקבל את התור מ1-9
	if (stat == ix) cout<<"Ix turn. AI...\n";
	else cout<<"Igul turn. AI...\n";
	Sleep(1000*1.1); // השהייה של שנייה וקצת
	location l;
	location a_check;
	int randC;
	Status enemy_stat = (stat==ix)? igul : ix;
	Status DemoBoard[3][3];
	for(int y=0; y<3; y++){
		for(int x=0; x<3; x++){
			DemoBoard[x][y] = board[x][y];
		}
	}



	switch (turn){
	case 1:{ l= random_empty_corner(board); break;}
	case 2:{if (board[1][1]==empty){l.x=1; l.y=1; break;}
		   else { l= random_empty_corner(board); break;}
		   }
	case 3:{
		if(board[0][0] == stat){
			if (board[2][2]==empty) {l.x=2; l.y=2; break;}
			else{
				randC = rand() % 2;
				if (randC==0){l.x=0; l.y=2; break;}
				else {l.x=2; l.y=0; break;}
			}
		}
		else if(board[2][0] == stat){
			if (board[0][2]==empty) {l.x=0; l.y=2; break;}
			else{
				randC = rand() % 2;
				if (randC==0){l.x=2; l.y=2; break;}
				else {l.x=0; l.y=0; break;}
			}
		}
		else if(board[2][2] == stat){
			if (board[0][0]==empty) {l.x=0; l.y=0; break;}
			else{
				randC = rand() % 2;
				if (randC==0){l.x=0; l.y=2; break;}
				else {l.x=2; l.y=0; break;}
			}
		}
		else if(board[0][2] == stat){
			if (board[2][0]==empty) {l.x=2; l.y=0; break;}
			else{
				randC = rand() % 2;
				if (randC==0){l.x=2; l.y=2; break;}
				else {l.x=0; l.y=0; break;}
			}
		}
		break;
		   }
	case 4:{if((board[0][0]==enemy_stat&& board[2][2]==enemy_stat) ||
			   (board[0][2]==enemy_stat&& board[2][0]==enemy_stat) ){
				   randC = rand() % 4;
				   // אם שתי פינות מנוגדות תפוסות על ידי היריב - כמובן שהאמצע תפוס כבר על ידינו - נשים באחד מ4 ההמקומות שהם לא פינות
				   switch (randC){
				   case 0: { l.x=1; l.y=0; break; }
				   case 1: { l.x=2; l.y=1; break; }
				   case 2: { l.x=1; l.y=2; break; }
				   case 3: { l.x=0; l.y=1; break; }
				   }
				   break;
		   }

		   // מכיוון שזה התור הרביעי אז וודאי שאנחנו עדיין לא יכולים לעשות ניצחון

		   //עכשיו בדיקה אם היריב יכול לעשות ניצחון בתור הבא שלו:
		   a_check = win_location_for_stat(board, enemy_stat);
		   if(a_check.x != -1){
			   l=a_check;
			   break;
		   }


		   // לא נעשה בדיקהת מקום מפתח לצד שלנו כי זה רק תור רביעי
		   // עכשיו אם סימן של היריב הוא משהו כמו 01 ו 20  אז צריך לחסום אותו ב 00.     משמעות: בדיקת מקום מפתח. 
		   a_check = if_there_is_a_key_location(board,enemy_stat);
		   if(a_check.x != -1){
			   l=a_check;
			   break;
		   }

		   l=random_empty(board, stat);
		   break;
		   }
	case 5: case 6: case 7: case 8:case 9:{
		a_check = win_location_for_stat(board,stat);
		if(a_check.x != -1){
			l=a_check;
			break;
		}
		a_check = win_location_for_stat(board, enemy_stat);
		if(a_check.x != -1){
			l=a_check;
			break;
		}


		
		// עכשיו אם סימן של היריב הוא משהו כמו 01 ו 20  אז צריך לחסום אותו ב 00.     משמעות: בדיקת מקום מפתח.
		a_check = if_there_is_a_key_location(board,stat);
		if(a_check.x != -1){
			l=a_check;
			break;
		}
		a_check = if_there_is_a_key_location(board,enemy_stat);
		if(a_check.x != -1){
			l=a_check;
			break;
		}

		l=random_empty(board, stat);
		break;
			}
	}

	return l;
}



void print(Status board[3][3]){
	int x, y;
	cout<<"\n   012\n\n";
	for (y=0; y<3; y++){
		cout<<y<<"  ";
		for(x=0; x<3; x++){
			if (board[x][y] == empty) cout<<(char)177;
			else if (board[x][y] == ix) cout<<"x";
			else if (board[x][y] == igul) cout<<"O";
		}
		cout<<"\n";
	}
	cout<<"\n";
}

Status victory(Status board[3][3]){
	if(board[0][0] != empty &&board[0][0]==board[1][0] && board[0][0]==board[2][0])return board[0][0];
	if(board[0][1] != empty &&board[0][1]==board[1][1] && board[0][1]==board[2][1])return board[0][1];
	if(board[0][2] != empty &&board[0][2]==board[1][2] && board[0][2]==board[2][2])return board[0][2];
	// 3 שורות 

	if(board[0][0] != empty &&board[0][0]==board[0][1] && board[0][0]==board[0][2])return board[0][0];
	if(board[1][0] != empty &&board[1][0]==board[1][1] && board[1][0]==board[1][2])return board[1][0];
	if(board[2][0] != empty &&board[2][0]==board[2][1] && board[2][0]==board[2][2])return board[2][0];
	// 3 עמודות

	if(board[0][0] != empty &&board[0][0]==board[1][1] && board[0][0]==board[2][2])return board[0][0];
	if(board[2][0] != empty &&board[2][0]==board[1][1] && board[2][0]==board[0][2])return board[2][0];
	// אלכסונים


	return empty; // אין עדיין ניצחון
}

void main(){
	srand( time(0)); 
	cout<<"**Wellcome to IX-Igul game by Noam and Daniel\n\n";
	Status board[3][3];
	Status vic;
	location l;
	bool user_first=true;
	int i=0, x, y;

	for (y=0; y<3; y++){ // איפוס המערך
		for (x=0; x<3; x++){
			board[x][y]=empty;
		}
	}


	print(board);
	char str[6];
	do{
		flushall(); // עבור קלט שיש בו רווחים
		cout<<"Are you first or second? (F/S):  ";
		cin>>str;
	}while(str[0]!='f' && str[0]!='F' && str[0]!='s' && str[0]!='S');


	while(i<9){
		if(str[0]=='f' || str[0]=='F') l = get_From_user(ix, board);
		else l = get_From_AI(i+1, board, ix);
		// l = (str[0]=='f' || str[0]=='F')? get_From_user(ix, board)  : get_From_AI(i+1, board, ix);  //  אני לא יודע למה .sלא עובד ב
		board[l.x][l.y] = ix;
		print(board);
		vic = victory(board);
		if (vic!=empty) break;
		i++;
		if (i>8) break;


		if(str[0]=='f' || str[0]=='F') l = get_From_AI(i+1, board, igul);
		else l = get_From_user(igul, board);
		//  l = (str[0]=='s' || str[0]!='S')?  get_From_user(igul , board)  :  get_From_AI(i+1, board, igul);  // אני לא יודע למה .sלא עובד ב
		board[l.x][l.y] = igul;
		print(board);
		vic = victory(board);
		if (vic!=empty) break;
		i++;

	}
	if (i==9) cout<<"\nA tie!!\n\n";
	else{
		if(vic==ix) cout<<"\nIx win!!\n\n";
		else cout<<"\nIgul win!!\n\n";
	}
}
