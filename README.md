<p align="center">МИНИСТЕРСТВО НАУКИ  И ВЫСШЕГО ОБРАЗОВАНИЯ РОССИЙСКОЙ ФЕДЕРАЦИИ  <br/>
Федеральное государственное автономное образовательное учреждение высшего образования  <br/>
"КРЫМСКИЙ ФЕДЕРАЛЬНЫЙ УНИВЕРСИТЕТ им. В. И. ВЕРНАДСКОГО"  <br/>
ФИЗИКО-ТЕХНИЧЕСКИЙ ИНСТИТУТ  <br/>
Кафедра компьютерной инженерии и моделирования<br/></p>
<br/>

### <p align="center">Отчёт по лабораторной работе № 5<br/> по дисциплине "Программирование"</p>
<br/>

студента 1 курса группы ПИ-б-о-192(2) <br/>
Золотой Александр Витальевич<br/>
направления подготовки 09.03.04 "Программная инженерия"    
<br/>

<table>
<tr><td>Научный руководитель<br/> старший преподаватель кафедры<br/> компьютерной инженерии и моделирования</td>
<td>(оценка)</td>
<td>Чабанов В.В.</td>
</tr>
</table>
<br/><br/>

<p align="center">Симферополь, 2020</p>


### Тема: Работа с текстовыми файлами.

**Цель:** <br/>
1.Научиться работать с текстовыми файлами; <br/>
2.Закрепить навыки работы со структурами

**<p align="center">Ход работы:</p>**
    
**Код программы:**
```cpp
#include <iostream>
#include "pch.h"
#include <fstream>
#include <vector>
#include <string>
#include "cstdlib"
using namespace std;

struct pass {
// PassengerId, Survived, Pclass, Name, Sex, Age, SibSp, Parch, Ticket, Fare, Cabin, Embarked;
    int id=0;
    bool surv=false;
    int Pclass=0;
    string name=" ";
    string sex=" ";
    int age=0;
    int sib=0;
    int par=0;
    string ticket = " ";
    double fare=0.0;
    string cabin = " ";
    string  emb="0";
};

pass help(string str) {
    pass p;
    string temp[13];
    int i = 0;

    for (int j = 0; j < 13; j++) {
        while (str[i] != ',' && str[i] != '\0') {
            temp[j] += str[i];
            i++;
        }
        i++;
    }
    p.id= atoi(temp[0].c_str());
    p.surv= atoi(temp[1].c_str());
    p.Pclass= atoi(temp[2].c_str());
    p.name = temp[3]+" "+temp[4];
    p.sex= temp[5];
    p.age= atoi(temp[6].c_str());
    p.sib= atoi(temp[7].c_str());
    p.par= atoi(temp[8].c_str());
    p.ticket = temp[9];
    p.fare = atof(temp[10].c_str());
    p.cabin = temp[11];
    p.emb = temp[12];
    return p;
}

int main()
{
    ifstream fin("train.csv");
    vector <string> temp;
    string str;
    while (getline(fin, str, '\r'))
    {
        if (str.size() > 0)
            temp.push_back(str);
    }
    fin.close();
   
    int all_surv = 0, surv1 = 0, surv2 = 0, surv3 = 0, surv_w = 0, surv_m = 0, C = 0, Q = 0, S = 0, all_w=0, all_m=0;
    vector<int> minor;
    double count_w = 0.0, count_m = 0.0, all_age = 0;
    for (int i = 1; i < temp.size(); i++) {
        pass a=help(temp[i]);
        if (a.surv == 1) {
            all_surv++;
            if (a.sex == "male") {
                surv_m++;
            }
            else surv_w++;

            switch (a.Pclass) {
            case 1:
                surv1++;
                break;
            case 2:
                surv2++;
                break;
            case 3:
                surv3++;
                break;
            }
        }

        if (a.age < 18) {
            minor.push_back(a.id);
        }
        if (a.emb == "C") {
            C++;
        }
        if (a.emb == "S") {
            S++;
        }
        if (a.emb == "Q") {
            Q++;
        }


        if (a.sex == "male") {
           all_m++;
           count_m += a.age;
        }
        else {
            all_w++;
            count_w += a.age;
        }
        all_age+=a.age;
    }
   string em;
    if (C > S) {
        if (C > Q) {
          em= "Cherbourg";
        }
        else em = "Queenstown";
    }
    else
    {
        if (S > Q) {
            em = "Southampton";
        }
        else em = "Queenstown";
    }
    ofstream fout("Rezultat.txt");
        fout << "Общее число выживших:" << all_surv<< endl;
        fout << "Число выживших из 1 класса:" << surv1<< endl;
        fout << "Число выживших из 2 класса:" << surv2<< endl;
        fout << "Число выживших из 3 класса:" << surv3<< endl;
        fout << "Количество выживших женщин:" << surv_w<< endl;
        fout << "Количество выживших мужчин:" << surv_m<< endl;
        fout << "Средний возраст пассажира:" << all_age/ temp.size()<< endl;
        fout << "Средний женский возраст:" << count_w/all_w<< endl;
        fout << "Средний мужской возраст:" << count_m / all_m << endl;
        fout << "Штат, в котором село больше всего пассажиров:" <<em<< endl;
        fout << "Cписок идентификаторов несовершеннолетних (младше 18) пассажиров:";
        for (int i = 0; i < minor.size(); i++) {
            if (i == minor.size()-1) {
                fout << minor[i] << ".";
                fout.close();
                return 0;
            }
            fout << minor[i] << ", ";
        }
}
```

[Файл Rezultat.txt](https://github.com/alexzolfff/lab5/blob/master/Rezultat.txt)

Полученные результаты:
<figure class="wp-block-table"><table class=""><thead><tr><th>Характеристика</th><th>Результат</th></tr></thead><tbody><tr>
<td>Общее число выживших</td>
<td><center>342</td>
<tr><td>Число выживших из 1 класса
<td><center>136</td><tr>
<tr><td>Число выживших из 2 класса
<td><center>87</td><tr>
<tr><td>Число выживших из 3 класса
<td><center>119</td><tr>
<tr><td>Количество выживших женщин
<td><center>223</td><tr>
<tr><td>Количество выживших мужчин
<td><center>109</td><tr>
<tr><td>Средний возраст пассажира
<td><center>23.7567</td><tr>
<tr><td>Средний женский возраст
<td><center>23.1943</td><tr>
<tr><td>Средний мужской возраст
<td><center>24.104</td><tr>
<tr><td>Штат, в котором село больше всего пассажиров
<td><center>Southampton</td><tr>
<tr><td>Cписок идентификаторов несовершеннолетних (младше 18) пассажиров
<td>6, 8, 10, 11, 15, 17, 18, 20, 23, 25, 27, 29, 30, 32, 33, 37, 40, 43, 44, 46, 47, 48, 49, 51, 56, 59, 60, 64, 65, 66, 69, 72, 77, 78, 79, 83, 85, 87, 88, 96, 102, 108, 110, 112, 115, 120, 122, 126, 127, 129, 139, 141, 148, 155, 157, 159, 160, 164, 165, 166, 167, 169, 172, 173, 177, 181, 182, 183, 184, 185, 186, 187, 194, 197, 199, 202, 206, 209, 215, 221, 224, 230, 234, 236, 238, 241, 242, 251, 257, 261, 262, 265, 267, 271, 275, 278, 279, 283, 285, 296, 298, 299, 301, 302, 304, 305, 306, 307, 308, 325, 330, 331, 334, 335, 336, 341, 348, 349, 352, 353, 355, 359, 360, 365, 368, 369, 375, 376, 382, 385, 387, 389, 390, 408, 410, 411, 412, 414, 416, 420, 421, 426, 429, 432, 434, 436, 445, 446, 447, 449, 452, 455, 458, 460, 465, 467, 469, 470, 471, 476, 480, 481, 482, 486, 490, 491, 496, 498, 501, 503, 505, 508, 512, 518, 523, 525, 528, 531, 532, 533, 534, 536, 539, 542, 543, 548, 550, 551, 553, 558, 561, 564, 565, 569, 574, 575, 579, 585, 590, 594, 597, 599, 602, 603, 612, 613, 614, 619, 630, 634, 635, 640, 643, 644, 645, 649, 651, 654, 657, 668, 670, 675, 681, 684, 687, 690, 692, 693, 698, 710, 712, 719, 721, 722, 728, 732, 733, 739, 740, 741, 747, 751, 752, 756, 761, 765, 767, 769, 774, 777, 778, 779, 781, 782, 784, 788, 789, 791, 792, 793, 794, 803, 804, 814, 816, 820, 825, 826, 827, 828, 829, 831, 832, 833, 838, 840, 842, 845, 847, 850, 851, 853, 854, 860, 864, 869, 870, 876, 879, 889.</td><tr>
</table>

**Вывод:** в ходе лабораторной работы я научилcя работать с текстовыми файлами и закрепил навыки работы со структурами.



