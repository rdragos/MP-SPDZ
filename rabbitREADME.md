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



# How to run the experiments

* Passive ring HM
  * `./compile.py -R 64 bench_rabbit_ring 32 1048000 1` and `./compile.py -R 64 mcomp 2 32 1048000 1`
  * `./Scripts/ring.sh bench_rabbit_ring-32-1048000-1` and `./Scripts/ring.sh mcomp-2-32-1048000-1`
* Passive field HM
  * `./compile.py -p 64 bench_rabbit_field 32 1048000 1` and `./compile.py mcomp 32 1048000 1`
  * `./Scripts/rep-field.sh bench_rabbit_field-32-1048000-1 -P 18446744073708797953` and `./Scripts/rep-field.sh mcomp-32-1048000-1 -lgp 128`

* When running the rabbit field we need to set MOD = -DGFP_MOD_SZ=1 compilation flag in CONFIG.mine

# List of protocols ran

* Field case: with the additional parameter -32-1048000-1 -P 18446744073708797953

| Executable | Local running script |
| --- | --- |
|`soho-party.x` | `./Scripts/soho.sh` (DM passive HE)|
|`semi-party.x` | `./Scripts/semi.sh` (DM passive OT)|
|`replicated-field-party.x` | `rep-field.sh` (HM passive)|
|`malicious-rep-field-party.x` | `./Scripts/mal-rep-field.sh` (HM active)|
|`mascot-party.x` | `./Scripts/mascot.sh` (DM active OT)|
|`cowgear-party.x` | `./Scripts/cowgear.sh` (DM active HE)|

* For the ring case make sure to compile with -R 64

| Executable | Local running script |
| --- | --- |
|`replicated-ring-party.x` | `./Scripts/ring.sh`|
|`semi2k-party.x` | `./Scripts/semi2k.sh`|
|`malicious-rep-ring-party.x` | `./Scripts/mal-rep-ring.sh`|
|`spdz2k-party.x` | `./Scripts/spdz2k.sh`|
