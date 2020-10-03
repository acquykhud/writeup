# RE
## notyourleg
- Bỏ file vào IDA, ta có 1 file ARM
```cpp
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int v3; // r0
  char v4; // r5
  int v5; // r0
  unsigned int i; // [sp+4h] [bp-30h]
  char v8; // [sp+Ch] [bp-28h]
  void *v9; // [sp+24h] [bp-10h]

  v9 = &_stack_chk_guard;
  std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::basic_string(&v8, argv, envp);
  v3 = std::operator<<<std::char_traits<char>>(&_bss_start, "Please give me your legs...");
  std::ostream::operator<<(v3, &std::endl<char,std::char_traits<char>>);
  std::operator>><char,std::char_traits<char>,std::allocator<char>>(0x210F0, &v8);
  for ( i = 0; std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::length(&v8) >= i; ++i )
  {
    v4 = *(_BYTE *)std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::operator[](&v8, i);
    *(_BYTE *)std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::operator[](&v8, i) = v4 - 4;
  }
  if ( std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::compare(
         &v8,
         "D?IQO)?PBwjks[u,q[gjks[pd/[=NI[jkp[FqOP[t42y") != 0 )
    v5 = std::operator<<<std::char_traits<char>>(&_bss_start, "\nI think you don't know what is goin' on");
  else
    v5 = std::operator<<<std::char_traits<char>>(
           &_bss_start,
           "\nCongratz!!! That's is how you use your arms, not your legs!!!");
  std::ostream::operator<<(v5, &std::endl<char,std::char_traits<char>>);
  std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::~basic_string(&v8);
  return 0;
}
```
- Chương trình trừ mỗi kí tự đi 4 và so sánh với chuỗi `"D?IQO)?PBwjks[u,q[gjks[pd/[=NI[jkp[FqOP[t42y"`.
- --> Ta cộng mỗi ký tự trên với 4 sẽ ra được flag.

## CrackMe01
- File được cho là x64, linux, được viết bằng golang, vẫn còn debug symbol -> RE dễ hơn.
- Bỏ file vào IDA, (theo kinh nghiệm của mình), mở hàm `main_main` ra để đọc.
```cpp
__int64 __fastcall main_main(__int64 a1, __int64 a2, __int64 a3, __int64 a4, __int64 a5, __int64 a6)
{
  __int64 v6; // r8
  __int64 v7; // r9
  __int64 v8; // rdx
  __int64 v9; // rdx
  __int64 v10; // r8
  __int64 v11; // r9
  __int64 v12; // r8
  __int64 v13; // r9
  __int64 v14; // rdx
  __int64 result; // rax
  __int64 v16; // rdx
  __int64 v17; // r8
  __int64 v18; // r9
  __int64 v19; // [rsp+8h] [rbp-A8h]
  __int64 v20; // [rsp+10h] [rbp-A0h]
  __int64 *v21; // [rsp+50h] [rbp-60h]
  void *v22; // [rsp+78h] [rbp-38h]
  __int64 v23; // [rsp+80h] [rbp-30h]
  __int128 v24; // [rsp+88h] [rbp-28h]
  __int128 v25; // [rsp+98h] [rbp-18h]

  if ( (unsigned __int64)&v23 <= *(_QWORD *)(__readfsqword(0xFFFFFFF8) + 16) )
    runtime_morestack_noctxt();
  *(_QWORD *)&v24 = &unk_4B2EA0;
  *((_QWORD *)&v24 + 1) = &off_4EB9E0;
  fmt_Fprintln(
    a1,
    a2,
    (__int64)&go_itab__os_File_io_Writer,
    (__int64)&v24,
    a5,
    a6,
    (__int64)&go_itab__os_File_io_Writer,
    os_Stdout);
  runtime_newobject(a1, a2);
  v21 = (__int64 *)v19;
  v22 = &unk_4B0980;
  v23 = v19;
  fmt_Fscanf(
    a1,
    a2,
    (__int64)&go_itab__os_File_io_Reader,
    (__int64)&v22,
    v6,
    v7,
    (__int64)&go_itab__os_File_io_Reader,
    os_Stdin,
    (__int64)&unk_4CBC7A,
    2LL);
  v25 = 0LL;
  runtime_convTstring(a1, a2, v8);
  *(_QWORD *)&v25 = &unk_4B2EA0; // <---------------- here
  *((_QWORD *)&v25 + 1) = v20; // <------------------ here
  fmt_Sprintf(a1, a2, v9, (__int64)&unk_4B2EA0, v10, v11, (__int64)&unk_4CD206);
  v14 = *v21;
  if ( v21[1] == 1 )
  {
    runtime_memequal(a1, a2, v14, 1);
    result = fmt_Fprintln(
               a1,
               a2,
               v16,
               (__int64)&go_itab__os_File_io_Writer,
               v17,
               v18,
               (__int64)&go_itab__os_File_io_Writer,
               os_Stdout);
  }
  else
  {
    fmt_Fprintln(
      a1,
      a2,
      v14,
      (__int64)&go_itab__os_File_io_Writer,
      v12,
      v13,
      (__int64)&go_itab__os_File_io_Writer,
      os_Stdout);
    result = time_Sleep(a1);
  }
  return result;
}
```
- Tips: trong binary golang (64 bit), 1 struct string trông giống như sau:
```cpp
struct GoString
{
	ULONGLONG* pSize;
	char** ppStr;
}
```
- Với pSize là con trỏ trỏ tới length của string, còn ppStr là con trỏ cấp 2 tới string thật.
- Sau một lúc debug bằng gdb, mình để ý 2 dòng được comment ở phía trên có gì đó hay ho, đó chính là bước tạo string, còn string gì thì mình không biết.
- Vì vậy mình dùng gdb để đặt breakpoint ở đó.
```bash
gef➤  b *0x4A7C98
Breakpoint 1 at 0x4a7c98: file /home/yoshie/go/src/ctf-1/main.go, line 23.
gef➤  r
Starting program: /home/vm/Desktop/CrackMe01 
[New LWP 2490]
[New LWP 2491]
[New LWP 2492]
[New LWP 2493]
[New LWP 2494]
Input flag:
xikhud
... <truncated> ...
     0x4a7c81 <main.main+321>  lea    rcx, [rip+0xb218]        # 0x4b2ea0
     0x4a7c88 <main.main+328>  mov    QWORD PTR [rsp+0x98], rcx
     0x4a7c90 <main.main+336>  mov    QWORD PTR [rsp+0xa0], rax
 →   0x4a7c98 <main.main+344>  lea    rax, [rip+0x25567]        # 0x4cd206
     0x4a7c9f <main.main+351>  mov    QWORD PTR [rsp], rax
     0x4a7ca3 <main.main+355>  mov    QWORD PTR [rsp+0x8], 0xd
     0x4a7cac <main.main+364>  lea    rax, [rsp+0x98]
     0x4a7cb4 <main.main+372>  mov    QWORD PTR [rsp+0x10], rax
     0x4a7cb9 <main.main+377>  mov    QWORD PTR [rsp+0x18], 0x1
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── threads ────
[#0] Id 1, Name: "CrackMe01", stopped 0x4a7c98 in main.createFlag (), reason: BREAKPOINT
[#1] Id 2, Name: "CrackMe01", stopped 0x464add in runtime.usleep (), reason: BREAKPOINT
[#2] Id 3, Name: "CrackMe01", stopped 0x465183 in runtime.futex (), reason: BREAKPOINT
[#3] Id 4, Name: "CrackMe01", stopped 0x465183 in runtime.futex (), reason: BREAKPOINT
[#4] Id 5, Name: "CrackMe01", stopped 0x465183 in runtime.futex (), reason: BREAKPOINT
[#5] Id 6, Name: "CrackMe01", stopped 0x465183 in runtime.futex (), reason: BREAKPOINT
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── trace ────
[#0] 0x4a7c98 → main.createFlag()
[#1] 0x4a7c98 → main.main()
─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Thread 1 "CrackMe01" hit Breakpoint 1, 0x00000000004a7c98 in main.createFlag () at /home/yoshie/go/src/ctf-1/main.go:23
23	/home/yoshie/go/src/ctf-1/main.go: No such file or directory.
gef➤  x/2gx $rsp+0x98
0xc00018cf68:	0x00000000004b2ea0	0x000000c000182200
gef➤  x/1gx 0x4b2ea0
0x4b2ea0:	0x0000000000000010
gef➤  x/1gx 0xc000182200
0xc000182200:	0x00000000004cdabd
gef➤  hexdump 0x00000000004cdabd 0x10
0x00000000004cdabd  73 40 75 72 69 33 6e 67 6b 68 30 6e 47 68 41 74    s@uri3ngkh0nGhAt
```
- Như struct đã mô tả ở trên, ta thấy `$rsp+0x98` chính là địa chỉ của GoString, có `size` = 0x10 và trỏ tới `s@uri3ngkh0nGhAt`.
- Ngoài ra trong đoạn code trên còn có 1 dòng nữa thú vị:
```cpp
fmt_Sprintf(a1, a2, v9, (__int64)&unk_4B2EA0, v10, v11, (__int64)&unk_4CD206);
```
- Double click vào `unk_4CD206`:
```
.rodata:00000000004CD206 unk_4CD206      db  48h ; H             ; DATA XREF: main_main+158↑o
.rodata:00000000004CD207                 db  43h ; C
.rodata:00000000004CD208                 db  4Dh ; M
.rodata:00000000004CD209                 db  55h ; U
.rodata:00000000004CD20A                 db  53h ; S
.rodata:00000000004CD20B                 db  2Dh ; -
.rodata:00000000004CD20C                 db  43h ; C
.rodata:00000000004CD20D                 db  54h ; T
.rodata:00000000004CD20E                 db  46h ; F
.rodata:00000000004CD20F                 db  7Bh ; {
.rodata:00000000004CD210                 db  25h ; %
.rodata:00000000004CD211                 db  73h ; s
.rodata:00000000004CD212                 db  7Dh ; }
```
- Ta thấy `HCMUS-CTF{%s}`.
- Vậy ta đoán ngay flag: `HCMUS-CTF{s@uri3ngkh0nGhAt}`.

## CrackMe02
- Giống bài CrackMe01, bỏ vào IDA, nhưng hàm `main_main` không có gì thú vị.
- Tuy nhiên hàm `main_QLEXTMbGJ8x5yHgv` lại construct rất nhiều string.
```cpp
__int64 __fastcall main_QLEXTMbGJ8x5yHgv(__int64 a1, __int64 a2, __int64 a3, __int64 a4, __int64 a5, __int64 a6)
{
  __int64 v6; // rcx
  __int64 v7; // rdx
  __int64 v8; // r8
  __int64 v9; // r9
  __int128 v11; // [rsp+0h] [rbp-A0h]
  __int64 v12; // [rsp+28h] [rbp-78h]
  __int128 v13; // [rsp+58h] [rbp-48h]
  __int128 v14; // [rsp+68h] [rbp-38h]
  __int128 v15; // [rsp+78h] [rbp-28h]
  __int128 v16; // [rsp+88h] [rbp-18h]

  v6 = __readfsqword(0xFFFFFFF8);
  if ( (unsigned __int64)&v15 + 8 <= *(_QWORD *)(v6 + 16) )
    runtime_morestack_noctxt(a1, a2, a3, v6, a5, a6);
  *(_QWORD *)&v13 = &unk_4CD10E;
  *((_QWORD *)&v13 + 1) = 5LL;
  *(_QWORD *)&v14 = &unk_4CCE58;
  *((_QWORD *)&v14 + 1) = 1LL;
  *(_QWORD *)&v15 = &unk_4CCF27;
  *((_QWORD *)&v15 + 1) = 3LL;
  *(_QWORD *)&v16 = &unk_4CD074;
  *((_QWORD *)&v16 + 1) = 4LL;
  *(_QWORD *)&v11 = &v13;
  *((_QWORD *)&v11 + 1) = 4LL;
  strings_Join(a1, a2, a3, v6, a5, v11);
  runtime_convTstring(a1, a2, (__int64)&unk_4CEA38);
  fmt_Sprintf(a1, a2, v7, (__int64)&unk_4B3F00, v8, v9);
  return v12;
}
```
- Cuối cùng ta thấy sau khi construct 1 đống này, chương trình gọi `fmt_Sprintf`, nên ta có thể đặt breakpoint sau hàm này và quan sát thanh ghi.
```bash
gef➤  b *0x4A87AF
Breakpoint 1 at 0x4a87af: file /home/yoshie/go/src/ctf/ctf-2/main.go, line 32.
gef➤  r
Starting program: /home/vm/Desktop/CrackMe02 
[New LWP 2641]
[New LWP 2642]
[New LWP 2643]
[New LWP 2644]
Input flag:
xikhud
     0x4a8798 <main.QLEXTMbGJ8x5yHgv+312> mov    QWORD PTR [rsp+0x18], 0x1
     0x4a87a1 <main.QLEXTMbGJ8x5yHgv+321> mov    QWORD PTR [rsp+0x20], 0x1
     0x4a87aa <main.QLEXTMbGJ8x5yHgv+330> call   0x499880 <fmt.Sprintf>
 →   0x4a87af <main.QLEXTMbGJ8x5yHgv+335> mov    rax, QWORD PTR [rsp+0x28]
     0x4a87b4 <main.QLEXTMbGJ8x5yHgv+340> mov    rcx, QWORD PTR [rsp+0x30]
     0x4a87b9 <main.QLEXTMbGJ8x5yHgv+345> mov    QWORD PTR [rsp+0xa8], rax
     0x4a87c1 <main.QLEXTMbGJ8x5yHgv+353> mov    QWORD PTR [rsp+0xb0], rcx
     0x4a87c9 <main.QLEXTMbGJ8x5yHgv+361> mov    rbp, QWORD PTR [rsp+0x98]
     0x4a87d1 <main.QLEXTMbGJ8x5yHgv+369> add    rsp, 0xa0
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── threads ────
[#0] Id 1, Name: "CrackMe02", stopped 0x4a87af in main.QLEXTMbGJ8x5yHgv (), reason: BREAKPOINT
[#1] Id 2, Name: "CrackMe02", stopped 0x464add in runtime.usleep (), reason: BREAKPOINT
[#2] Id 3, Name: "CrackMe02", stopped 0x465183 in runtime.futex (), reason: BREAKPOINT
[#3] Id 4, Name: "CrackMe02", stopped 0x465183 in runtime.futex (), reason: BREAKPOINT
[#4] Id 5, Name: "CrackMe02", stopped 0x465183 in runtime.futex (), reason: BREAKPOINT
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── trace ────
[#0] 0x4a87af → main.QLEXTMbGJ8x5yHgv(~r0=<optimized out>)
[#1] 0x4a853d → main.hQpJXFTszJqfDEjc()
[#2] 0x4a8365 → main.init.0()
[#3] 0x44190a → runtime.doInit(t=0x559be0 <main..inittask>)
[#4] 0x434985 → runtime.main()
[#5] 0x463341 → runtime.goexit()
─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Thread 1 "CrackMe02" hit Breakpoint 1, 0x00000000004a87af in main.QLEXTMbGJ8x5yHgv (~r0=...) at /home/yoshie/go/src/ctf/ctf-2/main.go:32
32	/home/yoshie/go/src/ctf/ctf-2/main.go: No such file or directory.
gef➤  hexdump $rdi
0x000000c0000143a0     48 43 4d 55 53 2d 43 54 46 7b 37 73 32 74 70 5e    HCMUS-CTF{7s2tp^
0x000000c0000143b0     6e 59 61 68 4e 25 68 46 76 3d 7d 00 00 00 00 00    nYahN%hFv=}.....
0x000000c0000143c0     00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
0x000000c0000143d0     00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
gef➤  
```
- Ta thấy luôn flag: `HCMUS-CTF{7s2tp^nYahN%hFv=}`

## PressMe
- Đề cho chương trình trông giống 1 game viết bằng Unity.
- Mình đã RE nhiều loại game kiểu này nên biết ngay logic của nó nằm ở `Assembly-CSharp.dll`.
- Mở ngay dll trên bằng `dnSpy-x86`, ta thấy trong class `CTF` có 1 array trông như này:
```CSharp
private byte[] xyq = new byte[]
	{
		72,
		67,
		77,
		85,
		83,
		32,
		45,
		32,
		67,
		84,
		70,
		123,
		32,
		116,
		82,
		117,
		110,
		71,
		95,
		84,
		104,
		85,
		95,
		99,
		111,
		95,
		67,
		72,
		105,
		95,
		72,
		97,
		110,
		103,
		125
	};
```
- Script:
```python
a = [72,67,77,85,83,32,45,32,67,84,70,123,32,116,82,117,110,71,95,84,104,85,95,99,111,95,67,72,105,95,72,97,110,103,125]
for i in a:
	print (chr(i), end='')
print ()
```
```
HCMUS - CTF{ tRunG_ThU_co_CHi_Hang}
```
- Bỏ dấu cách trong output, ta có flag.

## CrackMe03
- Như 2 bài CrackMe0x trên, ta lại dùng IDA, và decompile hàm sau:
```cpp
__int64 __fastcall main_RARzjZmrgEFKzrwx(char *a1, void *a2, __int64 a3, __int64 a4, __int64 a5, __int64 a6)
{
  unsigned __int64 v6; // rcx
  __int64 v7; // r8
  __int64 v8; // r9
  __int64 v9; // rdx
  __int64 v10; // r8
  __int64 v11; // r9
  __int64 v12; // r8
  __int64 v13; // r9
  __int64 v14; // r8
  __int64 v15; // r9
  __int64 v16; // rdx
  __int64 v17; // r8
  __int64 v18; // r9
  __int128 *v19; // rax
  __int64 v20; // rcx
  __int64 i; // rdx
  __int64 v22; // rdx
  __int64 result; // rax
  __int64 *v24; // [rsp+18h] [rbp-1D0h]
  __int64 v25; // [rsp+20h] [rbp-1C8h]
  __int64 v26; // [rsp+28h] [rbp-1C0h]
  __int64 v27; // [rsp+60h] [rbp-188h]
  char v28; // [rsp+68h] [rbp-180h]
  char v29; // [rsp+90h] [rbp-158h]
  __int64 *v30; // [rsp+188h] [rbp-60h]
  __int128 v31; // [rsp+190h] [rbp-58h]
  __int128 v32; // [rsp+1A0h] [rbp-48h]
  __int128 v33; // [rsp+1B0h] [rbp-38h]
  __int128 v34; // [rsp+1C0h] [rbp-28h]
  __int128 v35; // [rsp+1D0h] [rbp-18h]

  v6 = __readfsqword(0xFFFFFFF8);
  if ( (unsigned __int64)&v29 <= *(_QWORD *)(v6 + 16) )
    runtime_morestack_noctxt(a1, a2, a3, v6, a5, a6);
  *(_QWORD *)&v35 = &unk_4B1EA0;
  *((_QWORD *)&v35 + 1) = &off_4EA740;
  fmt_Fprintln(
    (__int64)a1,
    (__int64)a2,
    (__int64)&go_itab__os_File_io_Writer,
    (__int64)&v35,
    a5,
    a6,
    (__int64)&go_itab__os_File_io_Writer,
    os_Stdout);
  runtime_newobject((__int64)a1, (__int64)a2);
  v30 = v24;
  *(_QWORD *)&v34 = &unk_4AF980;
  *((_QWORD *)&v34 + 1) = v24;
  fmt_Fscanf(
    (__int64)a1,
    (__int64)a2,
    (__int64)&go_itab__os_File_io_Reader,
    (__int64)&v34,
    v7,
    v8,
    (__int64)&go_itab__os_File_io_Reader,
    os_Stdin,
    (__int64)&unk_4CAC7A,
    2LL);
  main_QswxHczcsfgvgPAU((__int64)a1, (__int64)a2, v9, *v30, v10, v11, *v30, v30[1]);
  main_wVApZHQYWnVReEMC((__int64)a1, (__int64)a2, (__int64)&v34, v26, v12, v13, v25, v26, (__int64)&v34);
  main_ZPmQQLBPKLEagZpz((__int64)a1, (__int64)a2, 1LL, (__int64)&v34, v14, v15);
  v19 = &v34;
  v20 = v26;
  if ( &v34 == (__int128 *)37 )
  {
    for ( i = 0LL; (__int64)v19 > i; i = v22 + 1 )
    {
      v27 = 373248LL;
      a1 = &v28;
      a2 = &unk_4ED0C8;
      v19 = (__int128 *)sub_463D24(&v28, &unk_4ED0C8, i, v20);
      v17 = *(&v27 + v22);
      if ( *(_QWORD *)(v20 + 8 * v22) != v17 )
      {
        *(_QWORD *)&v32 = &unk_4B1EA0;
        *((_QWORD *)&v32 + 1) = &off_4EA760;
        fmt_Fprintln(
          (__int64)&v28,
          (__int64)&unk_4ED0C8,
          v22,
          (__int64)&go_itab__os_File_io_Writer,
          v17,
          v18,
          (__int64)&go_itab__os_File_io_Writer,
          os_Stdout);
        return time_Sleep((__int64)&v28);
      }
    }
    *(_QWORD *)&v31 = &unk_4B1EA0;
    *((_QWORD *)&v31 + 1) = &off_4EA770;
    result = fmt_Fprintln(
               (__int64)a1,
               (__int64)a2,
               i,
               (__int64)&go_itab__os_File_io_Writer,
               v17,
               v18,
               (__int64)&go_itab__os_File_io_Writer,
               os_Stdout);
  }
  else
  {
    *(_QWORD *)&v33 = &unk_4B1EA0;
    *((_QWORD *)&v33 + 1) = &off_4EA750;
    fmt_Fprintln(
      (__int64)a1,
      (__int64)a2,
      v16,
      (__int64)&go_itab__os_File_io_Writer,
      v17,
      v18,
      (__int64)&go_itab__os_File_io_Writer,
      os_Stdout);
    result = time_Sleep((__int64)a1);
  }
  return result;
}
```
- Khi bài này được release, mình không còn nhiều thời gian, nên không đọc code, thay vào đó, mình sẽ nhập input, quan sát input thay đổi thế nào qua từng lệnh, từ đó đoán được flow của chương trình.
- Đầu tiên ta nhìn thấy hàm trên gọi `fmt_Fscanf`, ta có thể đoán ngay đây là hàm nhận input từ user, vậy ta chỉ cần đặt breakpoint từ sau chỗ này, cụ thể là đặt tại 3 chỗ sau:
	- `main_QswxHczcsfgvgPAU`
	- `main_wVApZHQYWnVReEMC`
	- `main_ZPmQQLBPKLEagZpz`
- Lưu ý: trên x64 linux, tham số của các hàm nằm theo thứ tự sau (từ trái qua phải): `rdi`, `rsi`, `rdx`, `rcx`, `r8`, `r9`, vì vậy ta chỉ cần coi các thanh ghi này là OK.
```bash
gef➤  b *0x4A62E0
Breakpoint 1 at 0x4a62e0: file /home/yoshie/go/src/ctf/ctf-21/main.go, line 20.
gef➤  b *0x4A6302
Breakpoint 2 at 0x4a6302: file /home/yoshie/go/src/ctf/ctf-21/main.go, line 20.
gef➤  b *0x4A6324
Breakpoint 3 at 0x4a6324: file /home/yoshie/go/src/ctf/ctf-21/main.go, line 20.
gef➤  r
Starting program: /home/vm/Desktop/CrackMe03 
[New LWP 2737]
[New LWP 2738]
[New LWP 2739]
[New LWP 2740]
[New LWP 2741]
Input flag:
xikhud
...
```
- Ta quan sát:
	- Trước khi chạy hàm `main_QswxHczcsfgvgPAU`:
		- `rcx`: trỏ tới string.
		- `rax`: = 0x6 = strlen(rcx)
	-	Sau khi chạy hàm `main_QswxHczcsfgvgPAU`:
```bash
gef➤  hexdump $rdi
0x000000c00013e000     78 00 00 00 00 00 00 00 69 00 00 00 00 00 00 00    x.......i.......
0x000000c00013e010     6b 00 00 00 00 00 00 00 68 00 00 00 00 00 00 00    k.......h.......
0x000000c00013e020     75 00 00 00 00 00 00 00 64 00 00 00 00 00 00 00    u.......d.......
```
- Ta nhận ra ngay: hàm trên đổi từ dãy `char*` sang dãy `ULONGLONG`.
- Tiếp theo:
	-  Sau khi chạy hàm `main_wVApZHQYWnVReEMC`:
```bash
gef➤  hexdump $rdi
0x000000c00013e000     78 00 00 00 00 00 00 00 69 00 00 00 00 00 00 00    x.......i.......
0x000000c00013e010     6d 00 00 00 00 00 00 00 38 01 00 00 00 00 00 00    m.......8.......
0x000000c00013e020     79 00 00 00 00 00 00 00 f4 01 00 00 00 00 00 00    y...............
```
- Ta thấy một số chỗ bị thay đổi.
- Tiếp theo:
	- Sau khi chạy hàm `main_ZPmQQLBPKLEagZpz`:
```bash
gef➤  hexdump 0x000000c00013e000
0x000000c00013e000     00 5e 1a 00 00 00 00 00 f9 a9 11 00 00 00 00 00    .^..............
0x000000c00013e010     69 2e 00 00 00 00 00 00 00 6e cf 01 00 00 00 00    i........n......
0x000000c00013e020     31 39 00 00 00 00 00 00 90 d0 03 00 00 00 00 00    19..............
```
- Ta lại thấy dãy ULONGLONG bị thay đổi.
```cpp
if ( &v34 == (__int128 *)37 )
  {
    for ( i = 0LL; (__int64)v19 > i; i = v22 + 1 )
    {
      v27 = 373248LL;
      a1 = &v28;
      a2 = &unk_4ED0C8;
      v19 = (__int128 *)sub_463D24(&v28, &unk_4ED0C8, i, v20);
      v17 = *(&v27 + v22);
      if ( *(_QWORD *)(v20 + 8 * v22) != v17 )
      {
        *(_QWORD *)&v32 = &unk_4B1EA0;
        *((_QWORD *)&v32 + 1) = &off_4EA760;
        fmt_Fprintln(
          (__int64)&v28,
          (__int64)&unk_4ED0C8,
          v22,
          (__int64)&go_itab__os_File_io_Writer,
          v17,
          v18,
          (__int64)&go_itab__os_File_io_Writer,
          os_Stdout);
        return time_Sleep((__int64)&v28);
      }
    }
```
- Đoạn code trên so sánh xem, dãy ULONGLONG trên có bao gồm 37 phần tử không, nếu có thì tiếp tục so sánh 37 phần tử đó với 1 array ở `0x4ED0C8`.
- Biết được điều đó, ta chỉ cần RE 2 hàm biến đổi ở trên là được.
```cpp
__int64 __fastcall main_wVApZHQYWnVReEMC(__int64 a1, __int64 a2, __int64 a3, __int64 a4, __int64 a5, __int64 a6, __int64 a7, __int64 a8, __int64 a9)
{
  __int64 i; // rdx

  for ( i = 0LL; i < a8; ++i )
  {
    if ( _bittest((const int *)&i, 0) )
      *(_QWORD *)(a7 + 8 * i) *= i;
    else
      *(_QWORD *)(a7 + 8 * i) += i;
  }
  return a9;
}
```
```cpp
__int64 __fastcall main_ZPmQQLBPKLEagZpz(__int64 a1, __int64 a2, __int64 a3, __int64 a4, __int64 a5, __int64 a6, __int64 a7, __int64 a8, __int64 a9)
{
  __int64 i; // rdx
  __int64 v10; // rbx

  for ( i = 0LL; i < a8; ++i )
  {
    v10 = *(_QWORD *)(a7 + 8 * i);
    if ( 0xAAAAAAAAAAAAAAABLL * v10 + 0x2AAAAAAAAAAAAAAALL > 0x5555555555555554LL )
      *(_QWORD *)(a7 + 8 * i) = v10 * v10;
    else
      *(_QWORD *)(a7 + 8 * i) *= v10 * v10;
  }
  return a9;
}
```
- 2 hàm trên khá dễ, 1 hàm nhân (hoặc cộng) các số trong array với `i` và một hàm bình phương (hoặc lập phương) các số trong array.
- Ta chỉ cần viết 1 hàm bruteforce từng ký tự ở từng vị trí là được.

## CarveMe
- Mở file bằng `jadx`, ta đọc tất cả các hàm của program, thấy được flag có 3 phần, phần 1 và phần 3 nằm ngay trong program (phần 1 được mã hoá bằng base64)
- Phần 2 yêu cầu ta phải có password, password gồm 8 ký tự, mỗi ký tự thuộc `[0-9]`, và 2 ký tự đầu là `75`.
- Ta chỉ cần viết 1 đoạn code bruteforce 1 triệu trường hợp để ra được part 2 của flag.

## SimpleBOF
- Ghi đè return address thành địa chỉ của hàm `success`.

## Hackme
```cpp
int check_password()
{
  char s; // [esp+8h] [ebp-20h]
  int v2; // [esp+1Ch] [ebp-Ch]

  v2 = 0xAABBCCDD;
  puts("[+] Password: ");
  gets(&s);
  if ( !strcmp(&s, "LoveM-TP") )
    puts("******** Access Granted ********");
  if ( v2 == 0xFEEF1608 )
  {
    puts("You got the secret");
    system("/bin/cat flag.txt");
    exit(0);
  }
  return 0;
}
```
- Trong hàm trên, dùng `get(&s)` để ghi đè v2 và lấy flag.

## Findme
```bash
.text:000000000040089C                 lea     rdi, command    ; "/bin/cat flag.txt"
.text:00000000004008A3                 call    _system
```
```cpp
int __cdecl main(int argc, const char **argv, const char **envp)
{
  char v4; // [rsp+0h] [rbp-80h]

  clean(argc, argv, envp);
  puts("Wellcome to HCMUS-CTF 2020! Please tell me your token:");
  gets((__int64)&v4);
  puts("So, that's your token! You're just a normal user :), Bye!!!");
  return 0;
}
```
- Trong hàm main, dùng `gets` để ghi đè return address thành `0x40089C` và lấy flag (chú ý calling convention alignment).

## Argument
- Command injection.
	- `ls .;sh` --> lấy shell + flag.

## Take my shell
- Bài này cho phép execute shellcode nhập từ bàn phím.
- Lên google tìm 1 shellcode "execute /bin/sh", nhập vào và lấy flag.

## CL
- Giống simpleBOF, nhưng ta chỉ cần ghi thêm 2 argument vào stack.

## NoobEncoding
- Dựa vào input để decode đoạn message nhiều lần -> ra được flag.

## Handshake
- Nhìn vào file pcap, ở những gói tin gần cuối cùng có 1 đoạn server và client nói chuyện với nhau.
- Tóm lại:
	- Server cho 1 con số nonce, và template của message.
	- Ta phải tìm 1 message thoả `sha256(message).startswith(b'\x00\x00') == True`.
	- Bài này ta thêm vào sau template 2 byte, rồi bruteforce 2 byte đó (256*256) để tìm hash thoả mãn -> server trả về flag.
