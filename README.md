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

### 3. 

Παρατηρούμε πως αλλάζει το system.cpu_clk_domain.clock σε 333 στα 3Ghz, ενώ το system.clk_domain.clock παραμένει στα 1000. Αυτό είναι λογικό αφού μεταβάλλαμε την συχνότητα cpu και όχι τον συγχρονισμό του συστήματος.Οι παραπάνω πυρήνες θα έτρεχαν σε system.cpu_clk_domain.clock.

|Benchmarks | 1GHz | 2GHz | 3GHz |
|---------- | ---- | ---- | ---- |
|specbzip| 0.161025 | 0.083982|0.058385|
|spechmmer|0.118530 | 0.059396| 0.039646|
|speclibm|0.262327 |0.174671| 0.146433|
|speccmcf| 0.127942 |0.064955|0.043867|
|specsjeng|0.704056 |0.513528 |0.449821|

Παρατηρούμε πως ενώ αρχικά από 1GHz σε 2 GHz φαίνεται ο χρόνος(sim_seconds) να μειώνεται περίπου στο μισό, το οποίο θα μας προετοίμαζε για τέλειο scaling, αυτό δεν συμβαίνει καταρχάς για όλα τα benchmarks, καθώς και ενώ στα 3GHz υπάρχει βελτίωση(ιδιαίτερα φαίνεται στο spechmmer speccmcf) δεν υπάρχει κατά κύριο λόγω τέλειο scaling. 


  
