# Remove Nth Node From End of List

> Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

>**Input:** head = [1,2,3,4,5], n = 2
 **Output:** [1,2,3,5]

> **Input:** head = [1], n = 1
**Output:** []

> **Input:** head = [1,2], n = 1
**Output:** [1]

> **Follow up:** Could you do this in one pass?

It was expected from the start that it was possible in one pass. Yet, most beginners will start their approach with two pass.

A few points of observations:
1. Consider $[1,2,3,4,5]$. Since n = 2, notice that we will first start counting from '5' then go down to '4'. Thus, when we start from '5' we have reached the first node from the last, then when we reach to '4', we have reached the second node from last

#### My first approach.
- Initialize temp as a node pointer and assign it to head.
- First find the length of the linked list as it was not present which will be our first pass
	- while temp!=Null {temp = temp→next}
- Initialize another pointer temp2 and assign it to head
- Then iterate just before the node we want to remove and do temp→next = temp→next→next in effect removing the node
	- Note that removing should mean we delete its memory reference, but this is competitive programming

#### A few optimizations:

1. Note that there is no need to declare temp and temp2 as we can do with just one node pointer called iter
	- We can continuously just change its value. As we are giving two passes, we can just change its value for each individual pass.

```c++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode * temp = head;
        int length = 0, i = 1;
        while(temp)
        {
            temp = temp->next;
            length++;
        }
        if(length == n){
            head = head->next;
            return head;
        }
        for(temp = head;i<length-n;i++)
        {
                temp = temp->next;
        }
        temp->next = (temp->next)->next;
        return head;
        
        
    }
};
```


#### Optimized Approach
This is an approach which deals with everything in just one pass of our linked list.![[Pasted image 20220929170515.png]]

Note, that 'x' is the number of nodes, and it's counting ends just before the counting of n starts. Similarly, 'n' ends at the end of the list

So let's say one of our pointer called as 'ahead' starting from the head reached till the last node in 'x'. It will have only 'n' number of nodes till we reach the end node.

Now in our first approach we had to calculate the length of the linked list. The question now arises is how to do the same problem without getting the length of the linked list and to do it in only one pass.

This is where we introduce the two pointers - ahead and behind.

To remove the nth node from the end of the linked list, we need to travel to the (x-1)th node. Thus , if our pointer starts from the head, it will have to go over the x-1 iterations.

The first step of letting the ahead pointer travel L-n steps is this

![[Drawing 2022-09-30 19.47.31.excalidraw]]

Note the last step, since we have travelled in L-n nodes, what has happened is that to reach the node just before the node we want to remove, it will take x-1 steps, which travelled would lead to the ahead pointer at the end of the list and the behind pointer to the node we want to go (just behind the node we want to remove)

This particular implementation of travelling x-1 steps by letting the ahead pointer travel the L-n nodes would help us in looping.

```c++
class Solution {

public:

    ListNode* removeNthFromEnd(ListNode* head, int n) {

    ListNode *fast = head, *slow = head;

    while(n--) fast = fast -> next;      

    if(!fast) return head -> next;    //If head is already null, remove the first node  

    while(fast -> next)                  

        fast = fast -> next, slow = slow -> next;            

    slow -> next = slow -> next -> next;

    return head;

}

};
```