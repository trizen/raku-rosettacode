[1]: https://rosettacode.org/wiki/Sync_subtitles

# [Sync subtitles][1]

### Brutal

```perl
# 20240621 Raku programming solution

sub MAIN() {
   my @lines = $*IN.lines;
   for 9, -9 -> $seconds {
      say "Original subtitle adjusted by {$seconds.fmt('%+d')} seconds.";
      for @lines -> $line {
         if $line ~~ /(\d ** 2 ':' \d ** 2 ':' \d ** 2 ',' \d ** 3) ' --> ' (\d ** 2 ':' \d ** 2 ':' \d ** 2 ',' \d ** 3)/ {
            my $start = adjust-time($0.Str, $seconds);
            my $end   = adjust-time($1.Str, $seconds);
            my $adjusted = $line;
            $adjusted ~~ s/\d ** 2 ':' \d ** 2 ':' \d ** 2 ',' \d ** 3 ' --> ' \d ** 2 ':' \d ** 2 ':' \d ** 2 ',' \d ** 3/$start ~ ' --> ' ~ $end/.Str;
            say $adjusted
         } else {
            say $line;
         }
      }
      say()
   }
}

sub adjust-time($time, $seconds) {
   my ($time_str, $milliseconds_str) = $time.split(',');
   my (\hh, \mm, \ss) = $time_str.split(':')>>.Int;
   my $milliseconds = $milliseconds_str.Int;
   my $datetime = DateTime.new(:year,     :month,      :day, 
                               :hour(hh), :minute(mm), :second(ss));
   given $datetime .= later(:seconds($seconds)) {
      return sprintf('%02d:%02d:%02d,%03d',.hour,.minute,.second,$milliseconds)
   }
}
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=lVTdbuNEFJYQV-cpDquu7JSJ49_EdtUuCKjIBQtaKnFTCbnxJDa1Z1aecUq0aiWeg5tewEvtI_AUnIlj1ykroc6FfXzm-87PN8fz519Ndts-Pv7d6vU0_vj5H6q9wR--Xr61J_gBELHe4VdVKbjCczw5Xb519h9nZmstG0wYThOcXuCJ4ispctWxaKlsh69-bMpNKbIKKawudcUxy39rleY53uzwQ09y1rW2rddf5tbkHnvfq7NDJJPnUINJZKwhC61yffA9PODMvs7x9BR9tFILP2mz3g4maOGUIlr4ItZsnLxT6ETprNGkUNfdVJc1t09c52fdsEGZydl_aFzkZD2jef9LGzQ87zo_RjxtkyBq9oLWBj1ewJkden8YyA_7vmami-PCzEQMxT3t3COvFH8m6h77rLd7OH4Txp7A3nEPYAb3SEfzHMk4jHO386vai1yXVVUeIMY1MZqafUe9r0oaSmZ1B2CI10XB8Lqu6aHUgDS0Hp1ak4sLZyl0zzlKYBjPEx6B80xzE5KA35J5ZeoQ_M5Odzyjas1Kayl00dmY5tmO4ZFwn1hpIdvGLooJI3YpWs3tujYfXRk29dL1uCm3XIyqcM6xIruxD0hlD2pOhvNquG4bgep9Uwq9pr_Y9fN0eLDXbpBbzDElMKfLzpwuCjsSoz_J7iY6XEiPHz_7xwPXTV0vDTwWRe5-yA6OOQvCBVxSaLwxkaRAI5TCbCMZ6ILjumyUxjprzP1QlWtuLpNaOYDgwxAliqJR2NBnrpfA5Z66ykiHLpIqHMchk3qt5F21A0kfzSgq8q2stjx3AAIYYnlhOA4esGSRwBUxOZFoQE31mVhxpWWjKANACGPsmByxOImBQPuy1o2su7oAop4zZ37gjzkLNndj-G7Lm50UlG8liZrRxaO46awkMWAOA3gRBCN25LJg7sE3dMjZistW4dWuyYSQKmub1lAXMCAXwXxM9VkYx3DJG7kqifkFQNxjY-YGY8nJEc8TeMdzBpD0qIT57lMxfuq6zCXtfpJiQ8E8Fw5un0WhO8aRbkkCSyUsve8QM9zw1a18QyyvZ4Us9OZj1oIcCXwv77r7aJUJXBJPE9uLUNNwXU3f8d-xFGhOzwTzYeBGR6V6HnPDGH4pddEdcL41Z5yj5qtCyEpu6L9doipIzFssNebyTjjdzP8L)



### Less brutal

```perl
# 20240622 Raku programming solution

grammar SRT { # loc.gov/preservation/digital/formats/fdd/fdd000569.shtml
   token TOP { ^ <subtitle>+ % \n $ }
   token subtitle { <index> \n <timecode> \n <text> \n? }
   token index { \d+ }
   token timecode { <timestamp> ' --> ' <timestamp> }
   token timestamp { \d ** 2 ':' \d ** 2 ':' \d ** 2 ',' \d ** 3 }
   token text { <line>+ % \n }
   token line { <-[\n]>+ }
}

class SRT::Actions { has @.subtitles;

   method TOP($/) {
      @.subtitles = $<subtitle>».made;
      make @.subtitles
   }

   method subtitle($/) {
      make {
         index => $<index>.Str,
         start => $<timecode><timestamp>[0].made,
         end   => $<timecode><timestamp>[1].made,
         text  => $<text>.made,
      }
   }

   method timestamp($/) { make $/.Str }

   method text($/) { make $<line>.join("\n") }
}

class SubtitleAdjuster {

    method adjust-time($time, $seconds) {
       my ($time-str, $milliseconds-str) = $time.split(',');
       my (\hh, \mm, \ss) = $time-str.split(':')>>.Int;
       my \mls            = $milliseconds-str.Int;
       my $datetime = DateTime.new( :year, :month, :day,
                                    :hour(hh), :minute(mm), :second(ss));
       given $datetime .= later(:seconds($seconds)) {
          return sprintf('%02d:%02d:%02d,%03d', .hour, .minute, .second, mls)
       }
   }

   method adjust-subtitles(@subtitles, Int $seconds) {
      @subtitles.map({
         $_<start> = self.adjust-time($_<start>, $seconds);
         $_<end>   = self.adjust-time($_<end>, $seconds);
         $_;
      });
   }

   method format-srt(@subtitles) {
      @subtitles.map({
         $_<index> ~ "\n"
         ~ $_<start> ~ " --> " ~ $_<end> ~ "\n"
         ~ $_<text> ~ "\n"
      }).join("\n");
   }
}

my $srt-content = $*IN.slurp;
my $parsed = SRT.parse($srt-content, :actions(SRT::Actions.new));
my @subtitles = $parsed.made;

my $adjuster = SubtitleAdjuster.new;

for 9, -9 -> \N {
   my @adjusted-subtitles = $adjuster.adjust-subtitles(@subtitles.deepmap(*.clone), N);
   say "Original subtitle adjusted by {N.fmt('%+d')} seconds.";
   say $adjuster.format-srt(@adjusted-subtitles);
}
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=jVbdbts2FAZ2NfApzjIHllNakfwvJ3FTbCuWXaRFG2AXdTewFm2rkUiDpJ0ahfsiu-nF9hS722P0EfYUO9Sf6SYtJsAydc75Pp4_HumPPxW7XX_8-NfazNujT9_-slAsy5iCly9u4D18D6mc-Qu5OV0prrnaMJNIcRoni8Sw9HQuVcaMPp3Hsf0FQdAfRL5emiwlAGDkLRdw8-w5Mv0G53r9xiQm5ZNHcAxTAQ3Y7a0qJZqeJyLm7ybW5NwkGZ_JmJdP_J2xq8cuMrdG2DR-5IorpCW0a21YtppAE9pte3dln6FyaU4IJyfQgea4-fCaVuvuAQU6aTdNE1GH6qit2Krbr6bi9cS6vCNkljKtbc7H4yczm2KNJkum4dKvEqPPiCXJuFnK2CbVa5y24L2V4eXYwQU09rn-528_YzE_K-0ydstdYyveucSV5oA9R1UPeBUpv5jgRkWt_JdG0b0eM6hMoa8L6CT8VfA6d8pBcBHj_cuI8B4iT3OJsG1xoN_di6vmKgIrQmqcWsc_M0SyA5uikP5bmQjvaCqOWgclK9P1JH671oYrzFLuQknGcnHbbu417J1CQ2N0Itb77EK2hULb1phGaGRJmialmRW1bEmt3terNDEeNl7rzAVPl0sK0yzDm9a1tYVWiHGzNZn4V8K4uGmWanCui_tbfw5pxMxwS47GP-Lyxnol-J0H4y1n6Pw4k8KgN-OYbZ1qfeUaL-Vaectly4ITsTbcyzL7ULjhYUT7aBfJBs_Q3gv_AlJcK6-01l6d35bbsaC4WSscMyuVCDP3msdBJx7XN3ocdOMmBd_6gn-FH7goyChgploV2_3mKstcnyrvsl5SwAw-UPS9BTbuynNcbfx-np-fCaZY83TuHzRRpXQa6ewAiydpktfyIaxVfglZPewK8UGAxaBva2Wc0P5nMOU0_wD28Ow1H5xAUZeP5aNCmofwoH3xAjhQ7VrO0Sw9R99tr6K7bQzUcKwA9vbJ1bWv07VaneXaFVOax6jAsevnD56LwAZkxSj23Llsm932IzJcHozcgq4ctvkGrJoJF_fGhGVBK0wrRBTaEWDw0-sin5a5hMbtgy0qQv8r_ebHnK9sEU78WSoFx4N0XaRFsy0cPVP46hYs3b9vq63gzRbeX_vzDKfF8aO42dpB2Sb-UY3fu-B2xH1vccdd8UlRfll8_PTNvyEJgnEQjrsh7feDvOClYEC7vSF5imcP3tgBJAXYaaKBLSQlZslhnihtcCAr-_ZMkznPW1L7BEiH1Cz9ft-h7XVoEEbkaQ6dMZwWBZNe-r6PS5wGqbxLt0Tig3JYgW9kusFiEtIlNVfY67nkXRoNI3KDSI4gnBXWeyZm-JaRSuMOhPSIa-uC-3QUjQga5W7NlcwKvwjpV5gB7XQ7LmZIB8GI_LThaot1pTCTCGX43tTcRpZgMsiA1MbDbtdB9wPaHYTkBxyDbMblWsPNVjEhpGZrtbbQIakth92BC-3Q3mhEnnIlZwkivyNkVNmOaNB1U46C0SAiL3hMCYkqq4h2gr0znXEQ0ABz91yKBZKFASnFHdrvBa4d5i2KyJUWTZNHCAwWfHYrHyMqrFA92gsHLmqIgoj8LO-Ap5pj5QVcIc4gOuzjVxh-krZf4PdLIsBWz5J1SI3tH7gahjTojciviVkWBY43tsb2M2G2FDKViy1OeNBLTOYtJAZieSf8ouf_Aw))
