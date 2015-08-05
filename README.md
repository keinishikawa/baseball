# baseball
バッティングゲーム

//61316444Å@êºêÏÅ@åc 
//ÉIÉäÉWÉiÉãÉQÅ[ÉÄÅuÉoÉbÉeÉBÉìÉOÉXÉgÉâÉbÉNÉAÉEÉgÉQÅ[ÉÄÅvÇÃçÏê¨

#include <windows.h>
#include <math.h>
#include <iostream>
#include <stdexcept>
#include <string>
#include <string.h>
#include "sdglib.h"


//ïœêîÅEä÷êîÇÃíËã`

static double xx, yy, rr, z, P, mag;
static int p, q;
int m(0);

//sprintf_sÇ≈ë„ì¸Ç≈Ç´ÇÈÇÊÇ§Ç…îzóÒç\ë¢Ç…ÇµÇΩÅB
char buff1[100], buff2[100];

char string_s[] = "60";
char string_s1[] = "100";
char string_s2[] = "30";
char string_s3[] = "10";
char string_s4[] = "80";
char string_s5[] = "50";


//ÉâÉWÉAÉìÇ©ÇÁÅãÇ…ïœä∑Ç∑ÇÈìÆçÏÇëΩópÇ∑ÇÈÇΩÇﬂÇ…ÅAÇ±ÇÃìÆçÏÇä÷êîâªÇµÇΩÅB
double change(double a){
	double b = a*3.14 / 180;
	return b;

}

//ìØólÇ…â~ì‡ÇÃóÃàÊÇï\Ç∑Ç∆Ç´Ç…égÇ§x^2+y^2<r^2ÇÃéÆÇçÏÇÈÇÃÇä»ó™âªÇ∑ÇÈÇΩÇﬂÇ…Ç±ÇÃìÆçÏÇä÷êîâªÇµÇΩÅB
double en(double b, double c){
	double d = b*b + c*c;
	return d;
}




//ÉfÉBÉXÉvÉåÉCÇ…ä÷Ç∑ÇÈìÆçÏ

void displayfunc(){
	using namespace SDGLibF;
	char buff[100];

	// Set drawing plane
	Before();


	//ÉOÉçÅ[ÉoÉãïœêîÇ≈ê›íËÇµÇΩïœêîÇópÇ¢ÇƒÅAìÆìIÇ…ïœâªÇ∑ÇÈâ~ÅiÉ{Å[ÉãÅjÇçÏÇ¡ÇΩÅB
	SetColor(1.0, 0.0, 0.0);
	DrawCircle(1.0, xx, yy, rr);

	//ÉoÉbÉgÇÃçÏê¨
	SetColor(0.0, 1.0, 1.0);
	DrawLine(3.0, 260, -220, 260 + 80 * cos(z), -220 + 80 * sin(z));
	DrawLine(3.0, 260, -220, 260 + 80 * cos(z - change(10)), -220 + 80 * sin(z - change(10)));
	DrawLine(3.0, 260 + 80 * cos(z), -220 + 80 * sin(z), 260 + 80 * cos(z - change(10)), -220 + 80 * sin(z - change(10)));
	DrawCircle(3.0, 260, -220, 5.0);

	//ç∂â∫Ç…ì_êîï\é¶
	sprintf_s(buff, "Your score is %d", p);
	DrawString(10, -290, buff);

	//ìIÇÃÉoÉãÅ[ÉìÇ∆ÇªÇÃì_êîÇÃï`âÊ

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



//â…Ç»Ç∆Ç´Ç…çsÇ§ìÆçÏ

void simulation(void){
	using namespace SDGLibF;
	static int count = 0;
	static double y = 0.0, vy = 0.0, ay = 0.0, x = 300.0, vx = 0.0;
	static double dt = 0.005, g = 9.8;
	static double A, B, C, D, E, F;

	//â^ìÆÇÃéÆÅiÉoÉbÉgÇ…ìñÇΩÇÈÇ‹Ç≈ÇÕèdóÕâ¡ë¨ìxgÇ…ÇµÇΩÇ™Ç¡Çƒyï˚å¸Ç…â¡ë¨ÇµÅAíµÇÀï‘ÇËå„ÇÕÉoÉbÉgÇÃäpìxÇ…âûÇ∂ÇƒÇ‹Ç¡Ç∑ÇÆêiÇﬁÅj  
	x = x + vx;
	ay = -g;
	y = y + vy*dt;
	vy = vy + ay*dt;
	xx = x; yy = y*mag;

	//ÉLÅ[É{Å[Éhì¸óÕÇ…ÇÊÇËPÇÃílÇ™ïœâªÇµÅAÇªÇÍÇ…ÇÊÇ¡ÇƒzÇÃílÇ™ëùÇµÇƒÇ¢Ç´ÅAÉoÉbÉgÇÃâÒì]Ç™énÇ‹ÇÈÅBÉfÉBÉXÉvÉåÉCÇÃçXêVÇóòópÇµÇƒé©ìÆâÒì]ÇµÇƒÇ¢ÇÈÇÊÇ§Ç…å©ÇπÇƒÇ¢ÇÈÅB
	z += P;

	//ÉoÉbÉgÇ™xé≤Ç…ëŒÇµÇƒ70ÅãÇÃínì_Ç≈é~Ç‹ÇÈÇΩÇﬂÇÃê›íË
	if (z > change(70))P = 0.0;

	//ÉoÉbÉgÇÃë≈åÇÇ…ÇÊÇÈíµÇÀï‘ÇËÇÃèåèÅByyÇÃç¿ïWÇÕëäéóÇçlÇ¶ÇƒÇ¢ÇÈÅBè⁄ÇµÇ≠ÇÕíÒèoÉåÉ|Å[ÉgÇ…ãLç⁄Ç∑ÇÈÅB
	if (xx == 300 && yy < -220 + 80 * (40 / (80 * cos(z))) * sin(z)) vx = cos(z + change(90)), vy = 10 * sin(z + change(90)), g = 0.0;

	//ç∂â∫ÇÃÉRÉÅÉìÉgóì
	if (xx > 600.0 || yy > 300.0 || xx < 0.0) sprintf_s(buff1, "missed!"), IdleFunc(NULL);
	if (en(xx - 30, yy - 240) < 900)p = 60, sprintf_s(buff1, "Excellent!"), ReDraw(), IdleFunc(NULL);
	if (en(xx - 100, yy - 270) < 100)p = 100, sprintf_s(buff1, "Genius!!!"), ReDraw(), IdleFunc(NULL);
	if (en(xx - 200, yy - 250) < 2500)p = 30, sprintf_s(buff1, "good"), ReDraw(), IdleFunc(NULL);
	if (en(xx - 510, yy - 230) < 4900)p = 10, sprintf_s(buff1, "It's a piece of cake..."), ReDraw(), IdleFunc(NULL);
	if (en(xx - 380, yy - 260) < 400)p = 80, sprintf_s(buff1, "Great!!"), ReDraw(), IdleFunc(NULL);
	if (en(xx - 300, yy - 250) < 1600)p = 50, sprintf_s(buff1, "nice!"), ReDraw(), IdleFunc(NULL);

	if (((count++) % (1)) == 0)  ReDraw();
}




//ÉLÅ[É{Å[ÉhÇí ÇµÇƒçsÇ§ìÆçÏ

void keyboardfunc(unsigned char k, int x, int y){
	using namespace SDGLibF;
	switch (k) {
	case 27:  exit(0);
	case 's': IdleFunc(simulation); break;
	case 'S': IdleFunc(NULL); break;

		//displayfuncÇ≈zÇÃílÇ…ïœâªÇãyÇ⁄ÇµÅAäpìxÇïœâªÇ≥ÇπÇÈ
	case 32:P += 0.015; break;

		//ÉåÉxÉãê›íËÇïœçXÇ≈Ç´ÇÈÅBÉåÉxÉãÇ™çÇÇ¢ÇŸÇ«íeÇ™ë¨Ç≠Ç»ÇÈÅB
	case 'r':mag += 2.0; q += 1; ReDraw(); break;

	default:break;
	}
}



//ÉÅÉCÉìä÷êî

int main(void){
	//âÊñ ï\é¶ÇÃê›íË
	SDGLib mygraphic(600, 600, "- 2D Graphics - (61316444 êºêÏÅ@åc)", 0.0, 600.0, -300.0, 300.0);
	mygraphic.SetCursor(GLUT_CURSOR_WAIT);
	mygraphic.Display(displayfunc);
	mygraphic.Keyboard(keyboardfunc);
	//èâä˙ílê›íË
	xx = 300.0; yy = 0.0; rr = 3.0; z = change(-45.0); P = 0.0; p = 0, mag = 5.0, q = 1;
	mygraphic.Start();
	return 0;
}

/*
çlé@ÅEê‡ñæ
ÅEóéÇƒÇ≠ÇÈãÖÇ…çáÇÌÇπÇƒÉoÉbÉgÇêUÇËÅAìIÇ…ìñÇƒÇÈÇ∆ì_êîÇ™ï\é¶Ç≥ÇÍÇÈä»à’ÉQÅ[ÉÄÉvÉçÉOÉâÉÄÇ≈Ç†ÇÈÅB
ÅEãÖÇ…ìñÇΩÇ¡ÇΩéûÇÃÉoÉbÉgÇÃäpìxÇ≈É{Å[ÉãÇÃîÚÇ‘ï˚å¸Ç™ïœÇÌÇÈÅBrÉLÅ[Ç≈ÉåÉxÉãÇïœçXÇ≈Ç´ÅAÉåÉxÉãÇ™è„Ç™ÇÈÇŸÇ«ãÖÇÃë¨ìxÇ™è„Ç™ÇÈÅB
ÅEsÉLÅ[Ç≈ã Ç™ìÆÇ´ÅAÉXÉyÅ[ÉXÉLÅ[Ç≈ÉoÉbÉgÇêUÇÈÅBEscÉLÅ[Ç≈ÉQÅ[ÉÄèIóπÅB
ÅEëΩópÇ∑ÇÈïœêîÇ…ÇÕîzóÒÇÅAëΩópÇ∑ÇÈìÆçÏÇ…ÇÕä÷êîÇíËã`ÇµÇƒÇ‚ÇÈÇ±Ç∆Ç…ÇÊÇ¡ÇƒÅAÉvÉçÉOÉâÉÄÇÃçÏê¨Çä»à’âªÇµÇΩÅB
ÅEÉOÉçÅ[ÉoÉãïœêîÇÇ§Ç‹Ç≠égÇ§Ç±Ç∆Ç…ÇÊÇ¡ÇƒÅAÉLÅ[É{Å[ÉhÇÃìÆçÏÇÇ§Ç‹Ç≠ÉfÉBÉXÉvÉåÉCÇ…ï\é¶Ç≥ÇπÇΩÅB
ÅEÉQÅ[ÉÄÇ™èIóπÅié∏îsÅjÇµÇΩÇÁÇ§Ç‹Ç≠é~Ç‹ÇÈÇÊÇ§Ç…ÅAÉoÉbÉgÇ‚É{Å[ÉãÇÃâ¬ìÆîÕàÕÇê›íËÇµÇΩÅB
ÅEÉoÉbÉgÇ™àÍìxÉ{É^ÉìÇâüÇµÇΩÇæÇØÇ≈Ç§Ç‹Ç≠âÒÇÈÇÊÇ§Ç…ÉVÉÖÉ~ÉåÅ[ÉVÉáÉìä÷êîÇÇ§Ç‹Ç≠égÇ¡ÇΩÅB
ÅEÉoÉãÅ[ÉìÇÃíÜÇ…É{Å[ÉãÇÃíÜêSÇ™ì¸Ç¡ÇΩÇ∆Ç´Ç…é~Ç‹ÇÈÇΩÇﬂÇÃèåèê›íËÇ…ÇÕâ~ÇÃóÃàÊÇÃêîäwìIíméØÇégÇ¡ÇΩÅB
ÅEÉ{Å[ÉãÇ™ÉoÉbÉgÇ…ìñÇΩÇ¡ÇΩç€Ç…íµÇÀï‘ÇÈÇΩÇﬂÇÃèåèê›íËÇ…ÇÕà»â∫ÇÃÇÊÇ§Ç»äÙâΩä÷åWÇóòópÇµÇΩÅB








*/
