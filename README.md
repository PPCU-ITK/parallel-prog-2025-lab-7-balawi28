[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/2CuuzvUr)

# Assignment 7
To better measure the speedup gained by accelerating using GPU, I modified `Nx` and `Ny` to equal `1000` instead of their initial values, and then compiled and ran the normal `cfd_euler.cpp` file:

`module load nvhpc && module load craype-accel-nvidia80`

`nvc++ -mp=gpu -gpu=cc80 -Ofast cfd_euler_normal.cpp -o cfd_euler_normal -Minfo=accel,mp`

`srun -p gpu --gres=gpu:1 --ntasks=1 --time=00:05:00 --mem=40G ./cfd_euler_normal`

the output is:
```
Step 0 completed, total kinetic energy: 492024
... 
(many steps, removed for better documentation readability)
... 
Step 1950 completed, total kinetic energy: 470917
Time taken: 18187 milliseconds
```

then ran the parallel version `cfd_euler.cpp`:

`module load nvhpc && module load craype-accel-nvidia80`

`nvc++ -mp=gpu -gpu=cc80 -Ofast cfd_euler.cpp -o cfd_euler -Minfo=accel,mp`

`srun -p gpu --gres=gpu:1 --ntasks=1 --time=00:05:00 --mem=40G ./cfd_euler`

the output is:

```
Step 0 completed, total kinetic energy: 492024
... 
(many steps, removed for better documentation readability)
... 
Step 1950 completed, total kinetic energy: 470917
Time taken: 747 milliseconds
```

This is a significant speedup:

speedup = 18187 / 747

speedup = 24.35 !!