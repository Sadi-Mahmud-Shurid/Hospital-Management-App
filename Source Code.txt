#include <iostream>
#include <fstream>
#include <string>

using namespace std;

class Patient {
public:
    Patient() : name(nullptr), gender('\0'), address(nullptr) {}
    Patient(const string& n, char g, const string& a) {
        name = new string(n);
        gender = g;
        address = new string(a);
    }

    ~Patient() {
        delete name;
        delete address;
    }

    const string& getName() const {
        return *name;
    }

    char getGender() const {
        return gender;
    }

    const string& getAddress() const {
        return *address;
    }
    void createPatient() {
    string name, address;
    char gender;

    cout << "Enter patient name: ";
    getline(cin, name);

    cout << "Enter patient gender: ";
    cin >> gender;

    cin.ignore();  // Ignore the newline character left in the input buffer

    cout << "Enter patient address: ";
    getline(cin, address);

    Patient patient(name, gender, address);

    ofstream file;
    file.open("patients.txt", ios::out | ios::app);

    if (!file) {
        cout << "Error opening file!" << endl;
        return;
    }

    file << patient.getName() << "," << patient.getGender() << "," << patient.getAddress() << endl;

    file.close();

    cout << "Patient created and saved!" << endl;
}
    void displayPatients() {
        ifstream file;
        file.open("patients.txt", ios::in);

        if (!file) {
            cout << "No patient records found!" << endl;
            return;
        }

        string line;
        string name, address;
        char gender;

        cout << "Patient Records:" << endl;

        while (getline(file, line)) {
           size_t pos = 0;
          

            pos = line.find(",");
            name = line.substr(0, pos);
            line.erase(0, pos + 1);

            gender = line[0];
            line.erase(0, 2);

            address = line;

            Patient patient(name, gender, address);

            cout << "Name: " << patient.getName() << endl;
            cout << "Gender(M/F): " << patient.getGender() << endl;
            cout << "Address: " << patient.getAddress() << endl;
            cout << "-------------------------" << endl;
        }

        file.close();
    }

private:
    string* name;
    char gender;
    string* address;
};





class Appointment : public Patient {
public:
    Appointment() {}
    Appointment(const string& n, char g, const string& a) : Patient(n, g, a) {}

    void saveAppointment() {
        ofstream file;
        file.open("appointments.txt", ios::out | ios::app);

        if (!file) {
            cout << "Error opening file!" << endl;
            return;
        }

        file << getName() << "," << getGender() << "," << getAddress() << endl;

        file.close();
    }
};

void displayHospitalInfo() {
    // Code for displaying hospital information
    cout << "Hospital Information" << endl;
    cout << "-------------------" << endl;
    cout << "Budapest University of Technology and Economics Health Center" << endl;
    cout << "Location:Budapest, Hungary" << endl;
    cout << "Contact: +3620596" << endl << "E-mail address: bmehealthcenter@gmail.com" << endl;
    cout << "Website: www.bmehealthcenter.com" << endl;
    cout << endl;
}


int main() {
    int choice;
    Patient patient;

    do {
        cout << "                              ||  HOSPITAL MANAGENENT APP  ||" << endl;
        cout << "                                        ---------" << endl << endl << endl << endl;
        cout << "1. Create patient" << endl << endl;
        cout << "2. Display patients" << endl << endl;
        cout << "3. Create appointment" << endl << endl;
        cout << "4. Display appointments" << endl << endl;
        cout << "5. Contact" << endl << endl;
        cout << "0. Exit" << endl << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        cin.ignore();  // Ignore the newline character left in the input buffer

        switch (choice) {
        case 1:
            system("cls");
            patient.createPatient();
            break;
        case 2:
            system("cls");
            patient.displayPatients();
            break;
        case 3: {
            system("cls");
            string name, time;
            char gender;

            cout << "Enter patient name: ";
            getline(cin, name);

            bool validGenderInput = false;

            while (!validGenderInput) {
                cout << "Enter patient gender(M/F): ";
                cin >> gender;

                cin.ignore();  // Ignore the newline character left in the input buffer

                try {
                    if (gender != 'M' && gender != 'F') {
                        throw invalid_argument("Invalid gender input. Please enter 'M' or 'F'.");
                    }

                    validGenderInput = true;
                }
                catch (const exception& e) {
                    cout << "Error: " << e.what() << endl;
                }
            }

            cout << "Enter Time(HH:MM): ";
            getline(cin, time);

            Appointment* appointment = new Appointment(name, gender, time);
            appointment->saveAppointment();
            delete appointment;

            cout << "Appointment created and saved!" << endl;
            break;
        }
        case 4: {
            system("cls");
            ifstream file;
            file.open("appointments.txt", ios::in);

            if (!file) {
                cout << "No appointments found!" << endl;
                break;
            }

            string line;
            Appointment appointment;

            cout << "Appointments:" << endl;

            while (getline(file, line)) {
                size_t pos = 0;

                pos = line.find(",");
                appointment.Patient::Patient(line.substr(0, pos), line[pos + 1], line.substr(pos + 3));
                line.erase(0, pos + 1);

                cout << "Name: " << appointment.getName() << endl;
                cout << "Gender(M/F): " << appointment.getGender() << endl;
                cout << "Time(HH:MM) " << appointment.getAddress() << endl;
                cout << "-------------------------" << endl;
            }

            file.close();
            break;
        }
        case 5:
            system("cls");
            displayHospitalInfo();
            break;
        case 0:
            cout << "Exiting..." << endl;
            break;
        default:
            cout << "Invalid choice!" << endl;
        }

        cout << endl;
    } while (choice != 0);

    return 0;
}