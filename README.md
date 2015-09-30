//61316444　西川　慶 
//オリジナルゲーム「バッティングストラックアウトゲーム」の作成


# sdglib.h
//sdglib.hを読み込む際に必要なコード


#ifndef __sdglib_h__
#define __sdglib_h__

//#include <math.h>
//#include <iostream>
//#include <stdexcept>
#include <string>
//#include <string.h>
//#include <GL/glut.h>

#if defined(WINDOWS) || defined(_WIN32) || defined(_WIN64)
#include <GL/glut.h>
#endif
#if defined(linux) || defined(__linux) || defined(__linux__)
#include <GL/glut.h>
#endif
#if defined(__APPLE__)
#include <GLUT/glut.h>
#endif

#ifndef M_PI
#define M_PI 3.14159265358979323846
#endif


class SDGLib {
public:
	SDGLib(int, int, std::string, double, double, double, double);
	//	~SDGLib();
	//void Before(void);
	//void After(void);
	//void ReDraw(void);
	void Start(void);
	void SetCursor(int);
	//void SetColor(float, float, float);
	//void DrawPoint(float, double, double);
	//void DrawLine(float, double, double, double, double);
	//void DrawCircle(float, double, double, double);
	//void DrawString(double, double, char *);
	void Display(void (*)(void));
	void Keyboard(void (*)(unsigned char, int, int));
private:
	int size_x, size_y;
	std::string title;
	double small_x, large_x, small_y, large_y;
};

void SDGLib::Display(void (*func)(void)) {
	glutDisplayFunc(func);
};
void SDGLib::Keyboard(void (*func)(unsigned char a, int b, int c)) {
	glutKeyboardFunc(func);
};

SDGLib::SDGLib(int size_x=600, int size_y=600, std::string title="No Title", double small_x=-300.0, double large_x=-300.0, double small_y=-300.0, double large_y=300.0) {
	int i=1;
	char *cv[1]={"SD"};
	glutInit(&i,cv);
	glutInitDisplayMode(GLUT_RGB | GLUT_DEPTH | GLUT_DOUBLE);
	glutInitWindowSize(size_x, size_y);
	glutInitWindowPosition(0, 0);
	glutCreateWindow(title.c_str());
	glClearColor(1.0, 1.0, 1.0, 1.0);
	gluOrtho2D(small_x, large_x, small_y, large_y);
	glutSetCursor(GLUT_CURSOR_HELP);
}
void SDGLib::SetCursor(int cursor_type){
	glutSetCursor(cursor_type);
}
void SDGLib::Start(void){
	glutMainLoop();
}


namespace SDGLibF {
	void Before(void){
		glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
	}
	void After(void){
		glutSwapBuffers();
	}
	void SetCursor(int cursor_type){
		glutSetCursor(cursor_type);
	}
	void SetColor(float r, float g, float b){
		glColor3f((GLfloat)r, (GLfloat)g, (GLfloat)b);
	}
	void DrawPoint(float size=10, double x=0, double y=0){
		glPointSize((GLfloat)size);
		glBegin(GL_POINTS);
		glVertex2d((GLdouble)x, (GLdouble)y);
		glEnd();
	}
	void DrawLine(float width=2, double x1=100, double y1=100, double x2=-100, double y2=-100){
		glLineWidth((GLfloat)width);
		glBegin(GL_LINES);
		glVertex2d((GLdouble)x1,(GLdouble)y1);
		glVertex2d((GLdouble)x2,(GLdouble)y2);
		glEnd();
	}
	void DrawCircle(float width, double x, double y, double r){
		int i;
		GLdouble x1, x2, y1, y2;

		for(i=0; i<360; i++){
			x1=x+r*sin(M_PI*i/180.0); y1=y+r*cos(M_PI*i/180.0);
			x2=x+r*sin(M_PI*(i+1)/180.0); y2=y+r*cos(M_PI*(i+1)/180.0);
			DrawLine((GLfloat)width, x1, y1, x2, y2);
		}
	}
	void DrawString(double x, double y, std::string s){
		glRasterPos2f(x, y);
		for(unsigned int i=0; i<s.length(); i++){
			glutBitmapCharacter(GLUT_BITMAP_TIMES_ROMAN_24, s.at(i));
		}
	}
	void ReDraw(void){
		glutPostRedisplay();
	}
	void IdleFunc(void (*func)(void)) {
		glutIdleFunc(func);
	}
}
# endif




//本文

#include <windows.h>
#include <math.h>
#include <iostream>
#include <stdexcept>
#include <string>
#include <string.h>
#include "sdglib.h"


//変数・関数の定義

static double xx, yy, rr, z, P, mag;
static int p, q;
int m(0);

//sprintf_sで代入できるように配列構造にした。
char buff1[100], buff2[100];

char string_s[] = "60";
char string_s1[] = "100";
char string_s2[] = "30";
char string_s3[] = "10";
char string_s4[] = "80";
char string_s5[] = "50";


//ラジアンから°に変換する動作を多用するために、この動作を関数化した。
double change(double a){
	double b = a*3.14 / 180;
	return b;

}

//同様に円内の領域を表すときに使うx^2+y^2<r^2の式を作るのを簡略化するためにこの動作を関数化した。
double en(double b, double c){
	double d = b*b + c*c;
	return d;
}




//ディスプレイに関する動作

void displayfunc(){
	using namespace SDGLibF;
	char buff[100];

	// Set drawing plane
	Before();


	//グローバル変数で設定した変数を用いて、動的に変化する円（ボール）を作った。
	SetColor(1.0, 0.0, 0.0);
	DrawCircle(1.0, xx, yy, rr);

	//バットの作成
	SetColor(0.0, 1.0, 1.0);
	DrawLine(3.0, 260, -220, 260 + 80 * cos(z), -220 + 80 * sin(z));
	DrawLine(3.0, 260, -220, 260 + 80 * cos(z - change(10)), -220 + 80 * sin(z - change(10)));
	DrawLine(3.0, 260 + 80 * cos(z), -220 + 80 * sin(z), 260 + 80 * cos(z - change(10)), -220 + 80 * sin(z - change(10)));
	DrawCircle(3.0, 260, -220, 5.0);

	//左下に点数表示
	sprintf_s(buff, "Your score is %d", p);
	DrawString(10, -290, buff);

	//的のバルーンとその点数の描画

	SetColor(1.0, 0.0, 0.0);
	DrawCircle(3.0, 30, 240, 30);
	DrawString(20, 230, string_s);

	SetColor(0.0, 1.0, 1.0);
	DrawCircle(3.0, 100, 270, 10);
	DrawString(90, 280, string_s1);

	SetColor(0.0, 1.0, 0.0);
	DrawCircle(3.0, 200, 250, 50);
	DrawString(190, 240, string_s2);

	SetColor(0.0, 0.0, 1.0);
	DrawCircle(3.0, 510, 230, 70);
	DrawString(500, 220, string_s3);

	SetColor(1.0, 1.0, 0.0);
	DrawCircle(3.0, 380, 260, 20);
	DrawString(370, 250, string_s4);

	SetColor(1.0, 0.0, 1.0);
	DrawCircle(3.0, 300, 250, 40);
	DrawString(290, 240, string_s5);

	SetColor(1.0, 0.0, 0.0);
	DrawString(30, -270, buff1);

	SetColor(1.0, 1.0, 0.0);
	sprintf_s(buff2, "Level:%d", q);
	DrawString(400, -270, buff2);

	// Set drawn plane
	After();
}



//暇なときに行う動作

void simulation(void){
	using namespace SDGLibF;
	static int count = 0;
	static double y = 0.0, vy = 0.0, ay = 0.0, x = 300.0, vx = 0.0;
	static double dt = 0.005, g = 9.8;
	static double A, B, C, D, E, F;

	//運動の式（バットに当たるまでは重力加速度gにしたがってy方向に加速し、跳ね返り後はバットの角度に応じてまっすぐ進む）  
	x = x + vx;
	ay = -g;
	y = y + vy*dt;
	vy = vy + ay*dt;
	xx = x; yy = y*mag;

	//キーボード入力によりPの値が変化し、それによってzの値が増していき、バットの回転が始まる。ディスプレイの更新を利用して自動回転しているように見せている。
	z += P;

	//バットがx軸に対して70°の地点で止まるための設定
	if (z > change(70))P = 0.0;

	//バットの打撃による跳ね返りの条件。yyの座標は相似を考えている。詳しくは提出レポートに記載する。
	if (xx == 300 && yy < -220 + 80 * (40 / (80 * cos(z))) * sin(z)) vx = cos(z + change(90)), vy = 10 * sin(z + change(90)), g = 0.0;

	//左下のコメント欄
	if (xx > 600.0 || yy > 300.0 || xx < 0.0) sprintf_s(buff1, "missed!"), IdleFunc(NULL);
	if (en(xx - 30, yy - 240) < 900)p = 60, sprintf_s(buff1, "Excellent!"), ReDraw(), IdleFunc(NULL);
	if (en(xx - 100, yy - 270) < 100)p = 100, sprintf_s(buff1, "Genius!!!"), ReDraw(), IdleFunc(NULL);
	if (en(xx - 200, yy - 250) < 2500)p = 30, sprintf_s(buff1, "good"), ReDraw(), IdleFunc(NULL);
	if (en(xx - 510, yy - 230) < 4900)p = 10, sprintf_s(buff1, "It's a piece of cake..."), ReDraw(), IdleFunc(NULL);
	if (en(xx - 380, yy - 260) < 400)p = 80, sprintf_s(buff1, "Great!!"), ReDraw(), IdleFunc(NULL);
	if (en(xx - 300, yy - 250) < 1600)p = 50, sprintf_s(buff1, "nice!"), ReDraw(), IdleFunc(NULL);

	if (((count++) % (1)) == 0)  ReDraw();
}




//キーボードを通して行う動作

void keyboardfunc(unsigned char k, int x, int y){
	using namespace SDGLibF;
	switch (k) {
	case 27:  exit(0);
	case 's': IdleFunc(simulation); break;
	case 'S': IdleFunc(NULL); break;

		//displayfuncでzの値に変化を及ぼし、角度を変化させる
	case 32:P += 0.015; break;

		//レベル設定を変更できる。レベルが高いほど弾が速くなる。
	case 'r':mag += 2.0; q += 1; ReDraw(); break;

	default:break;
	}
}



//メイン関数

int main(void){
	//画面表示の設定
	SDGLib mygraphic(600, 600, "- 2D Graphics - (61316444 西川　慶)", 0.0, 600.0, -300.0, 300.0);
	mygraphic.SetCursor(GLUT_CURSOR_WAIT);
	mygraphic.Display(displayfunc);
	mygraphic.Keyboard(keyboardfunc);
	//初期値設定
	xx = 300.0; yy = 0.0; rr = 3.0; z = change(-45.0); P = 0.0; p = 0, mag = 5.0, q = 1;
	mygraphic.Start();
	return 0;
}

/*
考察・説明
・落てくる球に合わせてバットを振り、的に当てると点数が表示される簡易ゲームプログラムである。
・球に当たった時のバットの角度でボールの飛ぶ方向が変わる。rキーでレベルを変更でき、レベルが上がるほど球の速度が上がる。
・sキーで玉が動き、スペースキーでバットを振る。Escキーでゲーム終了。
・多用する変数には配列を、多用する動作には関数を定義してやることによって、プログラムの作成を簡易化した。
・グローバル変数をうまく使うことによって、キーボードの動作をうまくディスプレイに表示させた。
・ゲームが終了（失敗）したらうまく止まるように、バットやボールの可動範囲を設定した。
・バットが一度ボタンを押しただけでうまく回るようにシュミレーション関数をうまく使った。
・バルーンの中にボールの中心が入ったときに止まるための条件設定には円の領域の数学的知識を使った。
*/
