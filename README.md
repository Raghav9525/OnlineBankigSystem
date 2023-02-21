# OnlineBankigSystem
this is online Banking project in which multiple user can create account and after login can perform multiple opraton like deposite and withdraw we have use Linked List to store the informaton of user

#include<stdio.h>
#include<iostream>
using namespace std;
#include<cstring>
#include<ctype.h>
#include<conio.h> 
#include<stdlib.h>
class Uid;

class Bank
{
public:
    char name[30];
    char acc_no[15];
    char toa[10];//toa=type of account
    float balance,dob;
    Bank *next;

    void create_acc(Bank **start)
    {
        Bank *ptr,*temp=*start;
        int i;
        ptr=(Bank*)malloc(sizeof(Bank));
        ptr->next=NULL;
        cout<<"Enter your name= ";
        fflush(stdin);
        gets(ptr->name);
        cout<<"Which type of Account you want to open= ";
        fflush(stdin);
        gets(ptr->toa);
        fflush(stdin);
        cout<<"Enter your mobile number= ";
        gets(ptr->acc_no);
        fflush(stdin);
        cout<<"Enter your date of birth year= ";
        cin>>ptr->dob;
        do
        {
           cout<<"How much amount you want to deposit(minimum 1000)=";
           cin>>ptr->balance;
           if(ptr->balance<1000)
           	 cout<<"\nInsaficient balance, please deposite more than 1000 again\n\n";
        }while(ptr->balance<1000);
        if(temp==NULL)
            *start=ptr;
        else
        {
            while(temp->next!=NULL)
                temp=temp->next;
            temp->next=ptr;
        }
        if(*start!=NULL)
        {
            cout<<"\nHii "<<ptr->name;
            cout<<" Your Account has successfully opened"<<endl<<endl;
            cout<<"your 'Mobile number' is your 'Account number' and 'date of birth year' is your 'Password'\n\n";
            cout<<"Press any key to continue=";
            getch();
            system("cls");
            cout<<endl;
        }
        else
            cout<<endl<<"Sorry Account has not opened please try again"<<endl;
    }

    friend void depo_money(Bank*);
    friend void withd_money(Bank*);
    friend void show_bal(Bank*);
    friend void acc_details(Bank*);


};
    void depo_money(Bank *avp)
    {
        float b;
        cout<<"How much money you want deposit= ";
        cin>>b;
        cout<<"\n\nYour account balance was= "<<avp->balance;
        avp->balance+=b;
        cout<<endl<<b<<" Deposited"<<endl;
        cout<<"\nNow your current balance is= "<<avp->balance<<endl;
    }
    void withd_money(Bank *avp)
    {
        float b;
        cout<<"How much money you want withdraw= ";
        cin>>b;
        if((avp->balance-b)<1000)
        {
    		 cout<<"Your Account Balance is="<<avp->balance<<endl;	
        	cout<<endl<<"Which is Insaficient balance for Withdraw"<<endl;
        	cout<<"\nBecuse Minimum 1000 rupeess should be in your account"<<endl;
		}
        else
        {
        	cout<<"\n\nYour account balance was= "<<avp->balance<<endl;
            avp->balance-=b;
            cout<<endl<<b<<" Withdrawn"<<endl;
            cout<<"\nNow your current balance is= "<<avp->balance<<endl;
        }
    }
    void show_bal(Bank *avp)
    {
        cout<<"Your Account Balance is="<<avp->balance<<endl;
    }
    void acc_details(Bank *avp)
    {
        if(avp==NULL)
            cout<<"\nNo Account Created\n";
        else
        {
            cout<<"\nAccount Details are:"<<endl;
            cout<<"Account holder name is= "<<avp->name<<endl;
            cout<<"Account number is= "<<avp->acc_no<<endl;
            cout<<"Type of Account is= "<<avp->toa<<endl;
            cout<<"Total Balance is= "<<avp->balance<<endl;
        }
    }


//derive class
class Uid:public Bank//Uid=userid
{
private:
    char uid[15];
    int pass;
public:
    Bank* login(Bank *start,int *la)// la=login account
    {
        int j;
        Bank *temp=start;
        if(temp==NULL)
                cout<<"No Account Avilable please create new account:\n";
        else
        {
                cout<<"Enter you details for login:\n";
                cout<<"Enter your Account number= ";
                fflush(stdin);
                gets(uid);
                cout<<"Enter your password= ";
                fflush(stdin);
                cin>>pass;

                while(temp!=NULL)
                {
                    if(strcmp(temp->acc_no,uid)==0&&temp->dob==pass)
                    {
                    	cout<<"\nyou have successfully login into Account\n";
                        cout<<"\nPress any key to continue=";
                        getch();
                        system("cls");
                    	return(temp);
					}
                    temp=temp->next;
                }
                *la=1;
        }
        return(temp);
    }
};

int main()
{
    Uid obj;
    Bank *start=NULL,*avp=NULL;//avp=account varifying pointer
    int o_c,br=0,i,j,la=0;// la=login atempts
    char choice;
    while(1)
    {
    	cout<<"Press(A,B,C) to perform given below operations:\n";
        cout<<"\nA. Create New Account\nB. Login into Account\nC. Exit\n"<<endl;
        cout<<"Enter your choice=";
        fflush(stdin);
        cin>>choice;
        cout<<endl;
        switch(tolower(choice))
        {
        case 'a':
        {
            start->create_acc(&start);
            break;
        }
        case 'b':
        {
            for(i=0,j=2;i<3;i++,j--)//j=2,1//0
            {
               avp=obj.login(start,&la);
               if(la==1)
               {
                   cout<<"\nInvalid Account number OR password\n";
                   cout<<j<<" Attempt left\n\n";
                   if(j==0)
                   {
                   		la=0;
				   		cout<<"You have exceeded the login Attempts, Please login into other Account\n\n";
				   		cout<<"Press any key to continue=";
				   		getch();
				   		system("cls");
				   		break;
					}
                   la=0;//0
               }
               else
                break;
            }
            if(j==0&&avp==NULL)//J=0
            	{break;}
            cout<<"\nBelow, given some operations that you can perform\n";
			cout<<"\nPress Number(1,2,3,4,5) infornt of the operation, To perform desired operation:\n";
            
            br=0;
            while(1)
            {
                if(br==1)
                    break;
                cout<<"\n1.Show Account Details\n2.Check Balance\n3.Deposit Money\n4.Withdraw Money\n5.Logout from Account\n";
                cout<<"\nWhich operation you want to perform= ";
                fflush(stdin);
                cin>>o_c;//o_c=operation choice
                cout<<endl;
                switch(o_c)
                {
                case 1:
                {
                    acc_details(avp);
                    cout<<"\nPress any key to continue=";
                    getch();
                    system("cls");
                    break;
                }
                case 2:
                {
                    show_bal(avp);
                    cout<<"\nPress any key to continue=";
                    getch();
                    system("cls");
                    break;
                }
                case 3:
                {
                    depo_money(avp);
                    cout<<"\nPress any key to continue=";
                    getch();
                    system("cls");
                    break;
                }
                case 4:
                {
                   withd_money(avp);
                   cout<<"\nPress any key to continue=";
                   getch();
                   system("cls");
                    break;
                }
                case 5:
                {
                	cout<<"You have successfully Logout\n\n";
                    br=1;
                    break;
                }
                default:
                    cout<<"Invalid choice";
                }
            }
            break;
        }
        case 'c':
            exit(0);

        default:
            cout<<"Invalid choice"<<endl;
        }
    }
    getch();
}


