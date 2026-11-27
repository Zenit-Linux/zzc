# zzc — frontend gcc

`zzc [opcje gcc] prog.zs [-o wyjście]` — Z# → IR → C → **gcc**. Pełny język: klasy, domknięcia, wyjątki, `import` modułów.

```
zzc prog.zs -o prog            # -O2 domyślnie
zzc -O3 -flto -g prog.zs
zzc -c prog.zs -o prog.o       # tylko obiekt;  -S → asembler
zzc run prog.zs -- argumenty   # zbuduj i uruchom
zzc --dry-run --print-cmd ...  # pokaż wywołanie gcc
zzc --gcc aarch64-linux-gnu-gcc prog.zs   # kompilator skrośny (niezweryfikowane)
```

Obiekt runtime (`zs_rt.c`) jest kompilowany **raz na zestaw flag kompilacji** i trzymany w `~/.cache/zzc` (`ZZC_CACHE`); `--no-cache` to wyłącza.
Flagi `-O* -f* -m* -g* -D* -std*` wpływają na klucz cache, pozostałe (`-l`, `-L`, `-Wl,`) idą tylko do linkowania.
To sterownik gcc (nie wtyczka `cc1`). Jeden plik wejściowy na wywołanie; pozostałe moduły przez `import`.
Kody wyjścia: 0 ok · 1 błąd (składnia, moduły, gcc) · 2 użycie. Testy: `tests/run.sh`.
