# Maze-solver
import heapq

def dijkstra(maze, start, end):
    rows, cols = len(maze), len(maze[0])
    distances = [[float('inf')] * cols for _ in range(rows)]
    distances[start[0]][start[1]] = 0
    visited = set()
    pq = [(0, start)]
    
    while pq:
        dist, curr = heapq.heappop(pq)
        if curr == end:
            break
        if curr in visited:
            continue
        visited.add(curr)
        row, col = curr
        neighbors = [(row+1, col), (row-1, col), (row, col+1), (row, col-1)]
        for nr, nc in neighbors:
            if 0 <= nr < rows and 0 <= nc < cols and maze[nr][nc] != '#':
                new_dist = dist + 1
                if new_dist < distances[nr][nc]:
                    distances[nr][nc] = new_dist
                    heapq.heappush(pq, (new_dist, (nr, nc)))
    
    # Backtrack to find the shortest path
    shortest_path = []
    row, col = end
    while (row, col) != start:
        shortest_path.append((row, col))
        row, col = min(((r, c) for r, c in [(row+1, col), (row-1, col), (row, col+1), (row, col-1)] if 0 <= r < rows and 0 <= c < cols and distances[r][c] == distances[row][col] - 1), key=lambda x: (x[0], x[1]))
    shortest_path.append(start)
    shortest_path.reverse()
    return shortest_path

# Example usage
maze = [
    ['#', '#', '#', '#', '#', '#', '#', '#', '#', '#'],
    ['#', ' ', ' ', ' ', '#', ' ', ' ', ' ', ' ', '#'],
    ['#', ' ', '#', ' ', '#', ' ', '#', '#', ' ', '#'],
    ['#', ' ', '#', ' ', ' ', ' ', '#', ' ', ' ', '#'],
    ['#', ' ', '#', '#', '#', '#', '#', ' ', '#', '#'],
    ['#', ' ', ' ', ' ', '#', ' ', ' ', ' ', ' ', '#'],
    ['#', '#', '#', ' ', '#', ' ', '#', '#', ' ', '#'],
    ['#', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', '#'],
    ['#', '#', '#', '#', '#', '#', '#', '#', '#', '#']
]

start = (1, 1)
end = (7, 8)

shortest_path = dijkstra(maze, start, end)
print("Shortest path:", shortest_path)
