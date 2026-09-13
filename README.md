# ft_printf

A reimplementation of C's `printf` function, built as part of the 42 School
curriculum. It produces a static library, `libftprintf.a`, and is implemented
on top of the author's own `libft`.

## Features

- Supports the full set of conversions required by the subject:
  `c`, `s`, `p`, `d`, `i`, `u`, `x`, `X` and `%`.
- Handles flags, field width and precision.
- Returns the number of characters printed, matching `printf`'s contract.

## Supported conversions

| Specifier | Description                                    |
| --------- | ---------------------------------------------- |
| `%c`      | Prints a single character.                     |
| `%s`      | Prints a string (prints `(null)` if `NULL`).   |
| `%p`      | Prints a pointer address in hexadecimal.       |
| `%d` / `%i`| Prints a signed decimal integer.              |
| `%u`      | Prints an unsigned decimal integer.            |
| `%x`      | Prints a number in lowercase hexadecimal.      |
| `%X`      | Prints a number in uppercase hexadecimal.      |
| `%%`      | Prints a percent sign.                         |

### Width & precision

- **Width**: a number following `%` sets the minimum field width. The output is
  padded with spaces (or zeros, when combined with the `0` flag) to reach it.
- **Precision**: a leading `.` in the format string. For integers it sets the
  minimum number of digits (zero-padded); for strings it limits the maximum
  number of characters printed. A bare `.` is treated as precision `0`.

### Precedence rules

- `-` overrides `0` (left-justification wins over zero-padding).
- `+` overrides the space flag.
- A precision set for `diuxX` disables the `0` flag (the value is padded with
  spaces instead of zeros).

## Platform behavior

Null pointers passed to `%p` are printed differently depending on the platform,
mirroring the system `printf`:

- **Linux**: `(nil)`
- **macOS**: `0x0`

This is controlled by the `NULLPTR` macro in `ft_printf.h`.

## Build & usage

Compile the library with `make`:

```sh
make        # build libftprintf.a
make bonus  # (alias for all)
make clean  # remove object files
make fclean # remove object files and the library
make re     # fclean + all
```

To use it in your own code, include the header and link the library:

```c
#include "ft_printf.h"

int main(void)
{
    ft_printf("Hello %s, %d%% complete\n", "world", 42);
    return (0);
}
```

```sh
cc main.c libftprintf.a -o program
```

## Repository structure

| File                | Purpose                                              |
| ------------------- | ---------------------------------------------------- |
| `ft_printf.c`      | Entry point and main format dispatcher.               |
| `ft_format_spec.c` | `t_formatspec` creation, flag precedence handling.    |
| `ft_format_parsers.c` | Parses flags, width, precision and specifier.      |
| `ft_format_utils.c`| Padding and justification helpers.                    |
| `ft_print_char.c`  | `%c` implementation.                                  |
| `ft_print_str.c`   | `%s` implementation.                                  |
| `ft_print_dec_int.c`| `%d` / `%i` implementation.                         |
| `ft_print_uint.c`  | `%u` implementation.                                  |
| `ft_print_hex.c`   | `%p` / `%x` / `%X` implementation.                    |
| `ft_printf.h`      | Public prototypes and the `t_formatspec` struct.      |

## Credits

Written by sgadinga as part of the 42 School Cursus. Code is distributed for
educational purposes under the 42 School license.