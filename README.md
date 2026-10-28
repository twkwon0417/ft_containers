# ft_containers: C++98 STL Containers

A reimplementation of `vector` and `map` from the C++ standard library in C++98, following the École 42 ft_containers subject. I built it in the GDGoC Konkuk container study group (Oct 2025 – Jan 2026).

The containers live in namespace `ft` and are header-only (`include/ft/`).

## What is implemented

**`ft::vector`**
- Storage is managed through `std::allocator`: memory is allocated first, elements are created with `construct()` and destroyed with `destroy()`, so the code controls exactly when each object's lifetime starts and ends.
- Copy assignment uses copy-and-swap: build a full copy first, then swap it in, so a failed copy leaves the original unchanged.
- The range constructor and `insert(first, last)` use `enable_if` and `is_integral` so that `vector(5, 10)` is not mistaken for an iterator range, and dispatch on `iterator_traits<It>::iterator_category`.
- `at()` throws `std::out_of_range` and `reserve()` throws `std::length_error`, as in the standard.

**`ft::map`**
- Red-black tree with insert and erase rebalancing (`_insert_fixup`, `_erase_fixup`, left and right rotations).
- Bidirectional `iterator` and `const_iterator`.
- `find`, `operator[]`, `count`, `lower_bound`, `upper_bound`, `equal_range`, `swap`, `clear`, `key_comp`, `value_comp`, and the comparison operators.

**Utilities**
- `pair`, `make_pair`, `equal`, `lexicographical_compare`
- `enable_if`, `is_integral`, `iterator_traits`, `reverse_iterator`

## Testing

The tests and the Makefile come from [cpp-containers-tester](https://github.com/Yonaim/cpp-containers-tester), written by a member of the study group. The same test sources are built twice, once against `ft` and once against `std`, and the results are compared.

The test sources are not part of this repository. To run them, copy the tester's `test/` directory next to `include/`:

```bash
git clone https://github.com/Yonaim/cpp-containers-tester.git
cp -r cpp-containers-tester/test .
make core_utils_part_1 core_utils_part_2 core_vector_part_1 core_vector_part_2 \
     core_map_part_1 core_map_part_2 core_map_part_3
make NAMESPACE=std core_vector_part_1   # the same tests against the standard library
```

The build uses `-std=c++98 -Wall -Wextra -Werror -fsanitize=address`.
