[1]: https://rosettacode.org/wiki/Abbreviations,_automatic

# [Abbreviations, automatic][1]





Saving the "Days of Week, Also Known As"  table to a local file [DoWAKA.txt](https://github.com/thundergnat/rc-run/blob/master/rc/resources/DoWAKA.txt). Lines that have duplicate day names will get ∞ as the minimum number of characters, as there is no amount of characters that can be entered to distinguish the days uniquely. It is somewhat unclear as to what is meant by "return a null string". I have chosen to return Nil.



Note that this is using a previous version of the date file that has erroneous duplicate day names (see line 90). Since the effort was already expended to catch such problems, it may as well be demonstrated.

```perl
sub auto-abbreviate ( Str $string ) {
    return Nil unless my @words = $string.words;
    return $_ if @words».substr(0, $_).Set == @words for 1 .. @words».chars.max;
    return '∞';
}

# Testing
 say ++$, ') ', .&auto-abbreviate, '  ', $_ for './DoWAKA.txt'.IO.lines;
```

#### Output:
```
1) 2  Sunday Monday Tuesday Wednesday Thursday Friday Saturday
2) 2  Sondag Maandag Dinsdag Woensdag Donderdag Vrydag Saterdag
3) 4  E_djelë E_hënë E_martë E_mërkurë E_enjte E_premte E_shtunë
4) 2  Ehud Segno Maksegno Erob Hamus Arbe Kedame
5) 5  Al_Ahad Al_Ithinin Al_Tholatha'a Al_Arbia'a Al_Kamis Al_Gomia'a Al_Sabit
6) 4  Guiragui Yergou_shapti Yerek_shapti Tchorek_shapti Hink_shapti Ourpat Shapat
7) 2  domingu llunes martes miércoles xueves vienres sábadu
8) 2  Bazar_gÜnÜ Birinci_gÜn Çkinci_gÜn ÜçÜncÜ_gÜn DÖrdÜncÜ_gÜn Bes,inci_gÜn Altòncò_gÜn
9) 6  Igande Astelehen Astearte Asteazken Ostegun Ostiral Larunbat
10) 4  Robi_bar Shom_bar Mongal_bar Budhh_bar BRihashpati_bar Shukro_bar Shoni_bar
11) 2  Nedjelja Ponedeljak Utorak Srijeda Cxetvrtak Petak Subota
12) 5  Disul Dilun Dimeurzh Dimerc'her Diriaou Digwener Disadorn
13) 2  nedelia ponedelnik vtornik sriada chetvartak petak sabota
14) 13  sing_kei_yath sing_kei_yat sing_kei_yee sing_kei_saam sing_kei_sie sing_kei_ng sing_kei_luk
15) 4  Diumenge Dilluns Dimarts Dimecres Dijous Divendres Dissabte
16) 16  Dzeenkk-eh Dzeehn_kk-ehreh Dzeehn_kk-ehreh_nah_kay_dzeeneh Tah_neesee_dzeehn_neh Deehn_ghee_dzee-neh Tl-oowey_tts-el_dehlee Dzeentt-ahzee
17) 6  dy_Sul dy_Lun dy_Meurth dy_Mergher dy_You dy_Gwener dy_Sadorn
18) 2  Dimanch Lendi Madi Mèkredi Jedi Vandredi Samdi
19) 2  nedjelja ponedjeljak utorak srijeda cxetvrtak petak subota
20) 2  nede^le ponde^lí úterÿ str^eda c^tvrtek pátek sobota
21) 2  Sondee Mondee Tiisiday Walansedee TOOsedee Feraadee Satadee
22) 2  s0ndag mandag tirsdag onsdag torsdag fredag l0rdag
23) 2  zondag maandag dinsdag woensdag donderdag vrijdag zaterdag
24) 2  Diman^co Lundo Mardo Merkredo ^Jaùdo Vendredo Sabato
25) 1  pÜhapäev esmaspäev teisipäev kolmapäev neljapäev reede laupäev
26) Nil  
27) 7  Diu_prima Diu_sequima Diu_tritima Diu_quartima Diu_quintima Diu_sextima Diu_sabbata
28) 2  sunnudagur mánadagur tÿsdaguy mikudagur hósdagur friggjadagur leygardagur
29) 2  Yek_Sham'beh Do_Sham'beh Seh_Sham'beh Cha'har_Sham'beh Panj_Sham'beh Jom'eh Sham'beh
30) 2  sunnuntai maanantai tiistai keskiviiko torsktai perjantai lauantai
31) 2  dimanche lundi mardi mercredi jeudi vendredi samedi
32) 4  Snein Moandei Tiisdei Woansdei Tonersdei Freed Sneon
33) 2  Domingo Segunda_feira Martes Mércores Joves Venres Sábado
34) 2  k'vira orshabati samshabati otkhshabati khutshabati p'arask'evi shabati
35) 2  Sonntag Montag Dienstag Mittwoch Donnerstag Freitag Samstag
36) 2  Kiriaki' Defte'ra Tri'ti Teta'rti Pe'mpti Paraskebi' Sa'bato
37) 3  ravivaar somvaar mangalvaar budhvaar guruvaar shukravaar shanivaar
38) 6  pópule pó`akahi pó`alua pó`akolu pó`ahá pó`alima pó`aono
39) 7  Yom_rishon Yom_sheni Yom_shlishi Yom_revi'i Yom_chamishi Yom_shishi Shabat
40) 3  ravivara somavar mangalavar budhavara brahaspativar shukravara shanivar
41) 3  vasárnap hétfö kedd szerda csütörtök péntek szombat
42) 2  Sunnudagur Mánudagur ╞riδjudagur Miδvikudagar Fimmtudagur FÖstudagur Laugardagur
43) 2  sundio lundio mardio merkurdio jovdio venerdio saturdio
44) 3  Minggu Senin Selasa Rabu Kamis Jumat Sabtu
45) 2  Dominica Lunedi Martedi Mercuridi Jovedi Venerdi Sabbato
46) 4  Dé_Domhnaigh Dé_Luain Dé_Máirt Dé_Ceadaoin Dé_ardaoin Dé_hAoine Dé_Sathairn
47) 2  domenica lunedí martedí mercoledí giovedí venerdí sabato
48) 2  Nichiyou_bi Getzuyou_bi Kayou_bi Suiyou_bi Mokuyou_bi Kin'you_bi Doyou_bi
49) 1  Il-yo-il Wol-yo-il Hwa-yo-il Su-yo-il Mok-yo-il Kum-yo-il To-yo-il
50) 7  Dies_Dominica Dies_Lunæ Dies_Martis Dies_Mercurii Dies_Iovis Dies_Veneris Dies_Saturni
51) 3  sve-tdien pirmdien otrdien tresvdien ceturtdien piektdien sestdien
52) 2  Sekmadienis Pirmadienis Antradienis Trec^iadienis Ketvirtadienis Penktadienis S^es^tadienis
53) 3  Wangu Kazooba Walumbe Mukasa Kiwanuka Nnagawonye Wamunyi
54) 12  xing-_qi-_rì xing-_qi-_yi-. xing-_qi-_èr xing-_qi-_san-. xing-_qi-_sì xing-_qi-_wuv. xing-_qi-_liù
55) 3  Jedoonee Jelune Jemayrt Jecrean Jardaim Jeheiney Jesam
56) 3  Jabot Manre Juje Wonje Taije Balaire Jarere
57) 5  geminrongo minòmishi mártes mièrkoles misheushi bèrnashi mishábaro
58) 2  Ahad Isnin Selasa Rabu Khamis Jumaat Sabtu
59) 2  sφndag mandag tirsdag onsdag torsdag fredag lφrdag
60) 7  lo_dimenge lo_diluns lo_dimarç lo_dimèrcres lo_dijòus lo_divendres lo_dissabte
61) 4  djadomingo djaluna djamars djarason djaweps djabièrna djasabra
62) 2  Niedziela Poniedzial/ek Wtorek S,roda Czwartek Pia,tek Sobota
63) 3  Domingo segunda-feire terça-feire quarta-feire quinta-feire sexta-feira såbado
64) 1  Domingo Lunes martes Miercoles Jueves Viernes Sabado
65) 2  Duminicª Luni Mart'i Miercuri Joi Vineri Sâmbªtª
66) 2  voskresenie ponedelnik vtornik sreda chetverg pyatnitsa subbota
67) 4  Sunday Di-luain Di-màirt Di-ciadain Di-ardaoin Di-haoine Di-sathurne
68) 2  nedjelja ponedjeljak utorak sreda cxetvrtak petak subota
69) 5  Sontaha Mmantaha Labobedi Laboraro Labone Labohlano Moqebelo
70) 2  Iridha- Sandhudha- Anga.haruwa-dha- Badha-dha- Brahaspa.thindha- Sikura-dha- Sena.sura-dha-
71) 2  nedel^a pondelok utorok streda s^tvrtok piatok sobota
72) 2  Nedelja Ponedeljek Torek Sreda Cxetrtek Petek Sobota
73) 2  domingo lunes martes miércoles jueves viernes sábado
74) 2  sonde mundey tude-wroko dride-wroko fode-wroko freyda Saturday
75) 7  Jumapili Jumatatu Jumanne Jumatano Alhamisi Ijumaa Jumamosi
76) 2  söndag måndag tisdag onsdag torsdag fredag lordag
77) 2  Linggo Lunes Martes Miyerkoles Huwebes Biyernes Sabado
78) 6  Lé-pài-jít Pài-it Pài-jï Pài-sañ Pài-sì Pài-gÖ. Pài-lák
79) 7  wan-ar-tit wan-tjan wan-ang-kaan wan-phoet wan-pha-ru-hat-sa-boh-die wan-sook wan-sao
80) 5  Tshipi Mosupologo Labobedi Laboraro Labone Labotlhano Matlhatso
81) 6  Pazar Pazartesi Sali Çar,samba Per,sembe Cuma Cumartesi
82) 2  nedilya ponedilok vivtorok sereda chetver pyatnytsya subota
83) 8  Chu?_Nhâ.t Thú*_Hai Thú*_Ba Thú*_Tu* Thú*_Na'm Thú*_Sáu Thú*_Ba?y
84) 6  dydd_Sul dyds_Llun dydd_Mawrth dyds_Mercher dydd_Iau dydd_Gwener dyds_Sadwrn
85) 3  Dibeer Altine Talaata Allarba Al_xebes Aljuma Gaaw
86) 7  iCawa uMvulo uLwesibini uLwesithathu uLuwesine uLwesihlanu uMgqibelo
87) 2  zuntik montik dinstik mitvokh donershtik fraytik shabes
88) 7  iSonto uMsombuluko uLwesibili uLwesithathu uLwesine uLwesihlanu uMgqibelo
89) 7  Dies_Dominica Dies_Lunæ Dies_Martis Dies_Mercurii Dies_Iovis Dies_Veneris Dies_Saturni
90) ∞  Bazar_gÜnÜ Bazar_ærtæsi Çærs,ænbæ_axs,amò Çærs,ænbæ_gÜnÜ CÜmæ_axs,amò CÜmæ_gÜnÜ CÜmæ_gÜnÜ
91) 2  Sun Moon Mars Mercury Jove Venus Saturn
92) 2  zondag maandag dinsdag woensdag donderdag vrijdag zaterdag
93) 2  KoseEraa GyoOraa BenEraa Kuoraa YOwaaraa FeEraa Memenaa
94) 5  Sonntag Montag Dienstag Mittwoch Donnerstag Freitag Sonnabend
95) 1  Domingo Luns Terza_feira Corta_feira Xoves Venres Sábado
96) 7  Dies_Solis Dies_Lunae Dies_Martis Dies_Mercurii Dies_Iovis Dies_Veneris Dies_Sabbatum
97) 12  xing-_qi-_tiàn xing-_qi-_yi-. xing-_qi-_èr xing-_qi-_san-. xing-_qi-_sì xing-_qi-_wuv. xing-_qi-_liù
98) 4  djadomingu djaluna djamars djarason djaweps djabièrnè djasabra
99) 2  Killachau Atichau Quoyllurchau Illapachau Chaskachau Kuychichau Intichau
```
