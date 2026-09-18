# Binary-Search-Tree.........-
#include <iostream>
using namespace std;

template <typename T>
class BST
{
private:
    // BST Node
    struct Node
    {
        T data;
        Node* left;
        Node* right;

        Node(T value)
        {
            data = value;
            left = nullptr;
            right = nullptr;
        }
    };

    Node* root;

    // Insert helper
    Node* insert(Node* node, T value)
    {
        if (node == nullptr)
        {
            return new Node(value);
        }

        if (value < node->data)
        {
            node->left = insert(node->left, value);
        }
        else if (value > node->data)
        {
            node->right = insert(node->right, value);
        }
        else
        {
            cout << "Duplicate value not inserted: " << value << endl;
        }

        return node;
    }

    // Search helper
    bool search(Node* node, T value)
    {
        if (node == nullptr)
        {
            return false;
        }

        if (node->data == value)
        {
            return true;
        }

        if (value < node->data)
        {
            return search(node->left, value);
        }

        return search(node->right, value);
    }

    // Find minimum node
    Node* findMin(Node* node)
    {
        while (node != nullptr && node->left != nullptr)
        {
            node = node->left;
        }

        return node;
    }

    // Delete helper
    Node* remove(Node* node, T value)
    {
        if (node == nullptr)
        {
            return nullptr;
        }

        if (value < node->data)
        {
            node->left = remove(node->left, value);
        }
        else if (value > node->data)
        {
            node->right = remove(node->right, value);
        }
        else
        {
            // Case 1: No child
            if (node->left == nullptr && node->right == nullptr)
            {
                delete node;
                return nullptr;
            }

            // Case 2: Only right child
            if (node->left == nullptr)
            {
                Node* temp = node->right;
                delete node;
                return temp;
            }

            // Case 3: Only left child
            if (node->right == nullptr)
            {
                Node* temp = node->left;
                delete node;
                return temp;
            }

            // Case 4: Two children
            Node* temp = findMin(node->right);
            node->data = temp->data;
            node->right = remove(node->right, temp->data);
        }

        return node;
    }

    // In-order traversal
    void inOrder(Node* node)
    {
        if (node == nullptr)
        {
            return;
        }

        inOrder(node->left);
        cout << node->data << " ";
        inOrder(node->right);
    }

    // Pre-order traversal
    void preOrder(Node* node)
    {
        if (node == nullptr)
        {
            return;
        }

        cout << node->data << " ";
        preOrder(node->left);
        preOrder(node->right);
    }

    // Post-order traversal
    void postOrder(Node* node)
    {
        if (node == nullptr)
        {
            return;
        }

        postOrder(node->left);
        postOrder(node->right);
        cout << node->data << " ";
    }

    // Destructor helper
    void destroy(Node* node)
    {
        if (node == nullptr)
        {
            return;
        }

        destroy(node->left);
        destroy(node->right);

        delete node;
    }

public:

    // Constructor
    BST()
    {
        root = nullptr;
    }

    // Insert
    void insert(T value)
    {
        root = insert(root, value);
    }

    // Search
    bool search(T value)
    {
        return search(root, value);
    }

    // Delete
    void remove(T value)
    {
        if (!search(value))
        {
            cout << "Value not found: " << value << endl;
            return;
        }

        root = remove(root, value);
        cout << "Deleted: " << value << endl;
    }

    // In-order
    void inOrder()
    {
        cout << "In-order: ";
        inOrder(root);
        cout << endl;
    }

    // Pre-order
    void preOrder()
    {
        cout << "Pre-order: ";
        preOrder(root);
        cout << endl;
    }

    // Post-order
    void postOrder()
    {
        cout << "Post-order: ";
        postOrder(root);
        cout << endl;
    }

    // Destructor
    ~BST()
    {
        destroy(root);
        root = nullptr;
    }
};


int main()
{
    BST<int> tree;

    cout << "=== Binary Search Tree ===" << endl;

    // Insert values
    tree.insert(50);
    tree.insert(30);
    tree.insert(70);
    tree.insert(20);
    tree.insert(40);
    tree.insert(60);
    tree.insert(80);

    // Display traversals
    cout << "\nTree Traversals:" << endl;

    tree.inOrder();
    tree.preOrder();
    tree.postOrder();

    // Search
    cout << "\nSearch Operation:" << endl;

    int value = 40;

    if (tree.search(value))
    {
        cout << value << " found in the BST." << endl;
    }
    else
    {
        cout << value << " not found in the BST." << endl;
    }

    value = 100;

    if (tree.search(value))
    {
        cout << value << " found in the BST." << endl;
    }
    else
    {
        cout << value << " not found in the BST." << endl;
    }

    // Delete
    cout << "\nDelete Operation:" << endl;

    tree.remove(20);
    tree.inOrder();

    tree.remove(30);
    tree.inOrder();

    tree.remove(50);
    tree.inOrder();

    // Final traversals
    cout << "\nFinal Tree Traversals:" << endl;

    tree.inOrder();
    tree.preOrder();
    tree.postOrder();

    return 0;
}