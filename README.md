# c0c 編譯器

```
PS D:\ccc\book\c0c\code> make
gcc -O0 -std=gnu99 -Wall -Werror c0c/c0c.c c0c/compiler.c c0c/lexer.c c0c/scan.c util/map.c util/strTable.c util/util.c vm/vm.c x86/x86.c -o ../c0c

PS D:\ccc\book\c0c> ./c0c test/sum
#include <stdio.h>

int sum(int n) {
  int s = 0, i = 1;
  printf("i=%d s=%d n=%d\n", i, s, n);
  while (i <= n) {
    s = s + i;
    printf("i=%d s=%d\n", i, s);
    i = i + 1;
  }
  return s;
}

int main() {
  // int total = sum(10);
  // printf("sum(10)=%d\n", total);
  printf("sum(10)=%d\n", sum(10));
}

================= lex =================
0001:#          line=1 pos=1 type=Char
0002:include    line=1 pos=8 type=Id
0003:<          line=1 pos=10 type=Char
0004:stdio      line=1 pos=15 type=Id
0005:.          line=1 pos=16 type=Char
0006:h          line=1 pos=17 type=Id
0007:>          line=1 pos=18 type=Char
0008:int        line=3 pos=3 type=Type
0009:sum        line=3 pos=7 type=Id
0010:(          line=3 pos=8 type=Char
0011:int        line=3 pos=11 type=Type
0012:n          line=3 pos=13 type=Id
0013:)          line=3 pos=14 type=Char
0014:{          line=3 pos=16 type=Char
0015:int        line=4 pos=5 type=Type
0016:s          line=4 pos=7 type=Id
0017:=          line=4 pos=9 type=Char
0018:0          line=4 pos=11 type=Number
0019:,          line=4 pos=12 type=Char
0020:i          line=4 pos=14 type=Id
0021:=          line=4 pos=16 type=Char
0022:1          line=4 pos=18 type=Number
0023:;          line=4 pos=19 type=Char
0024:printf     line=5 pos=8 type=Id
0025:(          line=5 pos=9 type=Char
0026:"i=%d s=%d n=%d\n" line=5 pos=27 type=Literal
0027:,          line=5 pos=28 type=Char
0028:i          line=5 pos=30 type=Id
0029:,          line=5 pos=31 type=Char
0030:s          line=5 pos=33 type=Id
0031:,          line=5 pos=34 type=Char
0032:n          line=5 pos=36 type=Id
0033:)          line=5 pos=37 type=Char
0034:;          line=5 pos=38 type=Char
0035:while      line=6 pos=7 type=Id
0036:(          line=6 pos=9 type=Char
0037:i          line=6 pos=10 type=Id
0038:<=         line=6 pos=13 type=Char
0039:n          line=6 pos=15 type=Id
0040:)          line=6 pos=16 type=Char
0041:{          line=6 pos=18 type=Char
0042:s          line=7 pos=5 type=Id
0043:=          line=7 pos=7 type=Char
0044:s          line=7 pos=9 type=Id
0045:+          line=7 pos=11 type=Char
0046:i          line=7 pos=13 type=Id
0047:;          line=7 pos=14 type=Char
0048:printf     line=8 pos=10 type=Id
0049:(          line=8 pos=11 type=Char
0050:"i=%d s=%d\n" line=8 pos=24 type=Literal
0051:,          line=8 pos=25 type=Char
0052:i          line=8 pos=27 type=Id
0053:,          line=8 pos=28 type=Char
0054:s          line=8 pos=30 type=Id
0055:)          line=8 pos=31 type=Char
0056:;          line=8 pos=32 type=Char
0057:i          line=9 pos=5 type=Id
0058:=          line=9 pos=7 type=Char
0059:i          line=9 pos=9 type=Id
0060:+          line=9 pos=11 type=Char
0061:1          line=9 pos=13 type=Number
0062:;          line=9 pos=14 type=Char
0063:}          line=10 pos=3 type=Char
0064:return     line=11 pos=8 type=Id
0065:s          line=11 pos=10 type=Id
0066:;          line=11 pos=11 type=Char
0067:}          line=12 pos=1 type=Char
0068:int        line=14 pos=3 type=Type
0069:main       line=14 pos=8 type=Id
0070:(          line=14 pos=9 type=Char
0071:)          line=14 pos=10 type=Char
0072:{          line=14 pos=12 type=Char
0073:printf     line=17 pos=8 type=Id
0074:(          line=17 pos=9 type=Char
0075:"sum(10)=%d\n" line=17 pos=23 type=Literal
0076:,          line=17 pos=24 type=Char
0077:sum        line=17 pos=28 type=Id
0078:(          line=17 pos=29 type=Char
0079:10         line=17 pos=31 type=Number
0080:)          line=17 pos=32 type=Char
0081:)          line=17 pos=33 type=Char
0082:;          line=17 pos=34 type=Char
0083:}          line=18 pos=1 type=Char
============ compile =============
=============vmToAsm()==============
# extern   printf                     # printf                     #
# file     "test/sum.c"                   # "test/sum.c"                   #
# str      _Str0    "i=%d s=%d n=%d\n"          # _Str0    "i=%d s=%d n=%d\n"          #
# str      _Str3    "i=%d s=%d\n"          # _Str3    "i=%d s=%d\n"          #
# str      _Str4    "sum(10)=%d\n"          # _Str4    "sum(10)=%d\n"          #
# function sum      int               # sum      int               # 6
# param    n        int               # n        int               # P0
# local    s        int               # s        int               # L0
# =        s        0                 # L0       0                 #
# local    i        int               # i        int               # L1
# =        i        1                 # L1       1                 #
# arg      _Str0                      # $_Str0                     # 0
# arg      i                          # L1                         # 1
# arg      s                          # L0                         # 2
# arg      n                          # P0                         # 3
# local    t0                         # t0                         # L2
# call     t0       printf            # L2       _printf           #
# label    _WBEGIN1                   # _WBEGIN1                   #
# <=       t0       i        n        # L2       L1       P0       #
# jz       _WEND2   t0                # _WEND2   L2                #
# local    t1                         # t1                         # L3
# +        t1       s        i        # L3       L0       L1       #
# =        s        t1                # L0       L3                #
# arg      _Str3                      # $_Str3                     # 0
# arg      i                          # L1                         # 1
# arg      s                          # L0                         # 2
# call     t0       printf            # L2       _printf           #
# +        t1       i        1        # L3       L1       1        #
# =        i        t1                # L1       L3                #
# jmp      _WBEGIN1                   # _WBEGIN1                   #
# label    _WEND2                     # _WEND2                     #
# return   s                          # L0                         #
# -function sum                        # sum                        #
# function main     int               # main     int               # 4
# arg      10                         # 10                         # 0
# local    t0                         # t0                         # L0
# call     t0       sum               # L0       _sum              #
# arg      _Str4                      # $_Str4                     # 0
# arg      t0                         # L0                         # 1
# local    t1                         # t1                         # L1
# call     t1       printf            # L1       _printf           #
# -function main                       # main                       #
# -file    "test/sum.c"                   # "test/sum.c"                   #
PS D:\ccc\book\c0c> gcc test/sum0.c -o test/sum0
gcc.exe: error: test/sum0.c: No such file or directory
gcc.exe: fatal error: no input files
compilation terminated.
PS D:\ccc\book\c0c> gcc test/sum0.s -o test/sum0
PS D:\ccc\book\c0c> ./test/sum0
i=1 s=0 n=10
i=1 s=1
i=2 s=3
i=3 s=6
i=4 s=10
i=5 s=15
i=6 s=21
i=7 s=28
i=8 s=36
i=9 s=45
i=10 s=55
sum(10)=55
```