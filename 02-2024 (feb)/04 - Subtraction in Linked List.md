## 05. Sorted insert for circular linked list
The problem can be found at the following link: [Question Link](https://www.geeksforgeeks.org/problems/subtraction-in-linked-list/1)

### My Approach
- Defining reverse and subtract functions for linked list manipulation.
-  In subtract, iterate through linked lists, subtracting corresponding node values.
- Address borrowing cases by adjusting the subtraction and updating the carry.
- Create a new linked list to store subtraction results.
- Implement subLinkedList to subtract one list from another.
- Reverse input lists, subtract, handle borrow, then reverse result, and trim leading zeros before returning.

### Time and Auxiliary Space Complexity
- **Time Complexity**: O(n) - where n is the maximum number of nodes between the two lists.
- **Auxiliary Space Complexity**: O(n) - where n is the maximum number of nodes between the two input linked lists.

### Code (C++)
```cpp
class Solution {
public:
    Node * reverse(Node * head) {
        Node * prev = nullptr, * cur = head;
        
        while(cur){
            Node * next = cur -> next;
            cur -> next = prev;
            prev = cur;
            cur = next;
        }
        
        return prev;
    }
    
    pair<Node *, Node *> subtract(Node * head1, Node * head2) {
        LinkedList ans, second;
        int carry = 0;
        
        while(head1 or head2){
            int cur = (head1 ? head1 -> data : 0) - (head2 ? head2 -> data : 0) - carry;
            carry = cur < 0;
            
            if(carry){
                if((head1 and head1 -> next) or (head2 and head2 -> next)){
                    ans.insert(10 + cur);
                    second.insert(0);
                }
                else{
                    second.insert(abs(cur));
                }
            }
            else{
                ans.insert(cur);
                second.insert(0);
            }
            
            if(head1)
                head1 = head1 -> next;
            if(head2)
                head2 = head2 -> next;
        }
        
        
        if(carry){
            return {second.head, ans.head};
        }
        else{
            return {ans.head, nullptr}; // nullptr indicate that F >= S
        }  
    }
    
    Node* subLinkedList(Node* head1, Node* head2) {
        head1 = reverse(head1);
        head2 = reverse(head2);
        
        pair<Node *, Node *> res = subtract(head1, head2);
        Node * final_ans;
        
        if(res.second){
            head1 = res.first;
            head2 = res.second;
            
            final_ans = subtract(head1, head2).first;
        }
        else{
            final_ans = res.first;
        }
        
        final_ans = reverse(final_ans);
        
        while(final_ans -> data == 0 and final_ans -> next)
            final_ans = final_ans -> next;
        
        return final_ans;
    }
};
```
