<!--
 * @Author: Hiseh
 * @Date: 2019-10-09 10:24:36
 * @LastEditors: Hiseh
 * @LastEditTime: 2019-12-12 17:58:20
 * @Description: 
 -->
# 考场就座 
[返回首页](../README.md)

在考场里，一排有**N**个座位，分别编号为`0, 1, 2, ..., N-1`。

当学生进入考场后，他必须坐在能够使他与离他最近的人之间的距离达到最大化的座位上。如果有多个这样的座位，他会坐在编号最小的座位上。(另外，如果考场里没有人，那么学生就坐在 0 号座位上。)

因为一开始就告诉了教室大小，所以可以用数组来存储学生座位，每次就座时通过二分法查找座位，这种方式效率高*ϴ(ln<sup>N</sup>)*，但空间占用大*ϴ(N)*。考虑到学生就座次数比座位数少5个数量级（*1 <= N <= 10<sup>9</sup>*，*seat < 10<sup>4</sup>*），完全可以用稀疏矩阵实现，时间复杂度高*ϴ(N)*，但更节约内存。
## Python
采用第二种思路
```python
class ExamRoom:
    def __init__(self, N: int):
        self.N = N
        self.students = set()

    def seat(self, k) -> int:
        seat_no = 0

        if not self.students:
            # 第一个人坐在0
            self.students.add(seat_no)
        elif len(self.students) == 1:
            # 如果就一人，另一人需要坐在两头
            curr_no = next(iter(self.students))
            seat_no = 0 if curr_no > self.N - 1 - curr_no else self.N - 1
            self.students.add(seat_no)
        else:
            tmp_seats = sorted(self.students)

            # 获取当前已有座位号中，间隔最大的座位号
            # 对于相邻的两个座位e[i]和e[i+1]，这两个座位之间入座，那么最近的距离为(e[i+1] - e[i]) / 2
            # 比较这个距离，返回距离最大的e[i]和e[i+1]
            min_no, max_no = max(zip(tmp_seats, tmp_seats[1:]), key=lambda e: (e[1] - e[0]) // 2)

            if 0 not in self.students and tmp_seats[0] >= (max_no - min_no) // 2:
                # 0没有人的情况
                seat_no = 0
            elif self.N - 1 not in self.students and self.N - 1 - tmp_seats[-1] > (max_no - min_no) // 2:
                # N-1没有人的情况
                seat_no = self.N - 1
            else:
                # 正常情况
                seat_no = min_no + (max_no - min_no) // 2

            self.students.add(seat_no)

        return seat_no

    def leave(self, p: int) -> None:
        self.students.remove(p)
```
---

## C
参考python思路，用链表实现
```c
#include <stdlib.h>

typedef struct ExamRoom {
    int seat_no;
    struct ExamRoom* next;
} ExamRoom;

ExamRoom* examRoomCreate(int N) {
    // 把总座位数存在链表第一个元素里
    
    ExamRoom* root = (ExamRoom*)malloc(sizeof(ExamRoom));
    if (root) {
        root->seat_no = N;
        root->next = NULL;
    }
    return root;
}

void insertSeat(ExamRoom** root, int seat_no) {
    // 按顺序插入链表
    
    ExamRoom* tmp = (ExamRoom*)malloc(sizeof(ExamRoom));
    if (tmp) {
        tmp->seat_no = seat_no;
        tmp->next = NULL;
        ExamRoom* obj = *root;
        while (obj->next != NULL) {
            if (obj->next->seat_no > seat_no) {
                tmp->next = obj->next;
                obj->next = tmp;
                return;
            }
            obj = obj->next;
        }

        // 最坏可能
        obj->next = tmp;
    }
}

int examRoomSeat(ExamRoom* obj) {
    // 思路同python
    
    int seat_no = 0;
    if (obj->next == NULL) {
        // 第一个人坐在0
    } else if (obj->next->next == NULL) {
        // 如果就一人，另一人需要坐在两头
        ExamRoom* curr_seat = obj->next;
        seat_no = curr_seat->seat_no > obj->seat_no - 1 - curr_seat->seat_no
                      ? 0
                      : obj->seat_no - 1;
    } else {
        int min_no = 0;
        int max_no = obj->seat_no - 1;
        int max_rng = 0;
        int last_no = 0;

        // 获取最大间隔
        ExamRoom* tmp = obj->next;
        while (tmp->next != NULL) {
            if ((tmp->next->seat_no - tmp->seat_no) / 2 > max_rng) {
                min_no = tmp->seat_no;
                max_no = tmp->next->seat_no;
                max_rng = (max_no - min_no) / 2;
            }
            last_no = tmp->next->seat_no;

            tmp = tmp->next;
        }

        // 检查两头没人的情况
        if (obj->next->seat_no != 0 &&
            obj->next->seat_no >= max_rng) {
            seat_no = 0;
        } else if (obj->seat_no - 1 != last_no &&
                   obj->seat_no - 1 - last_no > max_rng) {
            seat_no = obj->seat_no - 1;
        } else {
            seat_no = min_no + max_rng;
        }
    }

    insertSeat(&obj, seat_no);
    return seat_no;
}

void examRoomLeave(ExamRoom* obj, int seat_no) {
    ExamRoom* root = obj;
    while (root->next != NULL) {
        if (root->next->seat_no == seat_no) {
            ExamRoom* tmp = root->next;
            root->next = tmp->next;
            free(tmp);
            break;
        }

        root = root->next;
    }
}

void examRoomFree(ExamRoom* obj) {
    while (obj != NULL) {
        ExamRoom* tmp = obj;
        obj = obj->next;
        free(tmp);
    }
}
```
[返回首页](../README.md)