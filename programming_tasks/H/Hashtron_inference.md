[1]: https://rosettacode.org/wiki/Hashtron_inference

# [Hashtron inference][1]

Translation of [#Phix](#Phix) and [Go](https://go.dev/play/p/AsmOzKWx7jB)

```perl
# 20240530 Raku programming solution

sub Inference($command, $bits, @program) {
   my  $out = 0;

   return $out unless @program.Bool;

   for ^$bits -> $j { # Iterate over the bits
      my $input = $command +| ($j +< 16);
      $input = Hashtron($input, @program[0][0], my $maxx = @program[0][1]);
      for @program[1..*] -> ($s, $max) {
         $input = Hashtron($input, $s, $maxx -= $max);
      }
      if ( $input +&= 1 ) != 0 { $out +|= 1 +< $j }
   }
   return $out;
}

sub Hashtron($n, $s, $max) {
   # Mixing stage, mix input with salt using subtraction
   my $m = $n - $s;

   # Hashing stage, use xor shift with prime coefficients
   for <-2 -3 +5 +7 -11 -13 +17 -19> -> $p {
      $m = ($m +^ ($m +> $p)) +& 0xFFFFFFFF; 
   }

   # Mixing stage 2, mix input with salt using addition
   $m = ($m + $s) +& 0xFFFFFFFF;

   # Modular stage using Lemire's fast alternative to modulo reduction
   return (($m * $max) +> 32) +& 0xFFFFFFFF;
}

sub MAIN() {
   say Inference(42, 64, (<0 2>,));

   for 0..^256 -> $i {
      say "$i ", Inference($i, 4, (
         <8776 79884>, <12638 1259>, <9953 1242>, <4658 1228>, <5197 1210>,
         <12043 1201>, <6892 1183>, <7096 1168>, <10924 1149>, <5551 1136>, 
         <5580 1123>, <3735 1107>, <3652 1091>, <12191 1076>, <14214 1062>, 
         <13056 1045>, <14816 1031>, <15205 1017>, <10736 1001>, <9804 989>, 
         <13081 974>, <6706 960>, <13698 944>, <14369 928>, <16806 917>, 
         <9599 906>, <9395 897>, <4885 883>, <10237 870>, <10676 858>,
         <18518 845>, <2619 833>, <13715 822>, <11065 810>, <9590 799>, 
         <5747 785>, <2627 776>, <8962 764>, <5575 750>, <3448 738>, 
         <5731 725>, <9434 714>, <3163 703>, <3307 690>, <3248 678>, 
         <3259 667>, <3425 657>, <3506 648>, <3270 639>, <3634 627>, 
         <3077 617>, <3511 606>, <27159 597>, <27770 589>, <28496 580>, 
         <28481 571>, <29358 562>, <31027 552>, <30240 543>, <30643 534>,
         <31351 527>, <31993 519>, <32853 510>, <33078 502>, <33688 495>, 
         <29732 487>, <29898 480>, <29878 474>, <26046 468>, <26549 461>, 
         <28792 453>, <26101 446>, <32971 439>, <29704 432>, <23193 426>, 
         <29509 421>, <27079 415>, <32453 409>, <24737 404>, <25725 400>, 
         <23755 395>, <52538 393>, <53242 386>, <19609 380>, <26492 377>, 
         <24566 358>, <31163 368>, <57174 363>, <26639 364>, <31365 357>,
         <60918 350>, <21235 338>, <28072 322>, <28811 314>, <27571 320>, 
         <17635 309>, <51968 169>, <54367 323>, <60541 254>, <26732 270>, 
         <52457 157>, <27181 276>, <19874 227>, <22797 320>, <59346 271>, 
         <25496 260>, <54265 231>, <22281 250>, <42977 318>, <26008 240>, 
         <87604 142>, <94647 314>, <52292 157>, <20999 216>, <89253 316>, 
         <22746 29>, <68338 312>, <22557 317>, <110904 104>, <70975 285>,
         <51835 277>, <51871 313>, <132221 228>, <18522 290>, <68512 285>, 
         <118816 302>, <150865 268>, <68871 273>, <68139 290>, <84984 285>, 
         <150693 266>, <396047 272>, <84923 269>, <215562 258>, <68015 248>, 
         <247689 235>, <214471 229>, <264395 221>, <263287 212>, <280193 201>, 
         <108065 194>, <263616 187>, <148609 176>, <263143 173>, <378205 162>, 
         <312547 154>, <50400 147>, <328927 140>, <279217 132>, <181111 127>,
         <672098 118>, <657196 113>, <459383 111>, <833281 105>, <520281 102>, 
         <755397 95>, <787994 91>, <492444 82>, <1016592 77>, <656147 71>, 
         <819893 66>, <165531 61>, <886503 57>, <1016551 54>, <3547827 49>, 
         <14398170 43>, <395900 41>, <4950628 37>, <11481175 33>, 
         <100014881 30>, <8955328 31>, <11313984 27>, <13640855 23>,
         <528553762 21>, <63483027 17>, <952477 8>, <950580 4>, <918378 2>, 
         <918471 1> )
      ) 
   } 
}
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=hVfdbltFEL7PBc8wtBHYjRPt7O7sj-JGwAWiEuUFUCu5yXFriO3KPyVVmyfhphdwyQvxNHy7s3bsUwRRm3jO2fn75puZ9e9_rCa_bj99-nO7mZ6nv7_4a719Rc8W027VLa67wen1cj6fLG5GdPpqtlmP6Ju3q-Xr1WQ-pA8nRDR_T3S63G7oKZnLk_Jk1W22q4U-3C5uu_V6r3Px3XJ5q6emyxW9rCbp_IpOf6EP9JiebbrVZNPR8l23os2bjsr7clodnc4Wb6unXVB09pEG0D0bE4fhZTu5P_bDZP1ms1ouBvrkIfafzQv8G1Wb88ndHc4evuIXe1slzP0rvrh48qKEOzgFEEWzgfB_fnfH7-j8qert7N-3v7MpDXYGzr56SkxD-hKQApYK5NnH8gx5Ituqc9-D-vLk_uSklO7B-eLBcYvzMT2f3c0Wr2m9mbzukP_sjtTnb7PNG1pPblGydT2wfbVZTa43s-WiVfl0XoBf0DmMagkfV18H5rbrju4AGB5Om8m3q9m8o-tlN53OrmfdQstZUB2fWzp3dCZ0FumcGf8hcfmcryon3u7Brb4H-H32Uv-Ut8MhkCJz9337uSTF5fNEyf5XqpObm9kuzwdHyLJvf2d5ebO9nayaabXxYzefrbqv1zSdrDcE291qMdnM3nW0WdK8KCxRrJvtHtBWuUHx9aSVCFk5-5nTVtbn3z77adDKuJ68P-hQj-yCH9FgbMhejYbDhwYzFxcvrYSK5myPZlF_BPnR6LDPZyMqRh7oPE4xBoo5JX81ojHb4BKxlVyknMVB8LYIPkh5Y1MRhHOEwOZqdGCLrfFFwXA5E1K2xJxcEaLJAUKo2myy9ZB89SIiDMEFCAfGRJLBY1vVXXQCwcQqBIFhk1kj5gx1E0OVvGVYNsEeG2NngBAbL3oqcZGcWhBrYNxw1NiiK-80h5yMp5zyZ9YSU44VshBNoBxMVXYhJ8pesfSQKCtgyLwcqz4OLGXJOGJq7NlloZRrFD4lfFbo2FgXKUX1YALqlSQdA5-EEyXNzgbOlJzqusgwZGsFgV-AwNUQPBvUvZeZRB8ppmbH4rPimnKwFIPXekWhKNWK8z5RdKlvxTFFW61k7zxFrpqOg6NotKLORApZrVhYCbFnxYGFFIKW3FuhIPpZgGTwSTWjoeCy0gKeEHPPionww00TEygo2hbAZBKF28YIO1LrPLbJg6tg37EhPEbRJVZe2OzQDVJphrQMkBJRwVgPU16TNAENIc4fVcsxAiGxGhPnjCOsKdgkRTA7hODDqFkXUiKfpRdUjs6ST5oFiIozNfAiQNsrR20wPpDX7rNBfIbA_fwi-tWLaxwyTN4HjSpHCIoyPqMlvKtRWUTvyNvQj0oMPFiFKpoIgaVVGgl6o6Z8BLG90RAFhIHQR91FEXI177FYwXRyuYYosGXJJe17NGCGoKkHj0xc7DEBrkMgJ0ocLlx0igiKGj2Eljr4BKExFsMGOvGogPCFdnPaAhYjCkecYptMhGvtN5sS-OaU-zbCC9708uMYirYCAhYEzNigAsZHhEINKhjxTFZaNUvRbeyZEiSIoSyN0Qy22jYVwQVPVgmHPzm2QMaSHYhhY58LUlrA6lAT1FfI6rC0WAAlkPrGgwwwxY1WxiQC-Y9NpQjuEesOyT74uENErC0LosVrMiah5TZtUGgc69PKxhJs1uXiChVYgbaYSRBim3K5eFRaYe9gWtky0Q6hwlbC08qQIpTKcBuYyBAJtqmdECQ8GnUpbNXUUQU5lW3itE9ZTCpoKbHQs7FUQWuYGMRqxjBjkv8XY5ht6ChwsLIPrAZeNtqmYssr7R0WTB8UovkxGPPW9yYoGgxLGKXTec7el2gUQDRJ2Te2NWnA6IGnhifMlShMjxZsUtkgnBsNXShrVIcPdmppQVbG4R2Xq0Bs2zvVFdvfyiif-EJZ5YNB94MpsU3CjKHKXnsMowmXRtapA2bjB_eMXldGkCiVG0eFBO1Wrxw1Ag-iJ8TDNVtwp7CYTZsqRqVedBg8Dp2ioydiOmZcBao-CuG9x1rVjcxBQGQlU5DAZYX2kEtoQSCqZcV5wYIMGgvoYjDz495W2Q06fABOAgi-f_1A5RJjYbUlU1Y5hBYaGGTRGa0XsLM4lvHUryQuOLhkgPdKx4yQipreiBjdUPmpVjAMTZIyA467CPQVFwsN9cLnfHJlFWojZgwkjIekn025ztW0MDtBCOqhjaeFnXxFw_Z0qFd9OrnXb63ty-vuS-w_)


```
14106184687260844995
0 0
1 1
2 1
3 1
4 2
5 2
6 2
7 2
8 2
9 3
10 3
11 3
12 3
13 3
14 3
15 3
16 4
17 4
18 4
19 4
20 4
21 4
22 4
23 4
24 4
25 5
26 5
27 5
28 5
29 5
30 5
31 5
32 5
33 5
34 5
35 5
36 6
37 6
38 6
39 6
40 6
41 6
42 6
43 6
44 6
45 6
46 6
47 6
48 6
49 7
50 7
51 7
52 7
53 7
54 7
55 7
56 7
57 7
58 7
59 7
60 7
61 7
62 7
63 7
64 8
65 8
66 8
67 8
68 8
69 8
70 8
71 8
72 8
73 8
74 8
75 8
76 8
77 8
78 8
79 8
80 8
81 9
82 9
83 9
84 9
85 9
86 9
87 9
88 9
89 9
90 9
91 9
92 9
93 9
94 9
95 9
96 9
97 9
98 9
99 9
100 10
101 10
102 10
103 10
104 10
105 10
106 10
107 10
108 10
109 10
110 10
111 10
112 10
113 10
114 10
115 10
116 10
117 10
118 10
119 10
120 10
121 11
122 11
123 11
124 11
125 11
126 11
127 11
128 11
129 11
130 11
131 11
132 11
133 11
134 11
135 11
136 11
137 11
138 11
139 11
140 11
141 11
142 11
143 11
144 12
145 12
146 12
147 12
148 12
149 12
150 12
151 12
152 12
153 12
154 12
155 12
156 12
157 12
158 12
159 12
160 12
161 12
162 12
163 12
164 12
165 12
166 12
167 12
168 12
169 13
170 13
171 13
172 13
173 13
174 13
175 13
176 13
177 13
178 13
179 13
180 13
181 13
182 13
183 13
184 13
185 13
186 13
187 13
188 13
189 13
190 13
191 13
192 13
193 13
194 13
195 13
196 14
197 14
198 14
199 14
200 14
201 14
202 14
203 14
204 14
205 14
206 14
207 14
208 14
209 14
210 14
211 14
212 14
213 14
214 14
215 14
216 14
217 14
218 14
219 14
220 14
221 14
222 14
223 14
224 14
225 15
226 15
227 15
228 15
229 15
230 15
231 15
232 15
233 15
234 15
235 15
236 15
237 15
238 15
239 15
240 15
241 15
242 15
243 15
244 15
245 15
246 15
247 15
248 15
249 15
250 15
251 15
252 15
253 15
254 15
255 15
```
