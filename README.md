from queue import Queue

def Solution(a, b, target):
    m = {}  # Dictionary to track visited states
    isSolvable = False
    path = []  # List to store the path from initial to solution state
    q = Queue()  # Queue for BFS traversal

    #nitializing with jugs being empty
    q.put((0, 0))

    while not q.empty():
        u = q.get()

        # Skip if state is already visited
        if (u[0], u[1]) in m:
            continue

        # Skip if state is outside valid range
        if u[0] > a or u[1] > b or u[0] < 0 or u[1] < 0:
            continue

        # Add current state to the path
        path.append([u[0], u[1]])

        # Mark current state as visited
        m[(u[0], u[1])] = 1

        # Check if target state is reached
        if u[0] == target or u[1] == target:
            isSolvable = True

            # Adjust the path if either jug reaches the target
            if u[0] == target and u[1] != 0:
                path.append([u[0], 0])
            elif u[1] == target and u[0] != 0:
                path.append([0, u[1]])

            # Print the path
            for i in range(len(path)):
                print("(", path[i][0], ",", path[i][1], ")")
            return

        # Enqueue possible states
        q.put((u[0], b))  # Fill Jug2
        q.put((a, u[1]))  # Fill Jug1

        for ap in range(max(a, b) + 1):
            c = u[0] + ap
            d = u[1] - ap

            # Enqueue states resulting from pouring Jug1 to Jug2
            if c == a or (d == 0 and d >= 0):
                q.put((c, d))

            c = u[0] - ap
            d = u[1] + ap

            # Enqueue states resulting from pouring Jug2 to Jug1
            if (c == 0 and c >= 0) or d == b:
                q.put((c, d))

        # Enqueue states resulting from emptying Jugs
        q.put((a, 0))
        q.put((0, b))

    # If solution is not possible, print a message
    if not isSolvable:
        print("Solution not possible")

# Driver code
if __name__ == '__main__':
    Jug1, Jug2, target = 4, 3, 2
    print("Path from initial state to solution state:")
    Solution(Jug1, Jug2, target)
