# ft_containers

`vector` and `map` from the C++ standard library, rewritten in C++98 for the École 42 ft_containers subject. I worked on it in the GDGoC Konkuk container study group from October 2025 to January 2026.

Everything is header-only, in namespace `ft`, under `include/ft/`.

### vector

Memory comes from `std::allocator`. Elements are built in place with `construct()` and torn down with `destroy()`, which keeps the object lifetimes separate from the raw storage. Copy assignment is copy-and-swap: the full copy is made first and then swapped in, so the original is untouched if copying throws.

The range constructor and `insert(first, last)` use `enable_if` and `is_integral` to keep a call like `vector(5, 10)` from being treated as an iterator pair, and they pick an algorithm from `iterator_traits<It>::iterator_category`. As in the standard library, `at()` throws `std::out_of_range` and `reserve()` throws `std::length_error`.

### map

A red-black tree with rebalancing after insert and after erase, using left and right rotations. It has bidirectional `iterator` and `const_iterator`, plus `find`, `operator[]`, `count`, `lower_bound`, `upper_bound`, `equal_range`, `swap`, `clear`, `key_comp`, `value_comp` and the comparison operators.

### Utilities

`pair`, `make_pair`, `equal`, `lexicographical_compare`, `enable_if`, `is_integral`, `iterator_traits` and `reverse_iterator`.

## Tests

The tests and the Makefile come from [cpp-containers-tester](https://github.com/Yonaim/cpp-containers-tester), written by another member of the study group. Each test is compiled once against `ft` and once against `std` and the results are compared. The test sources are not included here; copy the tester's `test/` folder next to `include/` to run them:

```bash
git clone https://github.com/Yonaim/cpp-containers-tester.git
cp -r cpp-containers-tester/test .
make core_utils_part_1 core_utils_part_2 core_vector_part_1 core_vector_part_2 \
     core_map_part_1 core_map_part_2 core_map_part_3
make NAMESPACE=std core_vector_part_1   # same tests against std
```

Builds use `-std=c++98 -Wall -Wextra -Werror -fsanitize=address`.
