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



========


from collections import deque

def bfs(graph, start, dragons):
  """
  Performs Breadth-First Search (BFS) to find the minimum number of warriors needed.

  Args:
      graph: Adjacency list representing the city connections.
      start: Starting city for warriors.
      dragons: Dictionary containing dragon information (city, initial heads, growth rate).

  Returns:
      Minimum number of warriors needed, or -1 if dragons cannot be killed.
  """
  queue = deque([(start, 0)])  # (city, warriors)
  visited = set()

  while queue:
    city, warriors = queue.popleft()
    visited.add(city)

    # Check if all dragons are dead
    if all(dragon["heads"] <= 0 for dragon in dragons.values()):
      return warriors

    # Explore adjacent cities
    for neighbor in graph[city]:
      if neighbor not in visited:
        # Check if warrior can kill dragon in this city
        if neighbor in dragons and dragons[neighbor]["heads"] > 0:
          dragons[neighbor]["heads"] -= 1
          queue.append((neighbor, warriors + 1))  # Increase warriors for attacking
        else:
          queue.append((neighbor, warriors))  # Move to next city with same warriors

  return -1  # Dragons cannot be killed

def solve_dragon_warrior(data):
  """
  Solves the Dragon-Warrior problem for each test case.

  Args:
      data: List of lines containing test case information.
  """
  for line in data:
    if line == "000":
      break

    # Read test case data
    N, M, K = map(int, line.split())
    graph = [[] for _ in range(N + 1)]  # Adjacency list (city 0 not used)
    dragons = {}  # Dictionary with dragon information

    for _ in range(M):
      a, b = map(int, input().split())
      graph[a].append(b)
      graph[b].append(a)  # Undirected graph

    for _ in range(K):
      city, heads, growth_rate = map(int, input().split())
      dragons[city] = {"heads": heads, "growth_rate": growth_rate}

    # Find starting city for warriors (city 1 by default)
    start_city = 1

    # Solve using BFS and print results
    min_warriors = bfs(graph, start_city, dragons.copy())
    print(min_warriors if min_warriors != -1 else "Dragons cannot be killed")

# Read input data
data = []
while True:
  line = input()
  if line:
    data.append(line)
  else:
    break

solve_dragon_warrior(data)



=========

def num_pizzas(N, restrictions):
  dp = [[0 for _ in range(1 << N)] for _ in range(N + 1)]
  dp[0][0] = 1

  def is_valid(mask, j):
    for a, b in restrictions:
      if (mask & (1 << a)) != 0 and (mask & (1 << b)) != 0:
        return False
    return True

  for i in range(1, N + 1):
    for mask in range(1 << N):
      for j in range(i):
        if is_valid(mask, j):
          dp[i][mask] += dp[i - 1][mask | (1 << j)]

  return dp[N][0]
