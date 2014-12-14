Структури от Данни
==================

## Домашно 1 (стек)
Напишете програма която приема за вход инфиксен израз и файл който описва операторите използвани в изараза и извежда резултата след пресмятане на израза. В израза ще има числа, знаци (оператори) и скоби, като скобите винаги са с най-висок приоритет. Операторите в израза са символи които имат определен приоритет и асоциативност описани във входния фаил в следния формат:
 - символ оператор приоритет аосциативност
   - асоциативност: 1 = дясно, 0 = ляво
   - приоритет: по-голям приоритет = изпълнява се преди тези с по-малък
   - операторите са тези: +-*/
   - символът ще бъде операцията в израза

Пример за файла:
```
a + 10 1
b + 5 1
c - 5 1
d * 10 1
e / 10 0
f / 10 1
```
Пример за израз:
```
31 a ( 5 b 32 f 10 e -230 ) c 324 d 17
```

Допълнителни забележки
 - Структурите от данни трябва да са с ваши имплементации или да разбирате в дълбочина използваните.
 - Името на файла и израза ще се подават като аргументи на командият ред на вашата програма.
 - Файлът с правилата за операторите ще бъде винаги валиден, но изразът ще трябва вие да го валидирате и да изведете съобщение за грешка ако не е валиден.
 - Ако има отрицателни числа в израза то знакът минус '-' ще бъде долепен до числото, ако не е долепен то значи този знак трябва да се намира във файлът или израза е невалиден.


 
 

## Домашно 2 (списък, опашка)
Имайки следното:
```
class Market 
{
public:
    Market(int NumberOfAllCashDecks); // максимални брой каси които може да бъдат отворени в магазина (без експресната)
    void AddClient(Client * clients, int number); // добавяме number клиенти в магазина
    MarketState getMarketState(); // връща състоянието на магазина
    ClientState getClientState(int ID); // връща състоянието на клиента
};

struct ClientState
{
    int CashDeskPosition; // номер на каса
    int QueuePosition; // позиция в опашката на касата
    Client * client;
};

struct MarketState
{
    int numberOfCashDesk; // броя на касите които са отворили в момента
    int * numberOfClientsAtCashDecsk; // броя на клиентите на всяка каса в този момент 
};

struct Client
{
    int ID; // уникален номер на клиента в магазина
    int numberOfGoods; // брой на покупките на клиента
    bool creditCard; // истина ако плаща с крединта карта
};
```
Решете следният проблем:

Имаме магазин с **N** каси и допълнително една експресна. На всеки "**tick**" на магазина всички каси обработват по един продукт от кошницата на клиентите и може да се случи едно от следните действия:

  
    1. Затваряне на каса
    2. Преместване на клиенти
    3. Отваряне на каса
    ** като техния приоритет е както следва  1 > 2 > 3

Забележка  ако има предпоставки за случването на повече от едно действие то се случва само това с най-голям приоритет. Ако има няколко места в магазина където може да се случи едно и също деиствие и то е с най-висок приоритет то се случва само на касата която е с "по-малък пореден номер" (тоест по напред в листа)

Действията се случват на следният принцип:
На всеки "**tick**" се проверява по колко души има на опашка на всяка каса и ако на някоя каса има повече от **N** клиента се отваря нова каса и се разделя опашката на две равни части (през средата). Ако има каси чиято разлика е повече от **N/8** клента то половината от клиентите на по-дългата опашка трябва да си намерят по удачно място (тоест да се пренаредят по останалите опашки). Ако някоя каса има по малко от **N/10** клиента алчния мениджър я затваря и хората трябва да се преразпределят по останалите каси. Когато дойде нов клиент на касите той се нарежда на опашката с най-малко хора или ако с с равен на тази с по-малък пореден номер. Ако клиента има 3 или по-малко продукта и на експресната каса има по-малко от __2*N__ то той се нарежда на нея.

Уточнение експресната каса никога не затваря както и никога не преразпределя клиентите си(тоест не подлежи на никое от горните 3 правила).

Клиентите идват на всеки "**tick**" но понеже магазина е доста натоварен то част от тях минават през касите без да са си купили нищо за тях (имат нула продукта и минават директно). Хората които плащат с кредитна карта им отнема 1 "**tick**" да платят (след като всички продукти са били обработени) а на хората плащащи в брой им отнема 2 "**tick**"-а.


При инициализиране на вашия клас ще получите с колко каси разполага магазина. Като в началото задължително ще работи 1 нормална каса + експресната. Чрез методите ```getClientState``` и ```getМаrketState``` ще искаме да можем да следим текущото състояние на магазина като информация за клента ще получаваме като го търсим по ID-то му което ще получава на входа на магазина (метода ```Add```). При всяко извикване на метода ```add``` то в магазина минава един "**tick**", тоест се правят нужните пренареждания и касите извършват едно действие.


Изисквания към имплементацията :
От вас се изисква да предоставите като публичен интерфейс само функциите които сме ви дали в класа ```Market``` (със същите имена на функциите и променливите). Останалата част от имплементацията е изцяло по ваше усмотрение. Също така е задължително да имате списък от опашки който да представлява множеството от каси с чакащите хора на тях .Списъкът и опашката трябва да са имплементирани от вас като не се позволява използването на кой да е  контейнер от STL. Може да ползвате други имплементирани контейнери от вас ако сметнете за добре.


## Домашно 3 (сортировки)

Направете 5 класа които имплементират CustomSorter интерфейс и имплементират поне 5 различни алгоритъма за сортиране, като 4 от тях са (heap, merge, quick, insertion).
```
class Sorter {
public:
    virtual template<typename T>
    void sort(T * data, size_t count) = 0;

    virtual unsigned long long getSortTime() const = 0;
};
```

Направете клас, имплементиращ ```SortTester```, който да изпълни сортиращата функция на всеки от класовете подадени в конструктора му. Сортирането се извършва върху данни, които самият ```SortTester``` трябва да генерира. Методът ```getSummary``` трябва да извежда кратка статистика за изпълнението на сортиращите алгоритми включваща времето за което се изпълняват и описание на данните върху които са работили.

```
class SortTester {
public:
    SortTester(Sorter ** sorters, int count);
    virtual void getSummary(std::ostream & out) = 0;
};
```

Към домашното си качете _кратко_ описание на данните, които вашият тестов клас генерира. Опишете как се справят алгоритмите, които сте имплементирали с генерираните от вас данни. Добавете и изхода от ```SortTester::getSummary```.



## Домашно 4 (дървета)

Напишете програма която ви позволява да създавате [XML](http://en.wikipedia.org/wiki/XML) документи. Трябва да измислите и направите удобен интерфейс (клас/класове) за създаване на документа и добавяне/премахване/променяне на елемнтите в него. Трябва като минимум да имплементирате следната функционалност:

  * Добавяне/Изтриване/Променяне на тагове
  * Добавяне/Изтриване/Променяне на атрибути на таг
  * Итериране на таговете йерархично
  * Изпринтиране на документа в изходен поток по два начина - добре форматирано и сбито (без излишни нови редове и разстояние)
