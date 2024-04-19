from itertools import combinations

def count_valid_pizzas(N, forbidden_pairs):
    total_subsets = 2 ** N  # Total number of subsets of ingredients
    
    # Generate all possible subsets of ingredients
    all_subsets = []
    for r in range(1, N + 1):
        all_subsets.extend(combinations(range(1, N + 1), r))
    
    invalid_subsets = set()
    for pair in forbidden_pairs:
        pair_set = set(pair)
        for subset in all_subsets:
            if pair_set.issubset(subset):
                invalid_subsets.add(subset)
    
    valid_subsets = set(all_subsets) - invalid_subsets
    count = len(valid_subsets)
    
    return count

# Example usage
N = 5
forbidden_pairs = [(1, 2), (3, 4)]
result = count_valid_pizzas(N, forbidden_pairs)
print(f"The number of different pizzas Erhan can make is: {result}")
