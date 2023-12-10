[1]: https://rosettacode.org/wiki/ASCII_control_characters

# [ASCII control characters][1]

There doesn't seem to really be a point or a purpose to this task other than creating a enumeration...



'space' is absolutely **NOT** a control character. Pretending it is is just completely incorrect.

```perl
enum C0 (|(^32).map({ (0x2400 + $_).chr => $_ }), '␡' => 127);

printf "Ord: %3d, Unicode: %s, Enum: %s\n", $_, .uniname, C0($_)
   for (^128).grep: {.chr ~~ /<:Cc>/}
```

#### Output:
```
Ord:   0, Unicode: <control-0000>, Enum: ␀
Ord:   1, Unicode: <control-0001>, Enum: ␁
Ord:   2, Unicode: <control-0002>, Enum: ␂
Ord:   3, Unicode: <control-0003>, Enum: ␃
Ord:   4, Unicode: <control-0004>, Enum: ␄
Ord:   5, Unicode: <control-0005>, Enum: ␅
Ord:   6, Unicode: <control-0006>, Enum: ␆
Ord:   7, Unicode: <control-0007>, Enum: ␇
Ord:   8, Unicode: <control-0008>, Enum: ␈
Ord:   9, Unicode: <control-0009>, Enum: ␉
Ord:  10, Unicode: <control-000A>, Enum: ␊
Ord:  11, Unicode: <control-000B>, Enum: ␋
Ord:  12, Unicode: <control-000C>, Enum: ␌
Ord:  13, Unicode: <control-000D>, Enum: ␍
Ord:  14, Unicode: <control-000E>, Enum: ␎
Ord:  15, Unicode: <control-000F>, Enum: ␏
Ord:  16, Unicode: <control-0010>, Enum: ␐
Ord:  17, Unicode: <control-0011>, Enum: ␑
Ord:  18, Unicode: <control-0012>, Enum: ␒
Ord:  19, Unicode: <control-0013>, Enum: ␓
Ord:  20, Unicode: <control-0014>, Enum: ␔
Ord:  21, Unicode: <control-0015>, Enum: ␕
Ord:  22, Unicode: <control-0016>, Enum: ␖
Ord:  23, Unicode: <control-0017>, Enum: ␗
Ord:  24, Unicode: <control-0018>, Enum: ␘
Ord:  25, Unicode: <control-0019>, Enum: ␙
Ord:  26, Unicode: <control-001A>, Enum: ␚
Ord:  27, Unicode: <control-001B>, Enum: ␛
Ord:  28, Unicode: <control-001C>, Enum: ␜
Ord:  29, Unicode: <control-001D>, Enum: ␝
Ord:  30, Unicode: <control-001E>, Enum: ␞
Ord:  31, Unicode: <control-001F>, Enum: ␟
Ord: 127, Unicode: <control-007F>, Enum: ␡
```
