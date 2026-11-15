
from itertools import accumulate, repeat, dropwhile, chain


def get_neighbors(cell, rows, columns):
    row, column = cell

    return filter(
        lambda neighbor: (
            1 <= neighbor[0] <= rows
            and 1 <= neighbor[1] <= columns
        ),
        (
            (row + 1, column),
            (row - 1, column),
            (row, column + 1),
            (row, column - 1)
        )
    )


def capture_next_day(state, rows, columns):
    day, captured_cells, frontier_cells = state

    new_cells = set(
        chain.from_iterable(
            map(
                lambda cell: get_neighbors(cell, rows, columns),
                frontier_cells
            )
        )
    ) - captured_cells

    return (
        day + 1,
        captured_cells | new_cells,
        new_cells
    )


def solve(rows, columns, landing_positions):
    captured_cells = set(landing_positions)

    initial_state = (
        1,
        captured_cells,
        captured_cells
    )

    states = accumulate(
        repeat(None),
        lambda state, _: capture_next_day(
            state, rows, columns
        ),
        initial=initial_state
    )

    final_state = next(
        dropwhile(
            lambda state: len(state[1]) < rows * columns,
            states
        )
    )

    return final_state[0]


print(solve(3, 3, [(2, 2)]))  # 3
print(solve(5, 5, [(1, 1)]))  # 9
print(solve(4, 4, [(1, 1), (4, 4)]))  # 4
