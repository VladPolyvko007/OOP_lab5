#include <stdlib.h>
#include <iostream>
#define N 5

using namespace std;

class advertisment
{
protected:
	string name;
	string customer;
public:
	advertisment()
	{
		name = "Назва";
		customer = "Замовник";
	};
	advertisment(string sname, string scustomer)
	{
		name = sname;
		customer = scustomer;
	};
	~advertisment() {};

	string get_name() { return name; }
	void set_name(string s) { name = s; }
	string get_customer() { return customer; }
	void set_customer(string s) { customer = s; }
	virtual double get_cost() { return 0; }
	virtual void input() {}
	virtual void output() {}
};

typedef advertisment ad;

class newspaper_ad : public advertisment
{
private:
	string np_name;
	double length;
	double width;
	double cost_per_cm;
public:
	newspaper_ad() : advertisment()
	{
		length, width, cost_per_cm = 0;
		np_name = "Назва газети";
	};
	newspaper_ad(string sname, string scustomer, string snp_name) : advertisment(sname, scustomer)
	{
		np_name = snp_name;
		length, width, cost_per_cm = 0;
	}
	~newspaper_ad() {};
	string get_np_name() { return np_name; }
	void set_np_name(string s) { np_name = s; }
	void set_length(double len) { length = len; }
	void set_width(double wid) { width = wid; }
	void set_cost_per_cm(double costper) { cost_per_cm = costper; }
	double get_length() { return length; }
	double get_width() { return width; }
	double get_cost_per_cm() { return cost_per_cm; }
	virtual double get_cost() { return length * width * cost_per_cm; }
	virtual void input()
	{
		cout << "\n\t" << name << "\n" << endl;
		cout << "Введіть довжину оголошення(см):";
		cin >> this->length;
		cout << "Введіть ширину оголошення(см):";
		cin >> this->width;
		cout << "Введіть ціну за квадратний сантиметр:";
		cin >> this->cost_per_cm;
	}
	virtual void output() 
	{
		cout << "\n" << get_name() << "\n" << get_customer() << "\n" << get_np_name() << "\n" << "Довжина:"
			<< get_length() << "\n" << "Ширина:" << get_width() << "\n" << "Ціна за квадратний см:"
			<< get_cost_per_cm() << "\n"
			<< "Ціна оголошення:" << get_cost() << endl;
	}
};

typedef newspaper_ad nad;

class billboard_ad : public advertisment
{
private:
	string place;
	double length, width, cost_per_day_sm;
	int days;
public:
	billboard_ad() : advertisment()
	{
		length, width, cost_per_day_sm, days = 0;
		place = "Місце";
	};
	billboard_ad(string sname, string scustomer, string splace) : advertisment(sname, scustomer)
	{
		place = splace;
		length, width, cost_per_day_sm, days = 0;
	}
	~billboard_ad() {};
	string get_place() { return place; }
	void set_place(string s) { place = s; }
	void set_length(double len) { length = len; }
	void set_width(double wid) { width = wid; }
	void set_cost_per_day_sm(double costper) { cost_per_day_sm = costper; }
	void set_days(int d) { days = d; }
	double get_length() { return length; }
	double get_width() { return width; }
	double get_cost_per_day_sm() { return cost_per_day_sm; }
	int get_days() { return days; }
	virtual double get_cost() { return length * width * cost_per_day_sm * days; }
	virtual void input()
	{
		cout << "\n\t" << name << "\n" << endl;
		cout << "Введіть довжину оголошення(м):";
		cin >> this->length;
		cout << "Введіть ширину оголошення(м):";
		cin >> this->width;
		cout << "Введіть ціну за квадратний метр в день:";
		cin >> this->cost_per_day_sm;
		cout << "Введіть кількість днів:";
		cin >> this->days;
	}
	virtual void output()
	{
		cout << "\n" << get_name() << "\n" << get_customer() << "\n" << get_place() << "\n" << "Довжина:"
			<< get_length() << "\n" << "Ширина:" << get_width() << "\n" << "Ціна за квадратний м в день:"
			<< get_cost_per_day_sm() << "\n" << "Кількість днів:" << get_days() << "\n"
			<< "Ціна оголошення:" << get_cost() << endl;
	}
};

typedef billboard_ad bad;

void sort_array(ad** arr)
{
	ad** pad, ** pad_ex, *adv;
	for (int k = 0; k < N - 1; k++)
	{
		pad_ex = arr + k;
		for (pad = pad_ex + 1; pad < arr + N; pad++)
			if ((*pad)->get_cost() < (*pad_ex)->get_cost())
				pad_ex = pad;
		if (pad_ex != arr + k)
		{
			adv = *pad_ex;
			*pad_ex = *(arr + k);
			*(arr + k) = adv;
		}
	}
}
void print_array(ad** arr);
double sum_cost(ad** arr);

int main()
{
	system("chcp 1251");
	ad* arr[N];
	nad ad1("Прибирання подвір'я", "Іван Петренко", "Справжній садівник");
	bad ad2("Виробництво меблів", "Андрій Оліярник", "Проспект Свободи, 27");
	bad ad3("Вулканізація шин", "Олексій Федчак", "Львівська, 15");
	nad ad4("Репетитор з математики", "Олексій Баранецький", "Всеосвіта");
	nad ad5("Манікюр гель-лаком", "Мар'яна Кривко", "Жіночі хитрощі");

	ad1.input();
	ad2.input();
	ad3.input();
	ad4.input();
	ad5.input();

	arr[0] = &ad1;
	arr[1] = &ad2;
	arr[2] = &ad3;
	arr[3] = &ad4;
	arr[4] = &ad5;

	print_array(arr);
	sort_array(arr);
	cout << "\n\t\tПосортований за зростанням масив:" << endl;
	print_array(arr);
	cout << "\n\nСумарна вартість всіх замовлень: " << sum_cost(arr);
	return 0;
}

void print_array(ad** arr)
{
	for (int i = 0; i < N; i++)
	{
		arr[i]->output();
	}
}
double sum_cost(ad** arr)
{
	double summ = 0;
	for (int i = 0; i < N; i++)
	{
		summ += arr[i]->get_cost();
	}
	return summ;
}
