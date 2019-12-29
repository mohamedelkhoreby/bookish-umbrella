# bookish-umbrella
the book store with the two classes 
#include<fstream> 
#include<conio.h> 
#include<string.h> 
#include<iomanip> 
#include <cstdlib>
#include <stdio.h> 
#include<iostream>
using namespace std;

class Book_Shop
{
          char book_number[90];
          char book_name[70];
           char book_type[30];
          char author_name[30];
          int  NoOfCopies;

  public:
          void get_book_details()
          {
cout<<"\nEnter Detalis About Book \n";
                    cout<<"\nEnter The Book Number: ";
                    cin>>book_number;
                    cout<<"\nEnter The Name of The Book: ";
                    cin.ignore();
                    cin.getline(book_name,30);
                    cout<<"\nEnter The type of The Book: ";
                    cin.ignore();
                    cin.getline(book_type,30);
                    cout<<"\nEnter The Author's Name: ";
                    cin.ignore();
                    cin.getline(author_name,30);
                    fflush(stdin);
           			cout<<"\nEnter No. Of Copies : ";
    				cin>>NoOfCopies;
          }

          void show_book()
          {
                    cout<<"\nBook Number:"<<book_number;
                    cout<<"\nBook Name:"<<book_name;
                     cout<<"\nBook type:"<<book_type;
                    cout<<"\nAuthor's Name:"<<author_name;
                    cout<<"\nCOPIES :"<<NoOfCopies;
          }
          void modify_book()
          {
                    cout<<"\nBook number : "<<book_number;
                    cout<<"\nModify Book Name : ";
                    cin.ignore();
                    cin.getline(book_name,50); 
					cout<<"\nModify Book type : ";
                    cin.ignore();
                    cin.getline(book_type,50);
                    cout<<"\nModify Author's Name: ";
                    cin.ignore();
                    cin.getline(author_name,50);
                    fflush(stdin);
       				cout<<"\nEnter No. Of Copies : ";
    				cin>>NoOfCopies;
          }
          char* getbooknumber()
          {
                    return book_number;
          }
          void report()
          {cout<<book_number<<setw(25)<<book_name<<setw(12)<<book_type<<setw(15)<<author_name<<setw(10)<<NoOfCopies<<endl;
    cout<<"_____________________________________________________________________________________________\n";}
    
};

fstream file;
Book_Shop bk;
void write_book()
{
          system("cls");
          int MoreOrMenu;
          file.open("book.dat",ios::out|ios::app);
          do
          {
                    bk.get_book_details();
                    file.write((char*)&bk,sizeof(Book_Shop));
                    cout<<"\nPress 1 to add more books.";
                    cout<<"\nPress 2 to return to main menu.\n";
                    cout<<"Enter: ";
                    cin>>MoreOrMenu;
          }while(MoreOrMenu == 1);
         file.close();
}

void display_a_book(char n[])
{
          system("cls");
          cout<<"\nBook Details\n";
          int check=0;
          file.open("book.dat",ios::in);
          while(file.read((char*)&bk,sizeof(Book_Shop)))
          {
                    if(strcmpi(bk.getbooknumber(),n)==0)
                    {
                               bk.show_book();
                              check=1;
                    }
          }
          file.close();
          if(check==0)
                    cout<<"\n\nBook does not found";
       getch();
}

void modify_book()
{
          system("cls");
          char n[20];
          int check=0;
          cout<<"\n\n\tModfiy Book";
          cout<<"\n\n\tEnter The book number: ";
          cin>>n;
          file.open("book.dat",ios::in|ios::out);
          while(file.read((char*)&bk,sizeof(Book_Shop)) && check==0)
          {
                    if(strcmpi(bk.getbooknumber(),n)==0)
                    {
                               bk.show_book();
                               cout<<"\nEnter The New Details of book"<<endl;
                               bk.modify_book();
                               int p=-1*sizeof(bk);
                              file.seekp(p,ios::cur);
                              file.write((char*)&bk,sizeof(Book_Shop));
                              cout<<"\n\n\t Record Updated Successfully...";
                              check=1;
                    }
          }
          file.close();
          if(check==0)
                    cout<<"\n\n Record Not Found ";
          getch();
}

void delete_book()
{
          system("cls");
          char n[20];
          int sign=0;
          cout<<"\n\n\n\tDelete Book";
          cout<<"\n\nEnter The Book's number You Want To Delete: ";
          cin>>n;
          file.open("book.dat",ios::in|ios::out);
          fstream file2;
          file2.open("Temp.dat",ios::out);
          file.seekg(0,ios::beg);
          while(file.read((char*)&bk,sizeof(Book_Shop)))
          {
                    if(strcmpi(bk.getbooknumber(),n)!=0)
                    {
                               file2.write((char*)&bk,sizeof(Book_Shop));
                    }
                    else
                              sign=1;
          }
          file2.close();
          file.close();
          remove("book.dat");
          rename("Temp.dat","book.dat");
          if(sign==1)
                    cout<<"\n\n\tRecord Deleted ..";
          else
                    cout<<"\n\nRecord not found";
          getch();
}

void display_allb()
{
          system("cls");
          file.open("book.dat",ios::in);
          if(!file)
          {
                    cout<<" File Could Not Be Open ";
                    getch();
                    return;
          }
          cout<<"\n\n\t\t the Book List in the store\n\n";
         cout<<"_____________________________________________________________________________________________\n";
          cout<<"Book Number"<<setw(15)<<"Book Name"<<setw(15)<<"book type"<<setw(10)<<"Author"<<setw(10)<<"Copies"<<endl;
          cout<<"_____________________________________________________________________________________________\n";
          while(file.read((char*)&bk,sizeof(Book_Shop)))
          {
                    bk.report();
          }
          file.close();
          getch();
}
void color()
{
     system("color 0f");
         system("color 7f");
     system("cls");

}

int main()
{
     int operation = 0;
          for(;;)
          {
               color();
                cout <<"\n\t\t    welcome to bookstore app ";
               cout<<"\n\t\t"<<endl<<"\t\t      please Press number";
               cout<<"\n\t\tPress 1 to Add Book Records:"<<endl<<"\t\tPress 2 to Show Book Records:";
                cout<<"\n\t\tPress 3 to Check Availability:"<<endl<<"\t\tPress 4 to Modify Book Records:";
                cout<<"\n\t\tPress 5 to Delete Book Records:"<<endl<<"\t\tPress 6 to Exit";
               cout<<"\n\t\tOption: ";
 			cin>>operation;
 			switch(operation)
          	{
    case 1:  system("cls");
       write_book();
         break;
    case 2: display_allb();
	break;
    case 3:
                              char n[20];
                               system("cls");
                              cout<<"\n\n\tPlease Enter The book No. ";
                              cin>>n;
                              display_a_book(n);
                              break;
    case 4: modify_book();break;
    case 5: delete_book();break;
   case 6: exit(0);
   break;
 default:cout<<"\a";
          }

          }
          return 0;
}
