1. 复杂链表的深赋值

   ==解决问题的关键：按照正常思路想，中途遇到了问题，然后挖掘这个问题的本质，然后解决它==

   random导致不好复制，因为random要指向的节点暂时还没有复制出来，所以解决方法是，先把所有节点都复制出来，并保持原有的相对位置

   使用链表拼接、拆分的方法

   https://www.nowcoder.com/practice/f836b2c43afc4b35ad6adc41ec941dba?tpId=13&tqId=23254&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking

   ```cpp
   /*
   struct RandomListNode {
       int label;
       struct RandomListNode *next, *random;
       RandomListNode(int x) :
               label(x), next(NULL), random(NULL) {
       }
   };
   */
   class Solution {
   public:
       RandomListNode* Clone(RandomListNode* pHead) {
           
           if(!pHead){return pHead;}
           // 复制节点
           RandomListNode* cur = pHead;
           while(cur){
               RandomListNode* tmp = new RandomListNode(cur->label);
               tmp -> next = cur -> next;
               cur -> next = tmp;
               cur = tmp -> next;
           }
           //复制random箭头
           RandomListNode* old = pHead;
           RandomListNode* clone = pHead -> next;
           RandomListNode* ret = pHead -> next;
           while(old){
               clone->random = (old -> random == NULL ) ? NULL : old -> random -> next;
               if(old -> next)old = old -> next -> next;
               if(clone -> next) clone = clone -> next -> next;
           }
           // 拆分链表
           old = pHead; 
           clone = pHead ->next;
           while(old){
               if(old -> next) old -> next = old -> next -> next;
               if( clone -> next) clone -> next = clone -> next -> next;
               old = old -> next;
               clone = clone -> next;
           }
           return ret;
       }
   };
   ```

   