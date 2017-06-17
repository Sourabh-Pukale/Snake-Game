# Snake-Game

#include<iostream>
#include<conio.h>
#include<stdlib.h>
#include<dos.h>
#include<stdio.h>
#include<windows.h>
#include<unistd.h>
using namespace std;
bool gameOver;
const int width=60;
const int height=20;
int x,y,fruitX,fruitY,score;
int tailX[100],tailY[100],ltail;
enum sDirection{
    STOP=0,RIGHT,UP,DOWN,LEFT
};
sDirection dir;
void setUp(){
    gameOver=false;
    dir=STOP;
    x=width/2;
    y=height/2;
    fruitX=rand()%width;
    fruitY=rand()%height;
    score=0;
}
void draw(){
    system("cls");
    for(int i=0;i<width+2;i++)
        cout<<"#";
    cout<<endl;

    for(int i=0;i<height;i++){
        for(int j=0;j<width;j++){
            if(j==0)cout<<".";
            if(i==y && j==x)cout<<"0";
            else if(i==fruitY && j==fruitX)cout<<"$";
            else{
            	bool tprint=false;
            	for(int k=0;k<ltail;k++){
            		if(tailX[k]==j && tailY[k]==i){
            			cout<<"o";
            			tprint=true;
					}
				}
				if(!tprint)cout<<" ";
			}
            if(j==width-1)cout<<".";
        }
        cout<<endl;
    }


    for(int i=0;i<width+2;i++)
        cout<<"#";
    cout<<endl;
    cout<<"Score"<<score<<endl;
}
void input(){

    if(_kbhit()){
        switch(_getch()){
            case 'a':
                dir=LEFT;break;
            case 'd':
                dir=RIGHT;break;
            case 'w':
                dir=UP;break;
            case 's':
                dir=DOWN;break;
            case 'x':
                gameOver=true;break;
        }
    }
}
void logic(){
	int prevX=tailX[0];
	int prevY=tailY[0];
	int prev2X,prev2Y;
	tailX[0]=x;
	tailY[0]=y;
	for(int i=1;i<ltail;i++){
		prev2X=tailX[i];
		prev2Y=tailY[i];
		tailX[i]=prevX;
		tailY[i]=prevY;
		prevX=prev2X;
		prevY=prev2Y;		
	}
    switch(dir){
        case LEFT:
            x--;
            break;
        case RIGHT:
            x++;
            break;
        case DOWN:
            y++;
            break;
        case UP:
            y--;
            break;
        default:
            break;

    }
    //if(x>width || x<0 || y>height || y<0)gameOver=true;
    if(x>=width)x=0;else if(x<0)x=width-1;
    if(y>=height)y=0;else if(y<0)y=height-1;
	for(int i=0;i<ltail;i++)
		if(tailX[i]==x && tailY[i]==y)gameOver=true;
    if(x==fruitX && y==fruitY){
    	score+=10;
    	fruitX=rand()%width;
    	fruitY=rand()%height;
		ltail++;
	}
}
int main(){
    setUp();
    while(!gameOver){
        draw();
        input();
        logic();
        Sleep(200);  //This slows our game
    }
    getch();
    return 0;
}
