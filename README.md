# 暑期資料結構期中報告 學生:11224123羅聖幃 11224125吳泰安


## 題目3:Python實現雙向鍊表

### 環形雙向鍊表
環形雙向鏈表是一種資料結構，它將每個節點雙向連接，並且將最後一個節點和第一個節點連接起來，形成一個環。
簡單比喻：它就像一條首尾相連、可以雙向行駛的環狀公路。

## 圖例說明
![01](https://github.com/lshengwei048/-/blob/main/%E5%9C%96%E7%89%871.png)

## 代碼作用

## 程式碼
class Player:
    """節點類"""
    def __init__(self):
        """初始化姓名、分數、指針"""
        self.name = ''
        self.score = 0
        self.rlink = None
        self.llink = None


def ergodic(head, num=None, is_print=False, left=False):
    """遍歷函數，num是遍歷到哪一個位置序號，is_print是否觸發打印方法，left表示是否由head開始往左遍歷"""
    ptr = head
    count = 0
    while True:
        if num == count:
            break

        if not left:
            if ptr.rlink != head:
                ptr = ptr.rlink
            else:
                break
        else:
            if ptr.llink != head:
                ptr = ptr.llink
            else:
                break
        count += 1
        if is_print:
            print('No.' + str(count), 
                  ptr.llink.name if ptr.llink != head else 'head', '<---', 
                  ptr.name, ptr.score, '--->', 
                  ptr.rlink.name if ptr.rlink != head else 'head')
    return ptr  # 返回遍歷完成的最後一個節點


head = Player()  # 初始化一個鏈表基點，不用存放任何數據
head.rlink = head  # 初始化右指針
head.llink = head  # 初始化左指針


while True:
    select = input("(1).新增  (2).查看  (3).插入  (4).刪除  (5).離開\n輸入: ")
    if select == "1":  # 新增節點，分為右新增和左新增
        direction = input("(1).右新增  (2).左新增\n輸入: ")
        if direction not in ("1", "2"):
            print("輸入錯誤")
            continue
        new_data = Player()
        new_data.name = input("姓名: ")
        new_data.score = input("分數: ")
        if direction == "1":  # 右新增
            ptr = ergodic(head)  # 從head開始向右遍歷獲取最後一個節點
            ptr.rlink = new_data
            new_data.llink = ptr
            new_data.rlink = head
            head.llink = new_data
        else:  # 左新增
            ptr = ergodic(head, left=True)  # 從head開始向左遍歷獲取最後一個節點
            ptr.llink = new_data
            new_data.rlink = ptr
            new_data.llink = head
            head.rlink = new_data

    elif select == "2":  # 遍歷查看所有節點，分為右遍歷和左遍歷
        direction = input("(1).右遍歷  (2).左遍歷\n輸入: ")
        if direction == "1":  # 右遍歷
            ergodic(head, is_print=True)
        elif direction == "2":  # 左遍歷
            ergodic(head, is_print=True, left=True)
        else:
            print("輸入錯誤")

    elif select == "3":  # 插入節點，分為右插入和左插入
        direction = input("(1).右插入  (2).左插入\n輸入: ")
        if direction not in ("1", "2"):
            print("輸入錯誤")
            continue
        try:
            num = int(input("請輸入需要插入的節點位置序號: "))  # 輸入序號必須是大於0的整數，如果輸入大於最後一個節點的序號
            if num < 1:
                print("輸入必須為大於0的正整數")
                continue
        except ValueError:
            print("輸入有誤")
            continue
        insert_data = Player()
        insert_data.name = input("姓名: ")
        insert_data.score = input("分數: ")
        if direction == "1":  # 右插入
            ptr = ergodic(head, num - 1)  # 獲取需要插入位置的前一個節點，新插入的節點就在這個節點的後面
            insert_data.llink = ptr
            insert_data.rlink = ptr.rlink
            ptr.rlink.llink = insert_data
            ptr.rlink = insert_data
        else:  # 左插入
            ptr = ergodic(head, num - 1, left=True)
            insert_data.rlink = ptr
            insert_data.llink = ptr.llink
            ptr.llink.rlink = insert_data
            ptr.llink = insert_data

    elif select == "4":  # 刪除節點，分為右刪除和左刪除
        direction = input("(1).右刪除  (2).左刪除\n輸入: ")
        if direction not in ("1", "2"):
            print("輸入錯誤")
            continue
        try:
            num = int(input("請輸入需要刪除的節點序號: "))
            if num < 1:
                print("輸入必須為大於0的正整數")
                continue
        except ValueError:
            print("輸入有誤")
            continue
        if direction == "1":  # 右刪除
            ptr = ergodic(head, num)          
        else:  # 左刪除
            ptr = ergodic(head, num, left=True)
            ptr.rlink.llink = ptr.llink
            ptr.llink.rlink = ptr.rlink
    elif select == "5":
        print("成功離開")
        break
    else:
        print("輸入錯誤，請重試")
