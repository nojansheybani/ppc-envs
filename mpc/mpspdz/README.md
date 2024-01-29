# MP-SPDZ

In `Dockerfile`, change `ARG cryptoplayers=10` to `ARG cryptoplayers=[number of parties]`.

You can change the script you're running in the Dockerfile too.

Finally, run `docker build -t mpspdz .`.