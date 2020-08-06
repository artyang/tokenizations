# Robust and Fast tokenizations alignment library for Rust and Python
[![creates.io](https://img.shields.io/crates/v/tokenizations.svg)](https://crates.io/crates/tokenizations)
[![pypi](https://img.shields.io/pypi/v/pytokenizations.svg)](https://pypi.org/project/pytokenizations/)
[![Actions Status](https://github.com/tamuhey/tokenizations/workflows/Test/badge.svg)](https://github.com/tamuhey/tokenizations/actions)

![sample](./img/demo.png)

Demo: [demo](https://tamuhey.github.io/tokenizations/)  
Rust document: [docs.rs](https://docs.rs/tokenizations)  
Blog post: [How to calculate the alignment between BERT and spaCy tokens effectively and robustly](https://gist.github.com/tamuhey/af6cbb44a703423556c32798e1e1b704)

## Usage (Python)

- Installation

```bash
$ pip install pytokenizations
```

- Install from source

This library uses [maturin](https://github.com/PyO3/maturin) to build the wheel.

```console
$ git clone https://github.com/tamuhey/tokenizations
$ cd tokenizations/python
$ pip install maturin
$ maturin build
```

Now wheel is created in `python/target/wheels` directory, and you can install it with `pip install *whl`.

### `get_alignments`

```python
def get_alignments(a: Sequence[str], b: Sequence[str]) -> Tuple[List[List[int]], List[List[int]]]: ...
```

Returns alignment mappings for two different tokenizations:

```python
>>> tokens_a = ["å", "BC"]
>>> tokens_b = ["abc"] # the accent is dropped (å -> a) and the letters are lowercased(BC -> bc)
>>> a2b, b2a = tokenizations.get_alignments(tokens_a, tokens_b)
>>> print(a2b)
[[0], [0]]
>>> print(b2a)
[[0, 1]]
```

`a2b[i]` is a list representing the alignment from `tokens_a` to `tokens_b`.   

### `get_original_spans`

```python
def get_original_spans(tokens: Sequence[str], original_text: str) -> List[Optional[Tuple[int, int]]]: ... 
```

Returns the span indices in original_text from the tokens.
This is useful, for example, when a processed result is mapped to the original text that is not normalized yet.

```python
>>> tokens = ["a", "bc"]
>>> original_text = "å  BC"
>>> get_original_spans(tokens, original_text)
[(0,1), (3,5)]
```

### `get_charmap`

```python
def get_charmap(a: str, b: str) -> Tuple[List[Optional[int]], List[Optional[int]]]: ...
```

Returns character mappings `a2b` (from `a` to `b`) and `b2a` (from `b` to `a`).

```python
>>> a = "åBC"
>>> b = "abc"
>>> get_charmap(a, b)
([0,1,2], [0,1,2])
```

## Usage (Rust)

See here: [docs.rs](https://docs.rs/tokenizations)  

## Related

- [Algorithm overview](./note/algorithm.md)  
- [Blog post](./note/blog_post.md)  
- [seqdiff](https://github.com/tamuhey/seqdiff) is used for the diff process.
- [textspan](https://github.com/tamuhey/textspan)
