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
