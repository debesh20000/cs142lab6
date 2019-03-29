#include<iostream>
using namespace std;


class Node
{
public:
  int data;
  Node *left;
  Node *right;
  Node *parent;

    Node (int value)
  {
    data = value;
    left = NULL;
    right = NULL;
    parent = NULL;
  }
};

class BST
{
public:
  Node * root;
  BST ()
  {
    root = NULL;
  }
  void insertHelper (int value)
  {
    insert (root, value);
  }
  void insert (Node * curr, int value)
  {
    // If root is NULL, then create a node and make it root. 
    if (root == NULL)
      root = new Node (value);
    // Else Decide to move left or right. 
    else if (value < curr->data)
      {
	// if left is already NULL, create a new node and link it. 
	if (curr->left == NULL)
	  {
	    curr->left = new Node (value);
	    curr->left->parent = curr;
	  }
	// Move left and call insert there. 
	else
	  insert (curr->left, value);
      }
    else
      {
	// if right is already NULL, create a new node and link it. 
	if (curr->right == NULL)
	  {
	    curr->right = new Node (value);
	    curr->right->parent = curr;
	  }
	// Move right and call insert there. 
	else
	  insert (curr->right, value);
      }
  }
  void displayHelper ()
  {
    display (root);
  }
  void display (Node * curr)
  {
    // Case for empty tree and leaf nodes children.
    if (curr == NULL)
      return;
    // IN order print.
    // Print left. 
    display (curr->left);
    // Print myself.
    cout << curr->data << ", ";
    // Print right.
    display (curr->right);
  }
  Node *searchHelper (int value)
  {
    return search (root, value);
  }
  Node *search (Node * curr, int value)
  {
    // Are you the value? or Are you NULL?  if yes return curr
    if (curr == NULL || curr->data == value)
      return curr;
    // else you search in right or left. 
    // Left
    else if (value < curr->data)
      return search (curr->left, value);
    // Right
    else
      return search (curr->right, value);
  }
  void print2DUtil (Node * root, int space)
  {
    // Base case  
    if (root == NULL)
      return;
    // Increase distance between levels  
    space += 5;
    // Process right child first  
    print2DUtil (root->right, space);

    // Print current node after space  
    // count  
    cout << endl;
    for (int i = 5; i < space; i++)
      cout << " ";
    cout << root->data << "\n";

    // Process left child  
    print2DUtil (root->left, space);
  }

  // Wrapper over print2DUtil()  
  void print2D ()
  {
    cout << "******************************" << endl;
    // Pass initial space count as 0  
    print2DUtil (root, 0);
    cout << "******************************" << endl;

  }



  void binary_delete (Node * curr, int key)
  {

  }

  int find_min (int value)
  {
    //search for a branch
    Node *current = search (root, value);
    //take the min of that branch
    Node *min = find_min1 (current);
    cout << endl;
    return min->data;
  }
  Node *find_min1 (Node * current)
  {
    //find min the recursion
    if (current->left == NULL)
      {
	return current;
      }
    else if (current == NULL)
      {
	return NULL;
      }
    //recursion carried to left
    else
      return find_min1 (current->left);
  }
  void replace_at_parent (int value1, int value2)
  {
    Node *rep = search (root, value2);
    Node *current = search (root, value1);
    replace_at_parent1 (current, rep);
  }
  void replace_at_parent1 (Node * current, Node * rep)
  {
    if (rep == root)
      {
	root = current;
      }
    else if (current == root)
      {
	return;
      }
    else
      {
	if (rep->data > rep->parent->data)
	  {
	    if (current == NULL)
	      {
		rep->parent->right = NULL;
	      }
	    else
	      {
		rep->parent->right = current;
		if (current->data > current->parent->data)
		  {
		    current->parent->right = NULL;
		  }
		else
		  current->parent->left = NULL;
		current->parent = rep->parent;
	      }

	  }
	else
	  {
	    if (current == NULL)
	      {
		rep->parent->left = NULL;
	      }
	    else
	      {
		rep->parent->left = current;
		if (current->data > current->parent->data)
		  {
		    current->parent->right = NULL;
		  }
		else
		  current->parent->left = NULL;
		current->parent = rep->parent;
	      }

	  }
      }
    print2DUtil (root, 0);
  }
  void deleteNode (int value)
  {
    //search deleting nodee
    Node *current = search (root, value);
    if (current == NULL)
      {
	return;
      }
    else
      {
	//replacing the values
	if (current->left == NULL)
	  {
	    replace_at_parent1 (current->right, current);
	  }
	else if (current->right == NULL)
	  {
	    replace_at_parent1 (current->left, current);
	  }
	else
	  {
	    Node *temp = find_min1 (current->right);
	    if (current == root)
	      {
		current->data = temp->data;
		replace_at_parent1 (NULL, temp);
		delete temp;
	      }
	    else
	      {
		replace_at_parent1 (temp, current);
		temp->parent = current->parent;
		temp->left = current->left;
		delete temp;
	      }
	  }
      }
  }
};

int
main ()
{

  BST bst1;
  bst1.insertHelper (4);
  bst1.insertHelper (2);
  bst1.insertHelper (3);
  bst1.insertHelper (1);
  bst1.insertHelper (6);
  bst1.insertHelper (5);
  bst1.insertHelper (7);
  bst1.insertHelper (8);
  bst1.displayHelper ();
  bst1.deleteNode (3);
  bst1.displayHelper ();
  cout << bst1.find_min (4) << endl;
  bst1.replace_at_parent (3, 4);
  return 0;

}
