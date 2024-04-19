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
