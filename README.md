# ArchLab2

## Βήμα 1ο:
### 1. Από το αρχείο config.ini
|cache | size | associativity |
| ---- | ---- | ------------- |
|L1 instruction |32kB |2 | 
|L1 data |64kB |2|   
|L2 |2MB|8|
 
cache_line_size=64  
in [system.cpu.icache] [system.cpu.dcache] [system.l2]

### 2. Από τα αντίστοιχα stats.txt

|Benchmark |	sim_seconds	| CPI	| L1 icache missrate	| L1 dcache missrate	| L2 cache missrate |
|--------- | ----------- | --- | ------------------ | ------------------ | ----------------- |
|specbzip | 0.083982|1.679650|0.000077| 0.014798| 0.282163 |
|spechmmer|0.059396 |1.187917 |0.000221|0.001637|0.077760|
|speclibm |0.174671|3.493415|0.000094|0.060972|0.999944|
|speccmcf|0.064955 |1.299095|0.023612|0.002108|0.055046|
|specsjeng|0.513528|10.270554|0.000020|0.121831|0.999972|

Ο μεγαλύτερος χρόνος και μεγαλύτερο cpi προκύπτουν από τα μεγαλύτερο missrate στην l1dcache και l2. Το κάθε benchmark έχει φυσικά διαφορετικές απαιτήσεις σε μνήμη, το οποίο πρέπει να θυμόμαστε όταν παίρνουμε σχεδιαστικές αποφάσεις.

### 3. 

Παρατηρούμε πως αλλάζει το system.cpu_clk_domain.clock σε 333 στα 3Ghz, ενώ το system.clk_domain.clock παραμένει στα 1000. Αυτό είναι λογικό αφού μεταβάλλαμε την συχνότητα cpu και όχι τον συγχρονισμό του συστήματος.Οι παραπάνω πυρήνες θα έτρεχαν σε system.cpu_clk_domain.clock.

|Benchmarks | sim_sec_1GHz | sim_sec_2GHz | sim_sec_3GHz |
|---------- | ---- | ---- | ---- |
|specbzip| 0.161025 | 0.083982|0.058385|
|spechmmer|0.118530 | 0.059396| 0.039646|
|speclibm|0.262327 |0.174671| 0.146433|
|speccmcf| 0.127942 |0.064955|0.043867|
|specsjeng|0.704056 |0.513528 |0.449821|

Παρατηρούμε πως ενώ αρχικά από 1GHz σε 2 GHz φαίνεται ο χρόνος(sim_seconds) να μειώνεται περίπου στο μισό, το οποίο θα μας προετοίμαζε για τέλειο scaling, αυτό δεν συμβαίνει καταρχάς για όλα τα benchmarks, καθώς και ενώ στα 3GHz υπάρχει βελτίωση(ιδιαίτερα φαίνεται στο spechmmer speccmcf) δεν υπάρχει κατά κύριο λόγω τέλειο scaling. 

### 4.

|Benchmark |	sim_seconds	| CPI	| L1 icache missrate	| L1 dcache missrate	| L2 cache missrate |
|--------- | ----------- | --- | ------------------ | ------------------ | ----------------- |
|speccmcf|0.064955 |1.299095|0.023612|0.002108|0.055046|
speccmcf_2133|0.064892 |1.297844|0.023612|0.002108|0.055046|

Βελτιώνοντας το clock της μνήμης για το benchmark specmcf παρατηρούμε πως τα sim_seconds :  0.064892 ενώ πριν  0.064955. Υπάρχει μια μικρή βελτίωση, αλλά καθώς οι caches είναι ίδιες ουσιαστικά βελτιώνονται οι περιπτώσεις στις οποίες έχουμε miss. Και όσο λιγότερα τα misses στα caches τόσο λιγότερη σημασία έχει η dram.


## Βήμα 2ο:

Hit time + Miss rate × Miss penalty

In order to reduce miss rate: larger cache size ->reducing capacity misses, higher associativity->reduces conflict misses  
Increasing capacity of cache:also increases hit time  
Reducing miss rate via larger cache line size  



|Benchmarks|L1 icache size|L1 icache associativity| L1 dcache size|L1 dcache associativity|L2 cache size|L2 cache associativity| cache line size|
|--|--|--|--|--|--|--|--|
|spec*_0|64kB|1|32kB|1|512kB|2|32|
|spec*_1|64kB|1|128kB|1|4MB|2|64|
|spec*_2|64kB|4|128kB|4|4MB|16|64|
|spec**_3|32kB|8|32kB|8|512kB|8|64|
|specsjeng_3|128kB|16|128kB|16|1MB|16|128|

Όπου * = {bzip,hmmer,libm,mcf,sjeng} και ** = {bzip,hmmer,libm,mcf}.

  
|Benchmarks	|CPI	|dcache.overall_miss_rate::total|icache.overall_miss_rate::total	|l2.overall_miss_rate::total|
|---------- |----|------------------------------------------|--------------------------------------------|----------|
|specbzip_0	|2.021019	|0.024030|	0.000089|	0.399687|
|specbzip_1	|1.652107	|0.014176|	0.000078|	0.265281|
|specbzip_2	|1.616016 |0.010949|	0.000070|	0.351744|
|specbzip_3 |1.770076 |0.015944| 0.000070| 0.359072|

|Benchmarks	|CPI	|dcache.overall_miss_rate::total|	icache.overall_miss_rate::total	|l2.overall_miss_rate::total|
|--|--|--|--|--|
|spechmmer_0|	1.232365|	0.006489|	0.000421|	0.037090|
|spechmmer_1|	1.192635|	0.001465|	0.000402|	0.081824|
|spechmmer_2|	1.185557|	0.000669|	0.000083|	0.207342|
|spechmmer_3| 1.188299| 0.002034| 0.000090| 0.064058|


|Benchmarks	|CPI	|dcache.overall_miss_rate::total|icache.overall_miss_rate::total	|l2.overall_miss_rate::total|
|--|--|--|--|--|
|speclibm_0 |5.619003|	0.122417|	0.000090|	0.994137|
|speclibm_1	|3.499635|0.061266	|0.000097|	0.993145|
|speclibm_2	|3.489571|0.060972	|0.000085|	0.999979|
|speclibm_3 |3.496180|0.060972|0.000085| 0.999979|

|Benchmarks	|CPI	|dcache.overall_miss_rate::total|icache.overall_miss_rate::total	|l2.overall_miss_rate::total|
|--|--|--|--|--|
|specmcf_0|	1.225828|	0.007979|	0.000048	|0.340802|
|specmcf_1|1.156079 |	0.002399|	0.000042	|0.614935|
|specmcf_2|1.154443 |0.001877	|0.000018	 |0.789452|
|specmcf_3|1.165041 |0.002345 |0.000018 |0.768573|

|Benchmarks	|CPI	|dcache.overall_miss_rate::total|icache.overall_miss_rate::total	|l2.overall_miss_rate::total|
|--|--|--|--|--|
|specsjeng_0	|17.669434|	0.243970|	0.000023	|0.997409|
|specsjeng_1	|10.284161|	0.122368	|0.000020|	0.991255|
|specsjeng_2	|10.264661	|0.121831	|0.000019	|0.999986|
|specsjeng_3 |6.798895| 0.060918|0.000013| 0.999978|


## Βήμα 3ο:
### Συνάρτηση κόστους

Γνωρίζοντας πως η L1 είναι ακριβότερη από την L2, πως αυξάνοντας το associativity αυξάνεται η πολυπλοκότητα(και άρα το κόστος), και καθώς όσο μεγαλύτερη η μνήμη,τόσο περισσότερο κοστίζει θεωρούμε:
##### f = (l1d_cost+l1i_cost+l2_cost)*cpi  
όπου  
L1*_cost = 8*kB + 2*associativity  
l2_cost = kB + 1*associativity  

π.χ
spec*_0 : 8*(l1_size)+ 2*(associativity_l1d+associativity_l1i) + 1*l2_size + 1*associativity_l2  => 8*(64+32)+ 2*2*1 + 512 + 1*2 = 1286  
spec*_1: 5638  
spec*_2: 5664  
spec**_3: 1064  
specjeng_3:  

Άρα f:  
specbzip_0 : cost*cpi => 1286*2.021019 = 2,599.0304  
specbzip_1 : 9,314.57  
specbzip_2 : 9,153.11  
specbzip_3 : 1,883.36  

spechmmer_0: 1,584.8214  
spechmmer_1: 6,724.0761  
spechmmer_2: 6,714.9948  
spechmmer_3: 1,264.3501  

speclibm_0: 7,226.0379  
speclibm_1: 19,730.9421  
speclibm_2: 19,764.9301  
speclibm_3: 3,719.9355  

specmcf_0 : 1,576.4148  
specmcf_1: 6,517.9734  
specmcf_2:6,538.7651  
specmcf_3: 1,239.6036  

specsjeng_0: 22,722.8921  
specsjeng_1: 57,982.0997  
specsjeng_2: 58,139.0399  
specsjeng_3: 21,430.1170  

###### To_do : make last section into bar_charts

