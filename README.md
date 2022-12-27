# mysubscription

prices = {
    "TOI": [3, 3, 3, 3, 3, 5, 6],
    "Hindu": [2.5, 2.5, 2.5, 2.5, 2.5, 4, 4],
    "ET": [4, 4, 4, 4, 4, 4, 10],
    "BM": [1.5, 1.5, 1.5, 1.5, 1.5, 1.5, 1.5],
    "HT": [2, 2, 2, 2, 2, 4, 4]
}


def calculate_total_cost(combination):
  total_cost = 0
  for newspaper, days in combination.items():
    total_cost += sum([prices[newspaper][i] for i in days])
  return total_cost


def generate_combinations(budget, newspapers, combination=None):
  if combination is None:
    combination = {}
  if budget < 0:
    return
  if budget == 0:
    yield combination
  for newspaper in newspapers:
    for day in range(7):
      if newspaper not in combination:
        combination[newspaper] = []
      if day not in combination[newspaper]:
        combination[newspaper].append(day)
        yield from generate_combinations(budget - prices[newspaper][day], newspapers, combination)
        combination[newspaper].remove(day)
      if not combination[newspaper]:
        del combination[newspaper]

budget = 40
combinations = generate_combinations(budget, prices.keys())
for combination in combinations:
  total_cost = calculate_total_cost(combination)
  if total_cost == budget:
    print(combination)
