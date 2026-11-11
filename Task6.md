```
from random import choice, random
from pymonad.state import State
from pymonad.tools import curry

# Опустим логику инициализации кол-ва гарантированных ходов для Гримма
BASE_ACTIONS = 5

# Добавим для интереса динамический доп ход.
MABEL_UNDERWEAR_CHANCE = 0.5


def start_turn(extra_action_chances: tuple[float, ...]):
    def init_turn(health: int) -> tuple[int, int]:
        actions = BASE_ACTIONS + sum(random() < chance for chance in extra_action_chances)
        return actions, health

    return State(init_turn)


@curry(2)
def apply_action(attack: str, actions_left: int):
    def inner_action(health: int) -> tuple[int, int]:
        if actions_left <= 0 or health <= 0:
            return actions_left, health

        if attack == "normal":
            damage = 100
        elif attack == "anchor":
            damage = choice((50, 200))
        else:
            raise ValueError(f"Неизвестная атака Grimm: {attack}")

        return actions_left - 1, max(0, health - damage)

    return State(inner_action)


def survival_result(_actions_left: int):
    def transition(health: int) -> tuple[str, int]:
        if health > 0:
            return "Lingerina жива", health
        return "Lingerina погибла", 0

    return State(transition)


user_state = start_turn((MABEL_UNDERWEAR_CHANCE,))

# Можно даже инициализировать спец приемы, совмещающие ряд действий, rpg maker кажется становится понятнее.
finale = (
    user_state
    .then(apply_action("normal"))
    .then(apply_action("normal"))
    .then(apply_action("anchor"))
    .then(apply_action("normal"))
    .then(apply_action("normal"))
    .then(apply_action("anchor"))
    .then(survival_result)
)


if __name__ == "__main__":
    starting_health = int(input("Здоровье Lingerina: "))
    status, _ = finale.run(starting_health)
    print(status)
```