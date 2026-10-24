# Fix My Code Challenge — challenge

خمسة أكواد مكسورة بخمس لغات مختلفة. المطلوب: نصلّح الخطأ **فقط**،
بدون إعادة كتابة.

| الملف | اللغة | الخطأ باختصار |
| --- | --- | --- |
| `0-fizzbuzz.py` | Python | ترتيب شروط خاطئ جعل فرع `FizzBuzz` ميتاً |
| `1-print_square.js` | JavaScript | `parseInt` بالأساس 16 بدل 10 |
| `2-sort.rb` | Ruby | إدراج في `i - 1` بدل `i` |
| `3-user.py` | Python | `_password` بدل `__password` + `.upper()` ضد `.lower()` |
| `4-delete_dnodeint/` | C | ربط مؤشر خاطئ + use-after-free + لا فحص NULL |

الشرح الكامل لكل خطأ موجود في الـ README بجذر المشروع.

## التشغيل

```bash
./0-fizzbuzz.py 50
# 1 2 Fizz 4 Buzz ... 14 FizzBuzz 16 ...

./1-print_square.js 10
# مربّع 10 × 10

ruby 2-sort.rb 12 41 2 C 9 -9 31 fun -1 32
# -9 -1 2 9 12 31 32 41   (غير الأرقام تُتجاهَل)

./3-user.py
# Test User          ← بدون أي رسالة خطأ

cd 4-delete_dnodeint
gcc -Wall -pedantic -Werror -Wextra -std=gnu89 main.c free_dlistint.c \
    print_dlistint.c add_dnodeint_end.c delete_dnodeint_at_index.c -o delete_dnodeint
./delete_dnodeint
```

## درس عام من المشروع

الأخطاء الخمسة كلها من نوع واحد: **الكود يعمل، لكن لا يفعل ما يظنّه كاتبه.**
لا يوجد فيها خطأ تركيبي واحد. ولهذا قراءة الكود وتشغيله على حالات طرفية
(15 في FizzBuzz، العدد 10 في المربّع، الحذف من منتصف القائمة) أهم من
قراءته سطراً سطراً.
