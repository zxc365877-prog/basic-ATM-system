專案名稱與簡介:基礎ATM系統 # basic-ATM-system

功能說明:可進行存、提款、轉帳、確認餘額、退出ATM等功能

使用技術:使用CLASS當作主要架構，這樣可以預留較佳的擴充性，且較易管理和可重複使用

如何運行:首先會需要使用者設定密碼，後來便可透過輸入密碼操作ATM的相關功能以及退出ATM



#include <iostream>

using namespace std;


class ATM{
    private: //設定私有是為了確保餘額和密碼不會被亂修改

        float balance;
        int password;

    public:

        ATM(float initial_balance, int initial_password ){  //設定建構子初值
            balance = initial_balance;
            password = initial_password;
        }

    bool verify_login(){                                    //設定密碼認證功能
        for(int x = 0; x < 3; x ++){
            int input_password;
            cout << "Please enter the password:" << endl;
            cin >> input_password;

            if(password == input_password){
                cout << "Successfully login" << endl;

                return true;
            }

            else{
                cout << "Login failed, please try again" << endl;
            }

                if(x == 2){
                    cout << "The error has occurred three times. Please try again." << endl;
                }
        }
        return false;
    }


    void menu(){
        cout << " ======ATM MENU====== " << endl;

        cout << "      1.Deposit" << endl;

        cout << "      2.Withdraw" << endl;

        cout << "      3.Transfer" << endl;

        cout << "      4.Check balance" << endl;

        cout << "      5.Exit ATM" << endl;
    }

    void deposit(){
        float amount;
        cout << "Please enter the deposit amount:" << endl;
        cin >> amount;

        balance += amount;
        cout << "Amount deposit:" << amount << "dollars" << endl;
        cout << "Current balance:" << balance << "dollars" << endl;

    }

    void withdraw(){
        float amount;
        cout << "Please enter the withdrawal amount:" << endl;
        cin >> amount;

        if(balance >= amount){
            balance -= amount;
            cout << "Amount withdrawn:" << amount << "dollars" << endl;
        }
        else{
            cout << "Insufficient balance" << endl;
        }
        cout << "Current balance:" << balance << "dollars" << endl;

    }

    void transfer(){
        float amount;
        string target_account;

        cout << "Please enter the transfer amount: " << endl;
        cin >> amount;

        cout << "Please enter the target account: " << endl;
        cin >> target_account;

        if(balance >= amount){
            balance -= amount;
            cout << "Transfer successful" << endl;
            cout << "Current balance:" << balance << "dollars" << endl;
        }
        else
            cout << "transfer fail,please try again" << endl;
            cout << "Insufficient balance" << endl;
    }

    void check_balance(){
        cout << "Current balance:" << balance << "dollars" << endl;
    }

    void out(){
        cout << " Thank you for using, you have exited the ATM " << endl;
    }

    void run() {
        int choice;
        bool need_relogin = false;      //這裡是當執行完某些功能後，若要繼續操作，需重新輸入密碼

        while (true) {
            if (need_relogin) {          //此段用來判斷密碼輸入是否正確
                if (!verify_login()) {
                    cout << "The password has been entered incorrectly too many times. Please try again." << endl;
                    break;
                }
                need_relogin = false;
            }

            menu();
            cout << "Please select a function (enter a number 1~5): " << endl ;
            cin >> choice;
                                          //下面段落為chat gpt提供
            if (cin.fail()) {            //當輸入非數字為true,便進入除錯
                cin.clear();            //清除錯誤的狀態
                cin.ignore(1000, '\n');  //將錯誤的部分丟掉，並持續清除直到遇到\n
                cout << "The input is invalid, please re_enter the number" << endl ;
                continue;               //繼續執行while迴圈
            }

            if (choice == 1) {

                deposit();
                need_relogin = true; //此列代表執行完該函式若要再次使用其他功能，需重新輸入密碼
            }

            else if (choice == 2) {

                withdraw();
                need_relogin = true;
            }

            else if (choice == 3) {

                transfer();
                need_relogin = true;
            }

            else if (choice == 4) {

                check_balance();
                need_relogin = true;
            }

            else if (choice == 5) {

                out();

                break;
            }

            else {

                cout << "invalid option, please re_enter" << endl;
            }

            int cont_choice; //此段用來詢問當執行完該功能後是否需要繼續執行其他功能

            cout << "Do you want to continue to operate the ATM? [Yes:1 / No:0] " << endl;

            cin >> cont_choice;

            if (cont_choice == 0) {

                out();

                break;
            }
        }
    }
};

int main(){

    float start_balance;

    int start_password;

    cout << "Please enter the account opening amount: " << endl;

    cin >> start_balance;

    cout << "Please set password (4 number): " << endl;

    cin >> start_password;

    ATM atm(start_balance , start_password);

    if(atm.verify_login()){

        atm.run();
    }

return 0;

}
