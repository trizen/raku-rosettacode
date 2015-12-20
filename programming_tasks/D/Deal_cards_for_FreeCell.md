[1]: http://rosettacode.org/wiki/Deal_cards_for_FreeCell

# [Deal cards for FreeCell][1]

```perl6
sub dealgame ($game-number = 1) {
    sub ms-lcg-method($seed = $game-number) { ( 214013 * $seed + 2531011 ) % 2**31 }
 
    # lazy list of the random sequence
    my @ms-lcg := (&ms-lcg-method ... *).map: * +> 16;
 
    constant CardBlock = '🂠'.ord;
    my @deck = gather for flat(1..11,13,14) X+ (48,32...0) -> $off {
        take chr CardBlock + $off;
    }
 
    my @game = gather while @deck {
        @deck[@ms-lcg.shift % @deck, @deck-1] .= reverse;
        take @deck.pop;
    }
 
    say "Game #$game-number";
    say @game.splice(0, 8 min +@game) while @game;
}
 
dealgame;
dealgame 617;
```

#### Output:
```
Game #1
🃋 🃂 🂹 🃛 🃅 🂷 🃗 🂵
🃎 🃞 🂩 🂥 🃁 🃝 🂾 🂳
🂢 🂮 🃉 🃍 🂫 🂡 🂱 🃓
🃔 🃕 🂪 🂽 🂴 🃑 🃄 🂧
🂣 🃊 🂤 🂺 🂸 🃒 🂻 🃇
🃆 🂨 🃈 🂭 🃖 🃃 🃘 🃚
🂦 🃙 🂲 🂶
Game #617
🃇 🃁 🃕 🂣 🂥 🃘 🃂 🂱
🃊 🂧 🃍 🃑 🃆 🂸 🂡 🂾
🂺 🃝 🂳 🃉 🂦 🃈 🃃 🃚
🃎 🂵 🂩 🃓 🂨 🂷 🃄 🂫
🃔 🂭 🃙 🂹 🃗 🂶 🃒 🂢
🂤 🂪 🂲 🃅 🃛 🃖 🂻 🂽
🃋 🂮 🃞 🂴
```