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
Εκτελώντας τις προσομοιώσεις για τα 5 benchmarks παίρνουμε τα παρακάτω αποτελέσματα, τα οποία φαίνονται στα αρχεία **stats.txt**:

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


Από τα αποτελέσματα παρατηρούμε καταρχάς ότι σε όλα τα benchmarks το cpi είναι δεκαπλάσιο του χρόνου εκτέλεσης, το οποίο ισχύει καθώς **ο χρόνος εκτέλεσης = cpi * αριθμό εντολών / συχνότητα**.Ακόμα βλέπουμε ότι στα benchmarks με υψηλό cpi έχουμε και μεγαλύτερο total miss rate στην dcache και στην l2.Αντίθετα υψηλό total miss rate στην icache έχουμε μόνο στο specmcf.


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


Τα αποτελέσματα που παίρνουμε είναι:
* system.cpu.cpi = 1.552991
* system.cpu.icache.overall_miss_rate::total = 0.000070
* system.cpu.dcache.overall_miss_rate::total = 0.010150
* system.l2.overall_miss_rate::total = 0.376078

Βλέπουμε ότι παρόλο που το cpi μειώθηκε έχουμε αύξηση του l2 miss rate.Οι αλλαγές στα miss rate των icache, dcache είναι αρκετά μικρές.Συμπεραίνουμε ότι στο συγκεκριμένο benchmark χρειάζεται μεγάλη l2, τουλάχιστον 2 MB όπως φαίνεται και στα αποτελέσματα των  δοκιμών.  

----INSERT RESULTS.txt IMAGE----

Στα γραφήματα που ακολουθούν βλέπουμε και το συσχετισμό μεταξύ l2 cache size - cpi.Επιλέξαμε τον συγκεκριμένο παράγοντα γιατί παρατηρήσαμε ότι η δική του αυξομείωση επηρεάζει περισσότερο το cpi.Οι υπόλοιπες μεταβλητές είναι σταθερές με τις τιμές που αναφέραμε παραπάνω.



-----INSERT GRAPH----


Για το benchmark **specblibm** παρατηρήσαμε ότι οι αλλαγές που δοκιμάσαμε στους παράγοντες δεν επηρεάζουν αρκετά το **CPI**.Η καλύτερη τιμή που πήραμε ήταν το 2.58, ενώ στις default τιμές είχαμε 2.62.Ωστόσο φαίνεται πως το μέγεθος της **l2** είναι αυτό που το επηρεάζει περισσότερο.Συγκεκριμένα μειώνοντάς το έχουμε και μικρότερο/καλύτερο cpi.  
Tα στοιχεία του υποσυστήματος μνήμης είχαν τις παρακάτω τιμές:  
* L1 instruction cache size = 16 κΒ
* L1 instruction cache associativity = 8
* L1 data cache size = 32 κΒ
* L1 data cache associativity = 8
* L2 cache size = 64 kΒ
* L2 cache associativity = 8
* cache line size = 64 κΒ

----INSERT RESULTS.txt IMAGE----

Εδώ βλέπουμε τα αποτελέσματα όλων των δοκιμών, όπου φαίνεται καλύτερα ότι οι διαφορές είναι πολύ μικρές σε κάθε αλλαγή.

Διατηρώντας σταθερούς όλους τους παράγοντες εκτός του l2 cache size παίρνουμε το παρακάτω γράφημα:
Οι τιμές τους είναι:  
* L1 instruction cache size = 32 κΒ
* L1 instruction cache associativity = 8
* L1 data cache size = 64 κΒ
* L1 data cache associativity = 8
* L2 cache associativity = 8
* cache line size = 64 κΒ

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

Ξανά το γράφημα που παρουσιάζουμε είναι μεταξύ των  **l2 cache size** και **cpi**.

-----INSERT GRAPH----















### **Βήμα 3**

Όπως γνωρίζουμε στη σχεδίαση ψηφιακών κυκλωμάτων το μέγεθος καθορίζει αποτελεσματικά το κόστος κατασκευής . Επίσης σε έναν μικρότερο βαθμό το κόστος επηρεάζεται από την πολυπλοκότητα του κυκλώματος. Πέρα από αυτούς τους δύο παράγοντες πολύ μεγάλο ρόλο παίζει το γεγονός πως επιθυμούμε η **l1_cache** να είναι μικρού σχετικά μεγέθους έτσι ώστε να πετυχαίνουμε υψηλές ταχύτητες , ενώ συνήθως χρησιμοποιείται **l2_cache** με μεγαλύτερο μέγεθος απ'την  **l1_cache**. Για τους παραπάνω λόγους , κατά την κατασκευή της συνάρτησης κόστους για την επιλογή των κατάλληλων μεγεθών επιλέγονται οι παρακάτω συντελεστές βαρών οι οποίοι πολλαπλασιάζονται με τις αντίστοιχες μεταβλητές για να καθοριστεί το συνολικό κόστος σχεδίασης :

      Cost (lis , lds , l2s, lia , lda ,l2a) = 10 * (lis + lds) + 5 * l2s + 4 * (lia + lda) + 2 * l2a
Όπου **lis** = li_size , **lds** = ld_size , **l2s**=l2_size, **lia** = li_assoc , **lda** = ld_assoc , l2a = l2_assoc. Ουσιαστικά θεωρούμε πως η αύξηση του μεγέθους της μνήμης **l1_cache** (**lis** , **lds** ) αλλά και της πολυπλοκότητας της (**lia** , **lda**) έχει την διπλάσια βαρύτητα απο τις αντίστοιχες μεταβλητές της **l2_cache**(**l2s** ,**l2a**). θέλουμε όταν διπλασιάζεται το μέγεθος και των δύο επιπέδων cache να διπλασιάζεται "σχεδόν" και το συνολικό κόστος . Επίσης θεωρούμε πως το συνολικό μέγεθος έχει αρκετά μεγαλύτερη σημασία απο την αντίστοιχη πολυπλοκότητα της μνήμης . 
