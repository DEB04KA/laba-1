#include <iostream>
#include <fstream>
#include <string>
#include <vector>
#include <cstdlib>

using namespace std;

const int YEAR = 2026;

class WorkerPublic {
public:
    string name;
    string job;
    int year;
    double money;

    int exp() {
        return YEAR - year;
    }

    void show() {
        cout << name << "  " << job << "  " << year << "  " << money << "  staj: " << exp() << endl;
    }
};

class WorkerPrivate {
private:
    string name;
    string job;
    int year;
    double money;

public:
    void set_name(string n) { name = n; }
    void set_job(string j) { job = j; }
    void set_year(int y) { year = y; }
    void set_money(double m) { money = m; }

    string get_name() { return name; }
    string get_job() { return job; }
    int get_year() { return year; }
    double get_money() { return money; }

    int exp() {
        return YEAR - year;
    }

    void show() {
        cout << name << "  " << job << "  " << year << "  " << money << "  staj: " << exp() << endl;
    }
};

int main() {
    vector<WorkerPublic> pub;
    vector<WorkerPrivate> priv;

    ofstream f1("workers.txt");
    f1 << "Ivanov I.I.,Engineer,2015,75000\n"
        << "Petrov P.P.,Programmer,2018,120000\n"
        << "Sidorov S.S.,Analyst,2020,90000\n"
        << "Kozlov K.K.,Engineer,2010,85000\n"
        << "Morozov M.M.,Manager,2019,110000\n";
    f1.close();

    ifstream f2("workers.txt");
    string line;

    while (getline(f2, line)) {
        int p1 = line.find(',');
        int p2 = line.find(',', p1 + 1);
        int p3 = line.find(',', p2 + 1);

        string n = line.substr(0, p1);
        string j = line.substr(p1 + 1, p2 - p1 - 1);
        int y = atoi(line.substr(p2 + 1, p3 - p2 - 1).c_str());
        double m = atof(line.substr(p3 + 1).c_str());

        WorkerPublic wp;
        wp.name = n;
        wp.job = j;
        wp.year = y;
        wp.money = m;
        pub.push_back(wp);

        WorkerPrivate wp2;
        wp2.set_name(n);
        wp2.set_job(j);
        wp2.set_year(y);
        wp2.set_money(m);
        priv.push_back(wp2);
    }
    f2.close();

    int min_y;
    double min_m;
    string find_job;

    cout << "public" << endl;

    cout << "Min staj: ";
    cin >> min_y;
    cout << "Min money: ";
    cin >> min_m;
    cout << "Job: ";
    cin.ignore();
    getline(cin, find_job);

    cout << "\nAll:" << endl;
    for (int i = 0; i < pub.size(); i++) pub[i].show();

    cout << "\na) Staj > " << min_y << ":" << endl;
    for (int i = 0; i < pub.size(); i++) if (pub[i].exp() > min_y) pub[i].show();

    cout << "\nb) Money > " << min_m << ":" << endl;
    for (int i = 0; i < pub.size(); i++) if (pub[i].money > min_m) pub[i].show();

    cout << "\nc) Job = " << find_job << ":" << endl;
    for (int i = 0; i < pub.size(); i++) if (pub[i].job == find_job) pub[i].show();

    cout << "\n private " << endl;

    cout << "\nAll:" << endl;
    for (int i = 0; i < priv.size(); i++) priv[i].show();

    cout << "\na) Staj > " << min_y << ":" << endl;
    for (int i = 0; i < priv.size(); i++) if (priv[i].exp() > min_y) priv[i].show();

    cout << "\nb) Money > " << min_m << ":" << endl;
    for (int i = 0; i < priv.size(); i++) if (priv[i].get_money() > min_m) priv[i].show();

    cout << "\nc) Job = " << find_job << ":" << endl;
    for (int i = 0; i < priv.size(); i++) if (priv[i].get_job() == find_job) priv[i].show();

    return 0;
}
