# kubectl speedup with persistent proxies

## Installation

Put the [kubectl-speedup](./kubectl-speedup) script somewhere on your PATH, eg.:

```bash
curl -O /usr/local/bin/kubectl-speedup https://github.com/reegnz/kubectl-speedup/master/kubectl-speedup
```

Then make sure you wrap it with a shell function, like so (in your `.bashrc` or `.zshrc`):

```bash
function kubectl() {
  kubectl-speedup "$@"
}
```

Initial benchmarks with hyperfine show a 1.6x speedup when working with an
AWS-EKS cluster that's in West-coast US and is accessed from Europe.

```
❯ hyperfine 'kubectl get pods' 'kubectl-speedup get pods'
Benchmark 1: kubectl get pods
  Time (mean ± σ):      3.937 s ±  0.090 s    [User: 0.767 s, System: 0.406 s]
  Range (min … max):    3.836 s …  4.108 s    10 runs

Benchmark 2: kubectl-speedup get pods
  Time (mean ± σ):      2.383 s ±  0.180 s    [User: 0.311 s, System: 0.141 s]
  Range (min … max):    2.237 s …  2.869 s    10 runs

Summary
  'kubectl-speedup get pods' ran
    1.65 ± 0.13 times faster than 'kubectl get pods'
```

## TODO

- Proxies are not cleaned up
- generated kubeconfig files are not cleaned up
