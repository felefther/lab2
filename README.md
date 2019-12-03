# Αρχιτεκτονική Υπολογιστών 7ο Εξάμηνο

## Κυπαρίσσης Οδυσσέας ΑΕΜ : 8955

## Φατόλας Ελευθέριος ΑΕΜ : 8958

## Ομάδα 24

## Αριστοτέλειο Πανεπιστήμιο Θεσσαλονίκης

## Τμήμα Ηλεκτρολόγων Μηχανικών και Μηχανικών Υπολογιστών



## 2η Εργαστηριακή Άσκηση



### **Βήμα 1**

**1.**  
Σύμφωνα με το αρχείο **options.py** το υποσύστημα μνήμης του επεξεργαστή που εξομοιώνεται θα έχει τις παρακάτω τιμές:

[system]  
cache_line_size=64

[system.cpu.dcache]  
assoc=2  
size=65536

[system.cpu.icache]  
assoc=2  
size=32768

[system.l2]  
assoc=8  
size=2097152
~~~
   Cache Options
    parser.add_option("--l1d_size", type="string", default="64kB")
    parser.add_option("--l1i_size", type="string", default="32kB")
    parser.add_option("--l2_size", type="string", default="2MB")
    parser.add_option("--l1d_assoc", type="int", default=2)
    parser.add_option("--l1i_assoc", type="int", default=2)
    parser.add_option("--l2_assoc", type="int", default=8)
    parser.add_option("--cacheline_size", type="int", default=64)
~~~

**2.**  
Εκτελώντας τις προσομοιώσεις για τα 5 benchmarks παίρνουμε τα παρακάτω αποτελέσματα, τα οποία φαίνονται στα αρχεία **stats.txt**. Η default τιμή του **cpu clock** είναι 1 GHz.  

Για το **specbzip** έχουμε:
~~~
sim_seconds                                  0.161025                       # Number of seconds simulated
system.cpu.cpi                               1.610247                       # CPI: cycles per instruction
system.cpu.icache.overall_miss_rate::total     0.000077                       # miss rate for overall accesses
system.cpu.dcache.overall_miss_rate::total     0.014675                       # miss rate for overall accesses
system.l2.overall_miss_rate::total           0.282157                       # miss rate for overall accesses
~~~

Για το **spechmmer** έχουμε:
~~~
sim_seconds                                  0.11853                       # Number of seconds simulated
system.cpu.cpi                               1.185304                       # CPI: cycles per instruction
system.cpu.icache.overall_miss_rate::total     0.000221                       # miss rate for overall accesses
system.cpu.dcache.overall_miss_rate::total     0.001629                       # miss rate for overall accesses
system.l2.overall_miss_rate::total           0.077747                       # miss rate for overall accesses
~~~

Για το **speclibm** έχουμε:
~~~
sim_seconds                                  0.262327                       # Number of seconds simulated
system.cpu.cpi                               2.623265                       # CPI: cycles per instruction
system.cpu.icache.overall_miss_rate::total     0.000094                       # miss rate for overall accesses
system.cpu.dcache.overall_miss_rate::total     0.060971                       # miss rate for overall accesses
system.l2.overall_miss_rate::total           0.999944                       # miss rate for overall accesses
~~~

Για το **specmcf** έχουμε:
~~~
sim_seconds                                  0.127942                       # Number of seconds simulated
system.cpu.cpi                               1.279422                       # CPI: cycles per instruction
system.cpu.icache.overall_miss_rate::total     0.023627                       # miss rate for overall accesses
system.cpu.dcache.overall_miss_rate::total     0.002108                       # miss rate for overall accesses
system.l2.overall_miss_rate::total           0.055046                       # miss rate for overall accesses
~~~
Για το **specsjeng** έχουμε:
~~~
sim_seconds                                  0.704056                       # Number of seconds simulated
system.cpu.cpi                               7.040561                       # CPI: cycles per instruction
system.cpu.icache.overall_miss_rate::total     0.000020                       # miss rate for overall accesses
system.cpu.dcache.overall_miss_rate::total     0.121831                       # miss rate for overall accesses
system.l2.overall_miss_rate::total           0.99972                       # miss rate for overall accesses
~~~

--------NEW LINKS------------

![sim_sec](https://github.com/felefther/lab2/blob/master/sim_sec.png)
![cpi_graph](https://github.com/felefther/lab2/blob/master/cpi.png)
![dcache](https://github.com/felefther/lab2/blob/master/dcache.png)
![icache](https://github.com/felefther/lab2/blob/master/icache.png)
![l2](https://github.com/felefther/lab2/blob/master/l2.png)


Από τα αποτελέσματα παρατηρούμε καταρχάς ότι σε όλα τα benchmarks το cpi είναι δεκαπλάσιο του χρόνου εκτέλεσης, όπως και πρέπει να ισχύει καθώς **ο χρόνος εκτέλεσης = cpi * αριθμό εντολών / συχνότητα**.Ακόμα βλέπουμε ότι στα benchmarks με υψηλό cpi έχουμε και μεγαλύτερο total miss rate στην dcache και στην l2.Αντίθετα υψηλό total miss rate στην icache έχουμε μόνο στο specmcf.


**3.**  
Τρέχοντας ξανά όλα τα benchmarks με την παράμετρο --cpu-clock=2GHz παρατηρούμε τα εξής:  
Η παράμετρος **system.clk_domain.clock** παραμένει στα 1000 ticks, δηλαδή το system clock παρέμεινε στα 1GHz, αναμενόμενο καθώς δεν ζητήσαμε να αλλάξει.Ωστόσο το **system.cpu_clk_domain.clock** άλλαξε όντως στα 500 ticks, δηλαδη τo cpu clock έγινε 2 Ghz.Όλα τα στοιχεία της μητρικής πλακέτας χρονίζονται στα 1GHz,ενώ η cpu στα 2GHz.Σε όλα τα benchmarks βλέπουμε βελτίωση της απόδοσης, ο χρόνος εκτέλεσης μειώνεται στο μισό.Το ρολόι της cpu θέλουμε σε γενικές γραμμλες να είναι μεγαλύτερο απ της μητρικής καθώς πρέπει να εκτελεί περισσότερες λειτουργίες.


### **Βήμα 2**

Σε αυτό το σημείο προσπαθούμε να μειώσουμε την τιμή του **cpi** σε κάθε benchmark.Αυτό το καταφέρνουμε επιλέγοντας διαφορετικές τιμές για το μέγεθος των **l1 και l2 cache**,το **associativity** μεταξύ τους αλλά και το **cache line size**.Αντίθετα κρατάμε σταθερές παραμέτρους όπως η συχνότητα, ο τύπος του επεξεργαστή που χρησιμοποιούμε (εδώ έχουμε minorCPU) και ο αριθμός των εντολών που θέλουμε να τρέξει, έτσι ώστε να δούμε με ποιές τιμές του υποσυστήματος μνήμης επιτυγχάνουμε μέγιστη απόδοση.    
Τρέχοντας για παράδειγμα την παρακάτω εντολή για το benchmark **speclibm** δίνουμε τις τιμές:  
* L1 instruction cache size = 64 κΒ
* L1 instruction cache associativity = 1
* L1 data cache size = 32 κΒ
* L1 data cache associativity = 1
* L2 cache size = 512 κΒ
* L2 cache associativity = 2
* cache line size = 64 κΒ


~~~
./build/ARM/gem5.opt -d spec_results/speclibm configs/example/se.py --cpu- type=MinorCPU --caches --l2cache --l1d_size=32kB --l1i_size=64kB --l2_size=512kB --l1i_assoc=1 --l1d_assoc=1 --l2_assoc=2 --cacheline_size=64 --cpu-clock=1GHz -c spec_cpu2006/470.lbm/src/speclibm -o "20 spec_cpu2006/470.lbm/data/lbm.in 0 1 spec_cpu2006/470.lbm/data/100_100_130_cf_a.of" -I 100000000
~~~

Σε όλες τις δοκιμές θα κρατήσουμε σταθερό το ρολόι της cpu στα 1 Ghz, τον τύπο επεξεργαστή Minor και τοω αριθμό των εκτελέσιμων εντολών ίσο με 100000000.

Γνωρίζουμε ότι αυξάνοντας το μέγεθος της **l2** βελτιώνεται η απόδοση μας.Επομένως προσπαθήσαμε να το εκμεταλλευτούμε όσο περισσότερο γίνεται σε συγκεκριμένα benchmarks.Επιπλέον όσο μεγαλύτερο το **cache line size** τόσο μειώνεται η πιθανότητα συγκρούσεων, άρα και το **συνολικό ποσοστό αστοχίας** το οποίο και θέλουμε.Ένας ακόμα παράγοντας που επηρεάζει το miss rate είναι το **associativity**, το οποίο όσο αυξάνεται τόσο μειώνεται το miss rate.Ωστόσο σ αυτήν την περίπτωση έχουμε και αρνητικά αποτελέσματα, όπως η αύξηση της πολυπλοκότητας.

Για το benchmark **specbzip** τρέξαμε αρκετούς συνδυασμούς και παρατηρήσαμε ότι οι παράγοντες που επηρεάζουν περισσότερο το **CPI** είναι το μέγεθος της **l2** και το **associativity** μεταξύτων μνημών.Μάλιστα όσο μεγαλύτερες ήταν οι τιμές τους, μέσα στα επιτρεπτά όρια πάντα, τόσο χαμηλότερο ήταν το **CPI**, με μικρότερη δυνατή τιμή το 1.552991.Σε αυτήν την περίπτωση τα στοιχεία του υποσυστήματος μνήμης είχαν τις παρακάτω τιμές:  
* L1 instruction cache size = 32 κΒ
* L1 instruction cache associativity = 8
* L1 data cache size = 128 κΒ
* L1 data cache associativity = 8
* L2 cache size = 4 ΜΒ
* L2 cache associativity = 8
* cache line size = 64 κΒ


Παρατηρήσαμε ότι παρόλο που το **cpi** μειώθηκε έχουμε αύξηση του **l2 miss rate**.Οι αλλαγές στα miss rate των icache, dcache είναι αρκετά μικρές.Συμπεραίνουμε ότι στο συγκεκριμένο benchmark χρειάζεται μεγάλη l2, τουλάχιστον 2 MB όπως φαίνεται και στα αποτελέσματα των  δοκιμών.  

----INSERT RESULTS.txt IMAGE----
~~~
Benchmarks	system.cpu.cpi	system.cpu.dcache.overall_miss_rate::total	system.cpu.icache.overall_miss_rate::total	system.l2.overall_miss_rate::total
specbzip_16_4_32_8_64_8_64	1.856737	0.015757	0.000080	0.749181
specbzip_16_8_16_8_16_8_64	2.042767	0.019388	0.000075	0.864965
specbzip_16_8_32_8_64_8_64	1.856671	0.015757	0.000075	0.749217
specbzip_16_8_128_8_4_8_64	1.553080	0.010150	0.000075	0.376114
specbzip_16_8_128_8_2_8_64	1.567042	0.010151	0.000075	0.421484
specbzip_16_8_128_8_64_8_64	1.716553	0.010095	0.000075	0.918881
specbzip_16_8_128_8_128_8_64	1.700894	0.010115	0.000075	0.868021
specbzip_32_8_64_8_32_8_64	1.810508	0.012536	0.000070	0.907320
specbzip_32_8_128_8_4_8_64	1.552991	0.010150	0.000070	0.376078
specbzip_32_8_128_8_2_8_64	1.567058	0.010154	0.000070	0.421497
specbzip_32_8_128_8_32_8_64	1.726795	0.010090	0.000070	0.950604
specbzip_32_8_128_8_64_8_64	1.716670	0.010094	0.000070	0.918958
specbzip_64_8_128_8_4_8_64	1.553062	0.010152	0.000070	0.376025
~~~

Στα γραφήματα που ακολουθούν βλέπουμε και το συσχετισμό μεταξύ των παραγόντων και της απόδοσης του συστήματος.Σε κάθε ένα έχουμε κρατήσει συγκεκριμές μεταβλητές σταθερές έτσι ώστε να φανεί καλύτερα ποιές μεταβλητές επηρεάζουν ποιό αποτέλεσμα.

![specbzip_l2_cpi]()
![specbzip_l2_size_l2]()
![specbzip_ld_size_dcache]()
![specbzip_li_size_icache]()

-----INSERT GRAPH----


Για το benchmark **specblibm** παρατηρήσαμε ότι οι αλλαγές που δοκιμάσαμε στους παράγοντες δεν επηρεάζουν αρκετά το **CPI**.Η καλύτερη τιμή που πήραμε ήταν το 2.58, ενώ στις default τιμές είχαμε 2.62.Ωστόσο φαίνεται πως το μέγεθος της **l2** είναι αυτό που το επηρεάζει περισσότερο.Συγκεκριμένα μειώνοντάς το έχουμε και μικρότερο/καλύτερο cpi.  
Tα στοιχεία του υποσυστήματος μνήμης για τη συγκεκριμένη τιμή του cpi είχαν τις παρακάτω τιμές:  
* L1 instruction cache size = 16 κΒ
* L1 instruction cache associativity = 8
* L1 data cache size = 32 κΒ
* L1 data cache associativity = 8
* L2 cache size = 64 kΒ
* L2 cache associativity = 8
* cache line size = 64 κΒ

----INSERT RESULTS.txt IMAGE----
~~~
Benchmarks	system.cpu.cpi	system.cpu.dcache.overall_miss_rate::total	system.cpu.icache.overall_miss_rate::total	system.l2.overall_miss_rate::total
speclibm_16_2_64_4_2_4_64	2.629421	0.060971	0.000113	0.999867
speclibm_16_2_64_4_2_8_64	2.629421	0.060971	0.000113	0.999867
speclibm_16_2_64_4_4_4_64	2.621112	0.060971	0.000113	0.999867
speclibm_16_2_64_4_4_8_64	2.620773	0.060971	0.000113	0.999867
speclibm_16_2_64_4_64_4_64	2.587098	0.060971	0.000113	0.999871
speclibm_16_2_64_4_64_8_64	2.587098	0.060971	0.000113	0.999867
speclibm_16_4_32_8_64_8_64	2.581159	0.060971	0.000103	0.999906
speclibm_16_8_16_8_16_8_64	2.586081	0.060971	0.000095	0.999952
specblibm_16_8_32_8_4_8_64	2.620800	0.060971	0.000095	0.999940
speclibm_16_8_32_8_64_8_32	3.922854	0.121940	0.000084	0.999985
speclibm_16_8_32_8_64_8_64	2.581157	0.060971	0.000095	0.999940
speclibm_16_8_64_8_64_8_64	2.586580	0.060971	0.000095	0.999940
speclibm_16_8_128_8_64_8_64	2.586529	0.060971	0.000095	0.999940
specblibm_32_8_128_8_4_8_64	2.620761	0.060971	0.000085	0.999979
speclibm_32_8_64_8_2_8_64	2.623265	0.060971	0.000085	0.999979
speclibm_32_8_64_8_32_8_64	2.584223	0.060971	0.000085	0.999979
speclibm_32_8_128_8_32_8_64	2.585101	0.060971	0.000085	0.999979
speclibm_32_8_128_8_64_8_64	2.587558	0.060971	0.000085	0.999979
speclibm_64_8_128_8_64_8_64	2.587558	0.060971	0.000084	0.999982
speclibm_128_8_128_8_64_8_64	2.587558	0.060971	0.000084	0.999982
speclibm_128_8_128_8_4_8_64	2.620761	0.060971	0.000084	0.999982
~~~

Εδώ βλέπουμε τα αποτελέσματα όλων των δοκιμών, όπου φαίνεται καλύτερα ότι οι διαφορές είναι πολύ μικρές σε κάθε αλλαγή, καθώς επίσης και τα αντίστοιχα γραφήματα.


![speclibm_l2_cpi]()
![speclibm_l2_size_l2]()
![speclibm_ld_size_dcache]()
![speclibm_li_size_icache]()
-----INSERT GRAPH----

Στην περίπτωση του benchmark **specmcf** τα αποτελέσματα είναι από την αρχή πολύ καλά, δηλαδή το **cpi** είναι χαμηλό στο 1.27 και με τις default τιμές.Και σε αυτήν την περίπτωση τα μεγέθη των l1 μένουν χαμηλά και δεν επηρεάζουν σε μεγάλο βαθμό την απόδοση.Και εδώ ξανά το l2 έχει την μεγαλύτερη επιρροή.Με τις ακόλουθες τιμές πετύχαμε το βέλτιστο cpi ίσο με 1.137.
* L1 instruction cache size = 16 κΒ
* L1 instruction cache associativity = 8
* L1 data cache size = 128 κΒ
* L1 data cache associativity = 8
* L2 cache size = 4 ΜΒ
* L2 cache associativity = 8
* cache line size = 64 κΒ

Εδώ βλέπουμε ότι θέλουμε και σχετικά μεγάλη τιμή **L1 data cache**.

--------INSERT RESULTS.TXT------
~~~
Benchmarks	system.cpu.cpi	system.cpu.dcache.overall_miss_rate::total	system.cpu.icache.overall_miss_rate::total	system.l2.overall_miss_rate::total
specmcf_16_4_32_8_64_8_64	1.147841	0.002345	0.000019	0.869273
specmcf_16_8_128_8_128_8_64	1.143583	0.001867	0.000019	0.988037
specmcf_16_8_128_8_64_8_64	1.143689	0.001867	0.000019	0.990021
specmcf_16_8_128_8_2_8_64	1.137876	0.001867	0.000019	0.798806
specmcf_16_8_128_8_4_8_64	1.137717	0.001867	0.000019	0.792734
specmcf_32_8_128_8_2_8_64	1.137872	0.001867	0.000018	0.799366
specmcf_32_8_128_8_4_8_64	1.137717	0.001867	0.000018	0.793251
specmcf_64_8_128_8_4_8_64	1.137717	0.001867	0.000018	0.793378
~~~


![specmcf_l2_cpi]()
![specmcf_l2_size_l2]()
![specmcf_ld_size_dcache]()
![specmcf_li_size_icache]()
-----INSERT GRAPH----

Για το benchmark **spechmmer** μπορούμε να δούμε ότι το cpi δεν μειώνεται αρκετά, οι τιμές του είναι 1.183 με 1.187.Ωστόσο βλέπουμε αισθητές διαφορές στο **l2 total miss rate** με μέγιστη τιμή το 0.547 και ελάχιστη το 0.008, τα οποία φαίνονται και παρακάτω.Το μικρότερο cpi (το οποίο σε αυτήν την περίπτωση ήταν σχεδόν ίδιο παντού) το είχαμε με τις τιμές που ακολουθούν:
* L1 instruction cache size = 64 κΒ
* L1 instruction cache associativity = 8
* L1 data cache size = 128 κΒ
* L1 data cache associativity = 4
* L2 cache size = 4 ΜΒ
* L2 cache associativity = 8
* cache line size = 64 κΒ

--------INSERT RESULTS.TXT------



![spechmmer_l2_cpi]()
![spechmmer_l2_size_l2]()
![spechmmer_ld_size_dcache]()
![spechmmer_li_size_icache]()
-----INSERT GRAPH----

Τέλος, για το benchmark **specsjeng** είχαμε από την αρχή πολύ υψηλό **cpi** και παρόλες τις αλλαγές δεν καταφέραμε να δούμε κάποια σημαντική αλλαγή και ήταν στα 7.04.Παρατηρήσαμε ότι μια μείωση του **l2 cache size** αύξησε λίγο το ήδη υψηλό cpi.Σε όλες τις περιπτώσεις το **l2 total miss rate** ήταν πάρα πολύ υψηλό στο 0.9999 και δεν είδαμε κάποια σημαντική πτώση.Τα υπόλοιπα **miss rate** έμειναν και αυτά σταθερά σε όλες τις δοκιμές, αλλά ήταν σε χαμηλά επίπεδα.Ένα σέτ τιμών ήταν:
* L1 instruction cache size = 32 κΒ
* L1 instruction cache associativity = 8
* L1 data cache size = 128 κΒ
* L1 data cache associativity = 8
* L2 cache size = 4 ΜΒ
* L2 cache associativity = 8
* cache line size = 64 κΒ



--------INSERT RESULTS.TXT------
~~~
Benchmarks	system.cpu.cpi	system.cpu.dcache.overall_miss_rate::total	system.cpu.icache.overall_miss_rate::total	system.l2.overall_miss_rate::total
specsjeng_16_4_32_8_4_8_64	7.039199	0.121831	0.000022	0.999968
specsjeng_16_8_32_4_4_8_64	7.039186	0.121831	0.000021	0.999969
specsjeng_16_8_32_8_4_8_64	7.039228	0.121831	0.000021	0.999970
specsjeng_16_8_32_8_2_8_64	7.040565	0.121831	0.000021	0.999970
specsjeng_16_8_32_8_64_8_64	7.099360	0.121831	0.000021	0.999983
specsjeng_16_8_128_8_4_8_64	7.039264	0.121830	0.000021	0.999974
specsjeng_32_4_128_4_4_8_64	7.039133	0.121831	0.000020	0.999983
specsjeng_32_4_128_8_4_8_64	7.039143	0.121831	0.000020	0.999983
specsjeng_32_8_128_8_2_8_64	7.040594	0.121831	0.000019	0.999986
specsjeng_32_8_128_8_4_8_64	7.039166	0.121831	0.000019	0.999986
specsjeng_64_8_128_8_4_8_64	7.042045	0.121830	0.000019	0.999986
specsjeng_32_8_64_8_4_8_64	7.039165	0.121831	0.000019	0.999984
~~~


![specsjeng_l2_cpi]()
![specsjeng_l2_size_l2]()
![specsjeng_ld_size_dcache]()
![specsjeng_li_size_icache]()
-----INSERT GRAPH----





### **Βήμα 3**

Όπως γνωρίζουμε στη σχεδίαση ψηφιακών κυκλωμάτων το μέγεθος καθορίζει αποτελεσματικά το κόστος κατασκευής . Επίσης σε έναν μικρότερο βαθμό το κόστος επηρεάζεται από την πολυπλοκότητα του κυκλώματος. Πέρα από αυτούς τους δύο παράγοντες πολύ μεγάλο ρόλο παίζει το γεγονός πως επιθυμούμε η **l1_cache** να είναι μικρού σχετικά μεγέθους έτσι ώστε να πετυχαίνουμε υψηλές ταχύτητες , ενώ συνήθως χρησιμοποιείται **l2_cache** με μεγαλύτερο μέγεθος απ'την  **l1_cache**. Για τους παραπάνω λόγους , κατά την κατασκευή της συνάρτησης κόστους για την επιλογή των κατάλληλων μεγεθών επιλέγονται οι παρακάτω συντελεστές βαρών οι οποίοι πολλαπλασιάζονται με τις αντίστοιχες μεταβλητές για να καθοριστεί το συνολικό κόστος σχεδίασης :

      Cost (lis , lds , l2s, lia , lda ,l2a) = 10 * (lis + lds) + 5 * l2s + 4 * (lia + lda) + 2 * l2a
Όπου **lis** = li_size , **lds** = ld_size , **l2s**=l2_size, **lia** = li_assoc , **lda** = ld_assoc , l2a = l2_assoc. Ουσιαστικά θεωρούμε πως η αύξηση του μεγέθους της μνήμης **l1_cache** (**lis** , **lds** ) αλλά και της πολυπλοκότητας της (**lia** , **lda**) έχει την διπλάσια βαρύτητα απο τις αντίστοιχες μεταβλητές της **l2_cache**(**l2s** ,**l2a**). θέλουμε όταν διπλασιάζεται το μέγεθος και των δύο επιπέδων cache να διπλασιάζεται "σχεδόν" και το συνολικό κόστος . Επίσης θεωρούμε πως το συνολικό μέγεθος έχει αρκετά μεγαλύτερη σημασία απο την αντίστοιχη πολυπλοκότητα της μνήμης . 
