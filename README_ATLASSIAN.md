# Build and Test

start a container to do builds and tests in .
```
docker run -it --rm \
  -v /Users/sgratton/Dev/pgbackrest:/pgbackrest \
  -w /pgbackrest \
  pgbackrest/test:u22-base-aarch64-20260529A \
  bash
```

you need to install a few things before you start, so run.
```
apt-get update && apt-get install -y rsync
apt-get install -y uncrustify

```

You need to set up the environment to build pgbackrest
```
meson setup build
```

Build pgbackrest
```
ninja -C build
```

Run the code linter (from the root)
```
cd ..
pgbackrest/test/test.pl --gen-check --log-level-test-file=off --no-coverage-report --vm-max=2 --vm=none --code-format-check
```

run the storage tests (from the root)
```
cd ..
perl pgbackrest/test/test.pl --vm=none --no-coverage --no-valgrind --test-path=/tmp/pgbr-test --module=storage
```