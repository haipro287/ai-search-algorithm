# ai-search-algorithm

ai project 2020

## Q1: Hàm DFS
    //Sử dụng stack để lưu các state mới tìm thấy
    
    discovered = util.Stack()
    
    //đưa state đầu vào ngăn xếp với độ ưu tiên bằng 0 (do không mất chi phí đến state này)
    discovered.push((problem.getStartState(), []))
    visited = list() 
    
    //kiểm tra ngăn xếp
    while not discovered.isEmpty():
    
        //lấy ra phần tử đầu của ngăn xếp, kiếm tra xem state đã được thăm chưa, nếu chưa thì thêm vào
        currentState, actions = discovered.pop()
        if currentState in visited:
            continue
        visited.append(currentState)
        
        //nếu đã đạt được đích sẽ trả về mảng hành động để thực thi
        if problem.isGoalState(currentState):
            return actions
         
        //lấy các state con của state hiện tại và đưa vào ngăn xếp
        successors = problem.getSuccessors(currentState)
        for state, action, cost in successors:
        
                //tính chi chí đến state đc tìm thấy và đưa vào ngăn xếp 
                newActions = actions + [action]
                discovered.push((state, newActions))
                
## Q2: Hàm BFS
    //sử dụng queue để lưu các state mới tìm thấy
    
    discovered = util.Queue()
    
    //đưa state đầu vào hàng đợi với độ ưu tiên bằng 0 (do không mất chi phí đến state này)
    discovered.push((problem.getStartState(), []))
    visited = list()
    
    //kiểm tra hàng đợi
    while not discovered.isEmpty():
    
        //lấy ra phần tử đầu của hàng đợi, kiếm tra xem state đã được thăm chưa, nếu chưa thì thêm vào
        currentState, actions = discovered.pop()
        
        //nếu đã đạt được đích sẽ trả về mảng hành động để thực thi
        if currentState in visited:
            continue
        visited.append (currentState)
        
        //nếu đã đạt được đích sẽ trả về mảng hành động để thực thi
        if problem.isGoalState(currentState):
            return actions
            
        //lấy các state con của state hiện tại và đưa vào hàng đợi
        successors = problem.getSuccessors(currentState)
        for state, action, cost in successors:
        
            //đưa các state tìm thấy vào hàng đợi
            newActions = actions + [action]
            discovered.push((state, newActions))
    
## Q3: Hàm UC
    //sử dụng priority queue để lưu các state mới tìm thấy
    
    discovered = util.PriorityQueue()
    
    //đưa state đầu vào hàng đợi với độ ưu tiên bằng 0 (do không mất chi phí đến state này)
    discovered.update((problem.getStartState(), []), 0)
    visited = list()
    
    //kiểm tra hàng đợi
    while not discovered.isEmpty():
    
        //lấy ra phần tử đầu của hàng đợi, kiếm tra xem state đã được thăm chưa, nếu chưa thì thêm vào
        currentState, actions = priQueue.pop()
        if currentState in visited:
            continue
        visited.append(currentState)
        
        //nếu đã đạt được đích sẽ trả về mảng hành động để thực thi
        if problem.isGoalState(currentState):
            return actions
            
        //lấy các state con của state hiện tại và đưa vào hàng đợi
        successors = problem.getSuccessors(currentState)
        for state, action, cost in successors:
            newActions = actions + [action]
            
            //đưa các state tìm thấy vào hàng đợi với độ ưu tiên bằng tổng chi phí để đến state đó
            discovered.update((state, newActions), problem.getCostOfActions(newActions))
            
## Q4: Hàm A*
    //giống ucs nhưng độ ưu tiên của state là tổng chi phí + giá trị dự đoán
    
## Q5: Hoàn thành giải thuật tìm 4 góc bằng bfs
    //thêm danh sách trạng thái của 4 góc vào class CornersProblem, 0 là chưa thăm, 1 là đã thăm
      cornersStatus = [0, 0, 0, 0]
    
    //getStartState()
        return (startingPosition, cornersStatus)
    
    //isGoalState(state)
        for item in state[1]:
            if item == 0:
                return False
        return True
     
    //getSuccessors(state)
        successors = []
        
        //lấy vị trí và trạng thái các góc hiện tại
        x, y = state[0]
        newCornersStatus = state[1][:]
        
        //lấy danh sách các hành động có thể xảy ra
        for action in [Directions.NORTH, Directions.SOUTH, Directions.EAST, Directions.WEST]:

            //lấy vị trí của tràn thái mới
            dx, dy = Actions.directionToVector(action)
            nextx, nexty = int(x + dx), int(y + dy)
                                                
            //kiểm tra state mới có hợp lệ hay không (có va tường không).
            //nếu hợp lệ, kiểm tra vị trí mới có phải góc không, nếu có cập nhật lại trạng thái các góc
            //thêm state mới tìm vào danh sách để trả về
            hitsWall = self.walls[nextx][nexty]
            if not hitsWall:
                if (nextx, nexty) in self.corners:
                    newCornersStatus[self.corners.index((nextx, nexty))] = 1
                cost = 1
                successors.append((((nextx, nexty), newCornersStatus), action, cost))
        self._expanded += 1 # DO NOT CHANGE
        return successors

## Q6: Hàm cornersHeuristic
    //cornersHeuristic(state, problem)
        //lấy vị trí các góc và tường
        corners = problem.corners 
        walls = problem.walls
        
        //lấy vị trí hiện tại và trạng thái các góc
        currentPosition = state[0]
        currrentCornersStatus = state[1]
        
        maxHeuristic = 0
        
        //tìm khoảng cách từ vị trí hiện tại đến 4 góc bằng manhattan và trả về giá trị cao nhất
        //giải thích:
        //    coi như góc xa nhất đó sẽ là góc cuối cùng đi tới(góc đạt đc goalState) 
        //    như vậy giá trị về khoảng cách đó sẽ tương đối chính xác đến goalState
        for i in range(0, 3):
            if currrentCornersStatus[i] == 0:
                heuristic = ((abs(corners[i][0] - currentPosition[0]) + abs(corners[i][1] - currentPosition[1])))  
                if heuristic > maxHeuristic:
                    maxHeuristic = heuristic
        return maxHeuristic # Default to trivial solution
