class MazeSolver:
    def __init__(self, maze):
        self.maze = maze
        self.visited = set()
        self.path = []

    def solve(self, start, end):
        self.path.append(start)
        self.visited.add(start)

        if start == end:
            return True

        neighbors = self.get_neighbors(start)
        for neighbor in neighbors:
            if neighbor not in self.visited:
                if self.solve(neighbor, end):
                    return True

        self.path.pop()
        return False

    def get_neighbors(self, position):
        row, col = position
        neighbors = []

        # Check for valid neighbors (up, down, left, right)
        if row > 0 and self.maze[row - 1][col] != "#":
            neighbors.append((row - 1, col))
        if row < len(self.maze) - 1 and self.maze[row + 1][col] != "#":
            neighbors.append((row + 1, col))
        if col > 0 and self.maze[row][col - 1] != "#":
            neighbors.append((row, col - 1))
        if col < len(self.maze[0]) - 1 and self.maze[row][col + 1] != "#":
            neighbors.append((row, col + 1))

        return neighbors


# Example usage
maze = [
    ["S", ".", ".", "#", ".", ".", "."],
    [".", "#", ".", "#", ".", "#", "."],
    [".", "#", ".", ".", ".", "#", "."],
    [".", "#", "#", "#", ".", "#", "."],
    [".", ".", ".", ".", ".", ".", "E"]
]

solver = MazeSolver(maze)
start = (0, 0)
end = (len(maze) - 1, len(maze[0]) - 1)

if solver.solve(start, end):
    print("Path found:")
    for position in solver.path:
        print(position)
else:
    print("No path found.")
