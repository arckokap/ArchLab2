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
|specbzip ||||||
|spechmmer|
|speclibm |
