# Fix My Code Challenge

مشروع نوعه مختلف: ما نكتب الكود من الصفر، ندخل على كود موجود ومكسور ونصلّح
الخطأ **فقط**. التحدي الحقيقي هو قراءة كود غيرك وفهم نيّته قبل ما تغيّر فيه.

## الأخطاء وإصلاحاتها

### `challenge/0-fizzbuzz.py` — Python

الترتيب الخاطئ للشروط جعل فرع `FizzBuzz` **غير قابل للوصول**: أي رقم يقبل
القسمة على 3 و 5 معاً يقبل القسمة على 3، فيمسكه الشرط الأول دائماً.

```python
if (i % 3) == 0:                              # 15 يدخل هنا
elif (i % 3) == 0 and (i % 5) == 0:           # هذا الفرع لا يُنفَّذ أبداً
```

الإصلاح: اختبار الشرط **الأخص** أولاً.

```python
if (i % 3) == 0 and (i % 5) == 0:
elif (i % 3) == 0:
elif (i % 5) == 0:
```

### `challenge/1-print_square.js` — JavaScript

`parseInt` كان يقرأ الوسيط بالأساس **السادس عشر** بدل العشري، فصار `10` يساوي 16.

```js
size = parseInt(process.argv[2], 16);   // قبل
size = parseInt(process.argv[2], 10);   // بعد
```

### `challenge/2-sort.rb` — Ruby

الإدراج كان في الموضع `i - 1` بدل `i`، فينزل العنصر خانة قبل مكانه الصحيح
ويخرّب الترتيب.

```ruby
result.insert(i - 1, i_arg)   # قبل
result.insert(i, i_arg)       # بعد
```

### `challenge/3-user.py` — Python

خطآن:

1. الـ setter يكتب في `self._password` (شرطة واحدة) بدل `self.__password`،
   فينشئ خاصية جديدة تماماً ويبقى `__password` فاضي إلى الأبد.
2. `is_valid_password` يقارن تجزئة بحروف **كبيرة** مع تجزئة مخزّنة بحروف
   **صغيرة**، فالمقارنة تفشل حتى مع كلمة المرور الصحيحة.

```python
self._password = ... .lower()                 # قبل
self.__password = ... .lower()                # بعد

return ... .hexdigest().upper() == self.__password    # قبل
return ... .hexdigest().lower() == self.__password    # بعد
```

### `challenge/4-delete_dnodeint/delete_dnodeint_at_index.c` — C

الفرع الخاص بالحذف من منتصف القائمة كان فيه ثلاث مشاكل:

1. `(*head)->prev->prev = (*head)->prev;` — يعدّل المؤشر الخطأ؛ المطلوب ربط
   `prev->next` بالعقدة التالية.
2. **use-after-free**: ينادي `free(*head)` ثم يقرأ `(*head)->next` بعدها.
3. ما فيه فحص لـ `*head == NULL` بعد التنقّل، فحذف عند فهرس يساوي طول القائمة
   يؤدي إلى segfault.

```c
(*head)->prev->next = (*head)->next;
if ((*head)->next != NULL)
	(*head)->next->prev = (*head)->prev;
free(*head);                     /* التحرير بعد إعادة الربط، لا قبله */
*head = saved_head;
```

## التحقق

| الملف | طريقة التحقق |
| --- | --- |
| `0-fizzbuzz.py` | شغّلته على 50 — الآن 15 و 30 و 45 تطبع `FizzBuzz` |
| `1-print_square.js` | الحجم 10 يطبع 10 أسطر × 10 محارف |
| `2-sort.rb` | `12 41 2 C 9 -9 31 fun -1 32` ← مرتّبة تصاعدياً مع تجاهل غير الأرقام |
| `3-user.py` | المخرجات صارت `Test User` فقط، بدون أي رسالة خطأ |
| `4-delete_dnodeint` | يُترجم بـ `-Wall -pedantic -Werror -Wextra -std=gnu89` بلا تحذيرات، ونظيف تحت AddressSanitizer و UBSan |

## الترجمة والتشغيل

```bash
./0-fizzbuzz.py 50
./1-print_square.js 10
ruby 2-sort.rb 12 41 2 C 9 -9 31 fun -1 32
./3-user.py
cd 4-delete_dnodeint
gcc -Wall -pedantic -Werror -Wextra -std=gnu89 main.c free_dlistint.c \
    print_dlistint.c add_dnodeint_end.c delete_dnodeint_at_index.c -o delete_dnodeint
./delete_dnodeint
```
