## Step1

連結リストを前から順にたどって行き,同じノードにたどり着いたら終わり.
つまり,今まで訪れたノードを記録して,一致したらループを抜ければよさそう.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        visited = set()
        while head:
            if head in visited:
                return True
            set.add(head)
            head = head.next
        return False
```

- 時間計算量：O(n)
- 空間計算量：O(n)

空間計算量をO(1)にするために2ポインタで解く.発想が思いつかず,
https://github.com/naoto-iwase/leetcode/pull/1/commits/21cd6c5ca124e1b81b91b8003681e1a49b1c5927　
内の**ヒント**を参考にした.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        move_fast = head
        move_slow = head
        while move_fast and move_fast.next:
            move_fast = move_fast.next.next
            move_slow = move_slow.next
            if move_fast == move_slow:
                return True
        return False
```

はじめ,
```python
while move_slow and move_slow.next:
```
としており,
> AttributeError: 'NoneType' object has no attribute 'next’
> 
のエラーがでていた.それもそのはず,早く進む方を終了条件にしなければ None の next は何も返らない.

また,このwhileの条件は,.next する対象のノードが存在することを担保するものであることにも注意.

## Step2

- 命名規則
    - `move_fast`, `move_slow`  はくどいように感じるので `fast` ,`slow` に変更
- 比較演算子
    
    ```python
    if fast == slow:
    ```
    

ではオブジェクトの同一性を判定しているため, `==` では値の一致をみている印象を受けそうであるため, `is` を使った方が糸が明確に伝わりそう.

さらにGemini 3より

- 空白とインデント
    - クラス内のメソッド間や,論理的なブロックの区切りに適切な空行を挿入

``` python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        fast = head
        slow = head

        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next

            if fast is slow:
                return True

        return False
```
        
