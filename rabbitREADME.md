Parameters used for benching:

* `mcomp-2-32-1048000-1`

Parameters that were used to bench rabbit:`
* `bench_rabbit_field-16-1048000-1`
* `bench_rabbit_ring-16-1048000-1`

These are launched using `./compile.py`, for eg run `./compile.py mcomp 2 32 1048000 1` and it will run
1048000 comparisons using edabits, 32 threads where each thread is executing 32750 comparisons in parallel using SIMD packing.
Similar with `bench_rabbit_field` and `bench_rabbit_ring`.


./compile.py -R 64 bench_rabbit_ring 32 1048000 1
./Scripts/ring.sh bench_rabbit_ring-32-1048000-1



# Notes on Rabbit debud for Ring

* A large part of the extra bit-triples come from the carry_babbit not having the compute_p optimization (it is still unclear what this optimization is and if it affects out correctness). 
* We are still not there and the other reason is two additional bit-triples per comparison. These two come from the `if a is None:` and `if b is None:` optimizations (in the first loop, compute_p is true but a happens to be None)   
* Surprisingly, the first fix itself does get out communication from 81MB to about 80 MB and not to the expected 71 MB. The reason for this is a bit myseterious but it seems like it is in the details of the outer protocol. Maybe the reveal a part of things.
* Found it, spurious `clear_a = a.reveal()` that gets us down to about 72 MB.
* Final bit, how to get the None optimization, can't do it without Dragos

* Other todos: are the other results making sense (HE/OT and Field), why do we have low round complexity, WAN experiments


# How to run the experiments

* Passive ring HM
  * `./compile.py -R 64 bench_rabbit_ring 32 1048000 1` and `./compile.py -R 64 mcomp 2 32 1048000 1`
  * `./Scripts/ring.sh bench_rabbit_ring-32-1048000-1` and `./Scripts/ring.sh mcomp-2-32-1048000-1`
* Passive field HM
  * `./compile.py -p 64 bench_rabbit_field 32 1048000 1` and `./compile.py mcomp 32 1048000 1`
  * `./Scripts/rep-field.sh bench_rabbit_field-32-1048000-1 -P 18446744073708797953` and `./Scripts/rep-field.sh mcomp-32-1048000-1 -lgp 128`


# List of protocols ran

* Field case: with the additional parameter -32-1048000-1 -P 18446744073708797953

  soho-party.x | soho.sh
  semi-party.x | semi.sh
  mascot-party.x | mascot.sh
  cowgear-party.x | cowgear.sh

* For the ring case make sure to compile with -R 64
  replicated-ring-party.x | ring.sh
  semi2k-party.x | semi2k.sh
  malicious-rep-ring-party.x | mal-rep-ring.sh
  spdz2k-party.x | spdz2k.sh
