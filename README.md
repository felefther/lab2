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

![sim_sec](https://github.com/felefther/lab2/blob/master/sim_sec.png)
![cpi_graph](https://github.com/felefther/lab2/blob/master/cpi.png)
![dcache](https://github.com/felefther/lab2/blob/master/dcache.png)
![icache](https://github.com/felefther/lab2/blob/master/icache.png)
![l2](https://github.com/felefther/lab2/blob/master/l2.png)


Από τα αποτελέσματα παρατηρούμε καταρχάς ότι σε όλα τα benchmarks το cpi είναι δεκαπλάσιο του χρόνου εκτέλεσης, το οποίο ισχύει καθώς **ο χρόνος εκτέλεσης = cpi * αριθμό εντολών / συχνότητα**.Ακόμα βλέπουμε ότι στα benchmarks με υψηλό cpi έχουμε και μεγαλύτερο total miss rate στην dcache και στην l2.Αντίθετα υψηλό total miss rate στην icache έχουμε μόνο στο specmcf.


**3.**  
Τρέχοντας ξανά όλα τα benchmarks με την παράμετρο --cpu-clock=2GHz παρατηρούμε τα εξής:  
Η παράμετρος **system.clk_domain.clock** παραμένει στα 1000 ticks, δηλαδή το system clock παρέμεινε στα 1GHz, αναμενόμενο καθώς δεν ζητήσαμε να αλλάξει.Ωστόσο το **system.cpu_clk_domain.clock** άλλαξε όντως στα 500 ticks, δηλαδη τo cpu clock έγινε 2 Ghz.Όλα τα στοιχεία της μητρικής πλακέτας χρονίζονται στα 1GHz,ενώ η cpu στα 2GHz.Σε όλα τα benchmarks βλέπουμε βελτίωση της απόδοσης, ο χρόνος εκτέλεσης μειώνεται στο μισό.Το ρολόι της cpu θέλουμε σε γενικές γραμμλες να είναι μεγαλύτερο απ της μητρικής καθώς πρέπει να εκτελεί περισσότερες λειτουργίες.





























