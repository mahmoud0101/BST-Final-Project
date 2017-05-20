#include<iostream>
#include <Windows.h>
#include <stdlib.h>
#include <stack>
#include <conio.h>
using namespace std;

class BTS
{
private:
	class Node
	{
	public:
		int ID;
		string name;
		int age;
		string *drugs = new string[10];
		Node* right;
		Node* left;

		Node()
		{
			right = 0;
			left = 0;
		}

		Node(int Id, string Name, int Age, string Drugs[])
		{
			name = Name;
			age = Age;
			ID = Id;
			for (int i = 0; i < 10; i++)
				drugs[i] = Drugs[i];

			right = 0;
			left = 0;
		}
	};

public:

	BTS();

	void Insert(int ID, string name, int age, string Drugs[]);
	void Delete(int);
	void Search(int);
	Node* FindMin(Node* root);
	void InOrder(Node*);

	Node* root;
	int Counter = 0;
	int ArrayID[100];
};



BTS::BTS()
{
	root = 0;
}

void BTS::Insert(int ID, string name, int age, string Drugs[])
{
	Node* node = new Node(ID, name, age, Drugs);
	if (root == 0)
	{
		root = node;
	}
	else
	{
		Node* ptr = root;
		Node* ptr2 = root;
		while (ptr != 0)
		{
			ptr2 = ptr;
			if (ID < ptr->ID)
			{
				ptr = ptr->left;
			}
			else if (ID > ptr->ID)
			{
				ptr = ptr->right;
			}
		}
		if (ID < ptr2->ID)
			ptr2->left = node;
		else
			ptr2->right = node;
	}
}

void BTS::Search(int Id)
{
	bool found = false;
	Node* ptr = root;
	while (ptr != 0 && !found)
	{
		if (Id > ptr->ID)
		{
			ptr = ptr->right;
		}
		else if (Id < ptr->ID)
		{
			ptr = ptr->left;
		}
		else if (Id == ptr->ID)
		{
			found = true;
		}
	}
	if (found == true)
	{
		cout << "Patient's ID : " << ptr->ID << endl;
		cout << "Patient's Name : " << ptr->name << endl;
		cout << "Patient's Age : " << ptr->age << endl;
		cout << "Patient's required drugs : ";
		for (int i = 0; i < 10; i++)
		{
			cout << i + 1 << "-" << ptr->drugs[i] << endl;
		}
	}
	else
		cout << "Not Found";

}
void BTS::Delete(int Id)
{
	Node* ptr = root;
	Node* parentPTR = root;
	while (ptr != 0)
	{
		if (Id > ptr->ID)
		{
			parentPTR = ptr;
			ptr = ptr->right;
		}
		else if (Id < ptr->ID)
		{
			parentPTR = ptr;
			ptr = ptr->left;
		}
		else if (Id == ptr->ID)
		{
			break;
		}
	}
	Node* dptr = ptr;
	if (dptr->right == 0 && dptr->left == 0)
	{
		if (parentPTR->ID < dptr->ID)
			parentPTR->right = 0;
		else
			parentPTR->left = 0;
		delete dptr;
	}
	else if (dptr->right == 0 && dptr->left != 0)
	{
		if (parentPTR->ID < dptr->ID)
			parentPTR->right = dptr->left;
		else
			parentPTR->left = dptr->left;
		delete dptr;
	}
	else if (dptr->left == 0 && dptr->right != 0)
	{
		if (parentPTR->ID < dptr->ID)
			parentPTR->right = dptr->right;
		else
			parentPTR->left = dptr->right;
		delete dptr;
	}
	else
	{
		Node* PPtr = FindMin(ptr->right);
		ptr->ID = PPtr->ID;
		ptr->name = PPtr->name;
		ptr->age = PPtr->age;
		for (int i = 0; i < 10; i++)
		{
			ptr->drugs[i] = PPtr->drugs[i];
		}
		delete PPtr;
	}

}

BTS::Node* BTS::FindMin(Node* ptr)
{
	Node* parent = ptr;
	while (ptr->left != 0)
	{
		parent = ptr;
		ptr = ptr->left;
	}

	parent->left = 0;
	return ptr;
}


void BTS::InOrder(Node* root)
{
	if (root == 0)
	{
		return;
	}
	else
	{
		InOrder(root->left);
		cout << "Patient's ID : " << root->ID << endl;
		cout << "Patient's Name : " << root->name << endl;
		cout << "Patient's Age : " << root->age << endl;
		cout << "Patient's required drugs : " << endl;
		for (int i = 0; i < 10; i++)
		{
			cout << i + 1 << "-" << root->drugs[i] << endl;
		}
		cout << endl;
		cout << endl;
		InOrder(root->right);
	}
}


void Menu();
void Insert();
BTS x;
int main()
{
	string Drugs[10] = { "panadol" , "panadol extra" , "panadol cold&flu" , "actifed" , "evastine" , "power caps" , "abimol" , "cast" ,"ventoline" , "orilex" };
	x.Insert(5, "Moaz", 19, Drugs);
	x.Insert(6, "maged", 29, Drugs);
	x.Insert(4, "mostafa", 24, Drugs);
	x.Insert(9, "yousef", 16, Drugs);
	x.Insert(20, "medhat", 15, Drugs);
	x.Insert(7, "hassan", 50, Drugs);
	x.Insert(2, "abdallah", 35, Drugs);
	x.Insert(3, "shenawy", 42, Drugs);

	Menu();


    return 0;
}

void Menu()
{
	cout << "1-Insert A patient" << endl;
	cout << "2-Delete A patient record" << endl;
	cout << "3-Search for a record" << endl;
	cout << "4-Display all records" << endl;
	int choice;
	cin >> choice;



	if (choice == 1)
	{
		Insert();
	}
	else if (choice == 2)
	{
		system("CLS");
		int IDNumberD;
		cout << "Please Enter the patient ID you want to Delete." << endl;
		cin >> IDNumberD;
		x.Delete(IDNumberD);
		system("CLS");
		Menu();

	}
	else if (choice == 3)
	{
		system("CLS");
		int IDNumberS;
		cout << "Please Enter the patient ID you want to Search About." << endl;
		cin >> IDNumberS;
		x.Search(IDNumberS);
		if (_getch())
		{
			system("CLS");
			Menu();
		}
	}
	else
	{
		system("CLS");
		x.InOrder(x.root);
		if (_getch())
		{
			system("CLS");
			Menu();
		}
	}
}

void Insert()
{

	int ID;
	string name;
	int age;
	string Drugs[10] = { "panadol" , "panadol extra" , "panadol cold&flu" , "actifed" , "evastine" , "power caps" , "abimol" , "cast" ,"ventoline" , "orilex" };


	system("CLS");
	cout << "Please Enter the patient information." << endl;
	cout << "ID : ";
	cin >> ID;
	cout << endl;
	cout << "Name : ";
	cin >> name;
	cout << endl;
	cout << "Age : ";
	cin >> age;
	cout << endl;
	x.Insert(ID, name, age, Drugs);

	system("CLS");
	Menu();
}
