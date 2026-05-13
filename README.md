# Financial-Loan-Management-Application.
inancial Loan Management Application is a C++-based system designed to manage borrowing and lending activities between users efficiently. The application uses a Binary Search Tree (BST) data structure to store and organize user records for fast searching, insertion, and management operations.
#include <iostream>
using namespace std;

class trans
{
public:
	int trans_id, b_userid, l_userid, dayborrow, dayallowed;
	double borrrow_amt, fine, returned_amount;
		trans * next;

		trans(int tid, int bid, int	lid, int bdays, int allowday, double bamt, double fi)
		{
			trans_id = tid;
			b_userid = bid;
			l_userid = lid;
			dayborrow = bdays;
			dayallowed = allowday;
			borrrow_amt = bamt;
			fine = fi;
			returned_amount = 0;
			next = NULL;
		}

		double calculatefine(int currentday)
		{
			if (currentday > dayborrow + dayallowed)
			{
				return borrrow_amt + borrrow_amt * fine / 100.0;
			}
			return borrrow_amt;
		}

};
class user
{
public:
	int userid;
	string name;
	bool isactive;
	trans* borrow_tran;
	trans* lent_tran;
	user* left;
	user* right;

	user(int id,string naam)
	{
		userid = id;
		name = naam;
		isactive = true;
		borrow_tran = NULL;
		lent_tran = NULL;
	    left = NULL;
		right = NULL;
	}


};

class bst
{
private :
	user* root ;

	user* adduser(user* node, int id, string naam)
	{
		if (node == NULL)
		{
			return new user(id, naam);
		}
		if (id < node->userid)
		{
			node->left = adduser(node->left,id, naam);
		}
		else if (id > node->userid)
		{
			node->right = adduser(node->right, id, naam);
		}
		else
		{
			node->name = naam;
		} return node;
	}
	user* search(user* node, int id)
	{
		if (node == NULL || node->userid == id)
		{
			return node;
		}
		if (id < node->userid)
		{
			return search( node->left, id);
		}
		
		else
		{
			return search(node->right, id);
		}
	}
	void inorder(user* node)
	{
		if (node == NULL)
		{
			cout << "there is no data" << endl;
			return;
		}
		inorder(node->left);
		cout << "the user id is" << node->userid << endl;
		cout << "the name is " << node->name << endl;
		cout << "the status of user (active or inactive)" << node->isactive << endl;

		inorder(node->right);
	}
public:
	bst()
	{
		root = NULL;
	}
	void adduser(int id, string naam)
	{
		 root = adduser(root,id, naam);
	}
	void updateuser(int id, string newname)
	{
		user* u = search(root, id);
		if (u != NULL && u->isactive)
		{
			u->name = newname;
			cout << "the user is updated successfully" << endl;
		}
		else
			cout << "there is no such user or user is inactive" << endl;
	}
	void deleteuser(int id)
	{
		user* u = search(root, id);
		if (u != NULL && u->isactive)
		{
			u->isactive = false;
			cout << "the user has been marked inactive" << endl;
		}
		else
			cout << "user not found or already in active" << endl;

	}
	user* searchuser(int id)
	{
		user* u = search(root, id);
		if (u == NULL || !u->isactive)
		{
			return 0;
		}
		return u;
	}
	void userlist()
	{
		cout << "the users are " << endl;
		inorder(root);
		cout << "   " << endl;
	}

};
class loansys
{
	bst user;
	trans* alltrans;
	int nexttrans_id;
	void addtrans(trans*& head, trans* newtrans)
	{
		if (head == NULL)
		{
			head = newtrans;
			return;
		}
		trans* temp = head;
		while (temp->next != NULL)
		{
			temp = temp->next;
		}
		temp->next = newtrans;
	}
public:
	loansys()
	{
		alltrans = NULL;
		nexttrans_id = 1;
	}
	void adduser(int id, string naam)
	{	user.adduser(id, naam);	}
	void updateuser(int id, string newname)
	{	user.updateuser(id, newname);	}
	void deleteuser(int id)
	{	user.deleteuser(id);	}
	void searchuser(int id)
	{	user.searchuser(id);	}
	void userlist()
	{  user.userlist();         }

}; 
