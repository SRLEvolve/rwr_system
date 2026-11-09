# rwr_system: notes for coding agents

## GPUs on the Brev box (srl-nvidia): use gpuq

Every GPU job on srl-nvidia goes through the shared queue **gpuq** (HyperQueue). Don't pick a GPU by hand or set
`CUDA_VISIBLE_DEVICES` yourself (decided 2026-11-09).

- Submit: `~/lschen_pomdar/gpuq/bin/gpuq submit --gpu 1 --name <name> --log <file> -- <command>`.
  - `--gpu 0.5` / `0.25` shares a GPU with other jobs. Nothing enforces the share, so size it honestly.
  - `--gpu 0` = CPU only; `--cpus N` books cores.
  - gpuq sets `CUDA_VISIBLE_DEVICES`; inside the job use the first visible device (`cuda:0`).
  - The job runs in the current directory. It gets `PATH`, the active venv/conda, `PYTHONPATH` and `LD_LIBRARY_PATH`
    from your shell; export anything else inside the command.
  - Jobs keep running after you log out. The log defaults to `~/lschen_pomdar/gpuq/logs/<job id>.log`.
- Priority, higher first: manual jobs `0` (the default) start before automated evolve jobs (`-10`). Running jobs are
  never pre-empted.
- Look and manage:
  - `gpuq ls` lists queued and running jobs.
  - `gpuq gpus` shows what runs on each GPU; `NOT VIA GPUQ` marks work started outside the queue.
  - `gpuq cancel <id>`, `gpuq help`.
- Never run `gpuq stop`: it kills everyone's running jobs.
