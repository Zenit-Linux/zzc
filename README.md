# zzc — frontend Z# dla GCC (0.0.1)

`zzc` to sterownik w stylu `gcc` dla Z#: parsuje i analizuje źródło (rdzeń `Z-Sharp`), obniża je do Z# IR i C (`emit_c`)
i oddaje kompilację do **GCC** razem z natywnym runtime (`Z-Sharp/runtime/native/zs_rt.c`). Opcje, których `zzc` nie zna,
trafiają do gcc bez zmian (`-O3 -g -flto -march=native -fsanitize=address -static -Wl,...`).

```
zzc prog.zs -o prog            # binarka (gcc -O2)
zzc -O3 -flto prog.zs -o prog
zzc -S prog.zs                 # asembler z gcc
zzc -c prog.zs                 # obiekt; runtime: zzc --runtime-obj zs_rt.o
zzc --emit-c prog.zs           # tylko C
zzc --gcc aarch64-linux-gnu-gcc prog.zs -o prog.arm64   # cross-kompilacja
```

## Stan (uczciwie)
* Działa: podzbiór natywny (funkcje, tablice, napisy, int/float/bool, pętle, switch, try/catch/finally) — ten sam co `zsc`, ta sama semantyka (testy różnicowe w `zsc/tests/native`).
* To frontend **przez C**, a nie wtyczka `cc1` z drzewami GENERIC/GIMPLE. Prawdziwy frontend GCC (język w drzewie źródeł GCC, `lang.opt`, `z#-lang.c`) jest celem długoterminowym; ten sterownik daje już pełne optymalizacje gcc (LTO, PGO, sanitizery, cross).
* Brak: klas, domknięć, modułów Z# w kodzie natywnym (jawny błąd z linią) — pełny język: `zsi`.
* Jeden plik źródłowy na wywołanie.
