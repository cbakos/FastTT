# FastTT
This package performs a faster tensor train (TT) decomposition for large sparse data.
It can be several times to several tens time faster than the widely-used TT-SVD algorithm while keeping same accuracy. The speedup ratio depends on the sparsity of the data.

If you used this package, please cite the following paper:

[1] Lingjie Li, Wenjian Yu, and Kim Batselier, "Faster tensor train decomposition for sparse data," arXiv#1908.02721

## Prerequisites

- [g++](https://gcc.gnu.org/) version >= 7 or latest Clang
- [Xerus](https://www.libxerus.org/)
- [Boost](https://www.boost.org/)
- [cxxopts](https://github.com/jarro2783/cxxopts)

## Usage

```
TTTensor sptensor2tt(Tensor b, int vpos, int max_rank, double eps);
```

Transform tensor `b` into TT-tensor.

`vpos`: Parameter p in FastTT.

`max_rank`: The max rank of the result TT-tensor. Priority over `eps`.

`eps`: Max tolerated relative error.


## Test

```
❯ ./test -h
Test fast T2TT.
Usage:
  test [OPTION...]

  -f, --file arg        Input file name
  -t, --type arg        Input file type: graph / image / tensor (default:
                        unspecific)
  -U, --undirected      If input graph is undirected
  -O, --obeserved arg   The obeservation ratio of the image (default: 0.01)
  -R, --random          Use random generated n^d tesors as input instead
  -n, arg               Parameter n of the tensor (default: 4)
  -d, arg               Parameter d of the tensor (default: 10)
  -l, --n_list arg      Use a list of n instead of n^d
  -N, --nnz arg         The number of nonzero elements of the random
                        generated tensor (default: 500)
  -F, --fixed_rank arg  Generate fixed-rank tesors (default: 0)
  -s, --sparsity arg    The sparsity of generated cores (default: 0.02)
  -p, arg               Parameter p of FastTT (default: -1)
  -r, --max_rank arg    Max ranks of the target tensor train (default: 0)
  -e, --epsilon arg     Desired tolerated relative error (default: 1e-14)
      --ttsvd           Test TT-SVD
      --rttsvd arg      Test Randomized TT-SVD for given target rank
                        (default: 10)
      --nofasttt        Do not test FastTT
  -S, --simple          Output simple result
      --save arg        Save the tensor as a tsv file (default: backup.tsv)
```

### Examples

A random 100^3 tensor with nnz=500:
```
❯ ./test -R -n 100 -d 3 -N 500 -p 1
```

A random 20^5 tensor with TT-rank=20 and sparsity=0.1:
```
❯ ./test -R -n 20 -d 5 -F 20 -s 0.1 -p 2 -e 0.1
```

An image as a 10x20x20x10x15x20x3 tensor:
```
❯ ./test -f dolphin-4000x3000.txt -t image -O 0.001 -l 10,20,20,10,15,20,3 -r 100 -p 6
```

An FDM problem as a 1600^3 tensor:
```
❯ ./test -f fdm.tsv -t tensor -n 1600 -d 3 -p 1
```

An undirected graph as a 10^5 tensor:
```
❯ ./test -f roadNet-PA.txt -t graph -U -n 10 -d 5 -p 3 -e 1e-2
```

### Data

- [dolphin-4000x3000.txt](https://drive.google.com/open?id=1RvhZmhm7LBVl5tC1iGRj1u5b3ZgBXTNa)

- [video_360x640x144x3.txt](https://drive.google.com/open?id=1cXxqoHhXG3CEBgnzlCXpytHYY-V2XUJ7)

- fdm.tsv: generated by `fdm.py`

- [roadNet-PA](https://snap.stanford.edu/data/roadNet-PA.html) from [SNAP](https://snap.stanford.edu/data/index.html)
