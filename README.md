#Αρχιτεκτονική Υπολογιστών 7ο Εξάμηνο

##Κυπαρίσσης Οδυσσέας ΑΕΜ : 8955

##Φατόλας Ελευθέριος ΑΕΜ : 8958

##Ομάδα 24

##Αριστοτέλειο Πανεπιστήμιο Θεσσαλονίκης

##Τμήμα Ηλεκτρολόγων Μηχανικών και Μηχανικών Υπολογιστών



##2η Εργαστηριακή Άσκηση



###**Βήμα 1**

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

 # Cache Options
    parser.add_option("--l1d_size", type="string", default="64kB")
    parser.add_option("--l1i_size", type="string", default="32kB")
    parser.add_option("--l2_size", type="string", default="2MB")
    parser.add_option("--l1d_assoc", type="int", default=2)
    parser.add_option("--l1i_assoc", type="int", default=2)
    parser.add_option("--l2_assoc", type="int", default=8)
    parser.add_option("--cacheline_size", type="int", default=64)


**2.**
Εκτελώντας τις προσομοιώσεις για τα 5 benchmarks παίρνουμε τα παρακάτω αποτελέσματα, τα οποία φαίνονται στα αρχεία **stats.txt**:

Για το **specbzip** έχουμε:

sim_seconds                                  0.161025                       # Number of seconds simulated
system.cpu.cpi                               1.610247                       # CPI: cycles per instruction
system.cpu.icache.overall_miss_rate::total     0.000077                       # miss rate for overall accesses
system.cpu.dcache.overall_miss_rate::total     0.014675                       # miss rate for overall accesses
system.l2.overall_miss_rate::total           0.282157                       # miss rate for overall accesses


Για το **spechmmer** έχουμε:

sim_seconds                                  0.11853                       # Number of seconds simulated
system.cpu.cpi                               1.185304                       # CPI: cycles per instruction
system.cpu.icache.overall_miss_rate::total     0.000221                       # miss rate for overall accesses
system.cpu.dcache.overall_miss_rate::total     0.001629                       # miss rate for overall accesses
system.l2.overall_miss_rate::total           0.077747                       # miss rate for overall accesses


Για το **speclibm** έχουμε:

sim_seconds                                  0.262327                       # Number of seconds simulated
system.cpu.cpi                               2.623265                       # CPI: cycles per instruction
system.cpu.icache.overall_miss_rate::total     0.000094                       # miss rate for overall accesses
system.cpu.dcache.overall_miss_rate::total     0.060971                       # miss rate for overall accesses
system.l2.overall_miss_rate::total           0.999944                       # miss rate for overall accesses


Για το **specmcf** έχουμε:

sim_seconds                                  0.127942                       # Number of seconds simulated
system.cpu.cpi                               1.279422                       # CPI: cycles per instruction
system.cpu.icache.overall_miss_rate::total     0.023627                       # miss rate for overall accesses
system.cpu.dcache.overall_miss_rate::total     0.002108                       # miss rate for overall accesses
system.l2.overall_miss_rate::total           0.055046                       # miss rate for overall accesses

Για το **specsjeng** έχουμε:

sim_seconds                                  0.704056                       # Number of seconds simulated
system.cpu.cpi                               7.040561                       # CPI: cycles per instruction
system.cpu.icache.overall_miss_rate::total     0.000020                       # miss rate for overall accesses
system.cpu.dcache.overall_miss_rate::total     0.121831                       # miss rate for overall accesses
system.l2.overall_miss_rate::total           0.99972                       # miss rate for overall accesses


---GRAPHS---

Από τα αποτελέσματα παρατηρούμε καταρχάς ότι σε όλα τα benchmarks το cpi είναι δεκαπλάσιο του χρόνου εκτέλεσης, το οποίο ισχύει καθώς **ο χρόνος εκτέλεσης = cpi * αριθμό εντολών / συχνότητα**.Ακόμα βλέπουμε ότι στα benchmarks με υψηλό cpi έχουμε και μεγαλύτερο total miss rate στην dcache και στην l2.Αντίθετα υψηλό total miss rate στην icache έχουμε μόνο στο specmcf.


**3.**
Τρέχοντας ξανά όλα τα benchmarks με την παράμετρο --cpu-clock=2GHz παρατηρούμε ότι ο χρόνος εκτέλεσης πέφτει στο μισό.Το system clock παρέμεινε στα 1GHz



----The system clock is needed to synchronize all components on the motherboard, which means they all do their work only if the clock is high; never when it's low. And because the clock speed is set above the longest time any signal needs to propagate through any circuit on the board, this system is preventing signals from arriving before other signals are ready and thus keeps everything safe and synchronized. The CPU clock has the same purpose, but is only used on the chip itself. Because the CPU needs to perform more operations per time than the motherboard, the CPU clock is much higher. And because we don't want to have another oscillator (e.g. because they also would need to be synchronized), the CPU just takes the system clock and multiplies it by a number, which is either fixed or unlocked (in that case the user can change the multiplier in order to over- or underclock the CPU). -----
























