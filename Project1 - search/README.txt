Name: Reza Shisheie
CSU ID: 2708062
Project1

I put 25+ hours on the project. What made it difficult to understand was the unique was the states should be fed. 
I did not find any help/instruction on the structure of Pac-Man and how function feedback to each oether. This is 
something, of available can cut the time spent on the first project substatially.  

Here is the description for everyone of the fucntions. Please refer to the code for complete explanation of each sement
and also comments. 


def graphSearch(problem, inputType):
    """ FUNCTIONALITY DESCRIPTION
    depending on what is the input number it iterats between stack, queue and Priority queue
    and initialize the problem. Then push the first node which is the current node into the frontierPath. 
    from this moment depending on the data structure nodes are pushed and poped from the structure.
    The esiest way to deal with this problem is using the same architecture that the get successor function 
    returns and putting appending all of then in a list and retuning. In this case, there is less hastile dealing
    with stack object. I also implemented a seprate code with just the last node and the whole path but it had etechnical
    issues. then, I changed to this one. I left them commented at the enf of this code
    
    1: Stack         --> good for DFS
    2: Queue         --> good for BFS
    3: PriorityQueue --> good for UCS
    
    PriorityQueue is also good for A* but due to complexity of implementation A* has a separate 
    implemetation 
    
    """


class CornersProblem(search.SearchProblem):
    def getStartState(self):
        """ FUNCTIONALITY DESCRIPTION
        the way that this setup work is to put the starting position in 
        the first cell and the rest of corners to the second, and the flag 
        for the status of corners (whther eaten or seen or not) in the thhird one.  
        and this is the starting state. for every move once the corner is met, 
        the corrosponding flag will be turned off. Once the sum of flags becomes
        4 it means that all of them were seen and problem done. -->STOP. 
        This will be dicussed in detail in the 
        rest of the program
        """
  
     
        
    def isGoalState(self, state):
        """ FUNCTIONALITY DESCRIPTION
        if the sume of all status flags are 4, it means that all of the corners
        were met and goal is reached. if there is still corners which has not 
        been metit means, then the corrosponding flag would be still 0 and the 
        return will be False
        for instance a [ 0, 0, 0, 0 ] means that none of the points were seen
        however, a [ 1, 1, 1, 1 ] means that all of the points were seen
        
        """
 


    def getSuccessors(self, state):
            """ FUNCTIONALITY DESCRIPTION
            it gets state[0] as x and y and state[1] as all the problem corners
            and the thord one is the flag about the corners. if the flag is 1 
            it means that the corner is met and if it is 0 it has not been met. 
            Actions.directionToVector(action) gets the travel cost and with one 
            of the 4 direction (action) already put in the main for loop. 
            nextx and nexty are generated by adding dx and dy to x and y. 
            By recaling self.wall, I can see if I pacman hits a wall by taking 
            that step or not. if the return value of hitWall is false it means 
            that there is no wall on the way and we can proceed. then a new list 
            "listOfCornersNotVisited" is generated out of those corners not been 
            visited. 
            if the next node is one of those corners, that node has to be taken 
            off of the list of corners and the return has to be updated. the result 
            would be the newstate + action + 1 (cost). This result will be appended 
            into the successors.
            By repeating this process for the other three actions SOUTH EAST WEST, 
            all sucessors will be determined and appended to the sucessor list. 
            Finally the sucessor list will be returned. 
                THIS IS ONE OF THE IMPORTANT THINGS I LEARNED: if I copy a list 
                to another list things get messed up. This is I believe because 
                of cnpy copying the first point or pointer. I am not really sure 
                but for the recet of the code I copied list one by one into a 
                new list.            
             """
   

def cornersHeuristic(state, problem):
    """ FUNCTIONALITY DESCRIPTION
    for distance I used the manhattan distance. this function takes the current node
    location and the remaining corners. then takes the distance from the current node 
    to every corner which is in the cornersNotVisited. it puts all the distances from 
    current node to all corners into outputManhattan and then goes into a loop to choose 
    the one which has the shortest path to the current node. Then huristic is updated
    by adding the distance associated to the distance between these two point. Then the
    closest corner which was selected in previous step is taken and used to update the 
    current node (current node was previously the start point). Once the current node 
    is updated to the first corner, that corner will be removed from the list of 
    cornersNotVisited. The reason to do that is to avoid recalculating distance to 
    those corners.
        For the rest of the program I used three approaches:
    1. Define a huristic as the distance from the current node to the closest one: 
    This approach is implemented when the break at the end of the code just before 
    "return huristicReturn" is activated. What it does is that it breaks the loop 
    as soon as     the first closest corner is found. This gives a result but this 
    is not good since the huristic is not as close as possible to the actual path. 
    It only enounters the distance to one node and thus it causes more expansions. 
    here is the result
    *** FAIL: Heuristic resulted in expansion of 1475 nodes --> I got 2/3
    
    2. A better approach is to define the huristic as the farthest point to the current 
    position (localMan[0] > minOutput[0]). I am still debating on why it is a better 
    solution but I guess it is closer to the actual path which is longer but it is 
    a better result than the shortest corner. Here is the result:
    *** PASS: Heuristic resulted in expansion of 1136 nodes --> I got 3/3
    
    3. the best result would be a huristic which is the closest to the actual path. 
    By repeating the first process and deactivating the break, this process is 
    repeated for the the updated current node (which was the corner closest to the initial node) 
    to the remaning corner which were not visited and then huristic is updated. 
    once all corners were taken out the huristic is returned. The function uses 
    minimum distance from the intial node to all corners one after the other one 
    as huristic. the goal is to find the shortest distance from current position 
    to all corvers where there is food. The closer the huristic is to the actual 
    path the better the result it. This is the closest huristic which is closest 
    to the actual path. here is the result:
    *** PASS: Heuristic resulted in expansion of 692 nodes --> I got 3/3
    
    4. This can be even further improved by using the madeDistance. What it does 
    is that it finds the shortest actual path from the current node to the corners. 
    Since this huristic is identical to the actual path it should take the minimum 
    expansion. I did not implemented that but it is implemented in the 
    "def foodHeuristic(state, problem):" problem and you can see the effect. 
    
    
    Manhattan Distance reasoning:
    1. the reason the manhattan distance is used is that as long as the motion 
    of pacman is not diagonal, manhattan distance is admissible since it takes 
    the minimum path and considering the walls the actual path to the nearest 
    goal is either larger or equal to manhattan distance. 
    2. manhattan distance is also consistent since the cost from any two points 
    cost(A to C) is always larger than h(A) - h(C):
    cost(A to C) >= h(A) - h(C) since the actual cost ( cost(A to C) ) is either 
    manhattan distance or considering walls it is larger than manhattan distance.
    3. the huristic non-negative and non-trivial since it adds up values so it 
    is always at least 1 (distance of two neighbours)
         
    """
   
   
   

class AStarFoodSearchAgent(SearchAgent):
def foodHeuristic(state, problem):
    """ FUNCTIONALITY DESCRIPTION
    For this problem I figured 3 different solutions. 
    
    1. I started with searching all the foods and returning the minium huristic 
    ( if localMan[0] < minOutput[0]: ) which is likely to be the one of neighbours. 
    for the huristic I used manhattan distance. I got 2/4 with 13898 expanded nodes. 
    The reason that so many nodes were expanded was due to small value of huristic 
    which leads the pacman to go one step at a time and as foods disappear these 
    steps increases but it would be the minimum number of steps to be taken to the 
    goal. here is the result:
    *** FAIL: test_cases/q7/food_heuristic_grade_tricky.test
    *** 	expanded nodes: 13898
    *** 	thresholds: [15000, 12000, 9000, 7000]
    
    2. in the second run I used maximum distance ( if localMan[0] > minOutput[0]: )
    of a food from the current location. I assume more foods can be consumed on the 
    way to the goal which decreases node expansion. I got 9551 node expansion and 
    got 3/4. here is the result
    *** FAIL: test_cases/q7/food_heuristic_grade_tricky.test
    *** 	expanded nodes: 9551
    *** 	thresholds: [15000, 12000, 9000, 7000]
    
    3. The biggest achivement was when I discovered that I can use the mazeDistance 
    function and combine it with the point which has the longest path. The power of 
    this method is its accuracy and similarity of the huristic estimate to the 
    actual path. this method makes a maze for each two points and finds the shortest 
    path to them. This means that the estimated huristic is closest to the actual path 
    (as mentioned in slides). This means that it will have fewer branches and thus 
    gets to the goal faster. However, it has a drawback. Although, it has fewer braches, 
    it takes a long time to evaluate it since the algorithm is looking for the 
    best actual path as huristic. This is like the example of finding the best path by Uber
    driver. If the path is very accurate but takes days to be evaluated, there is no point 
    in it. I got 5/4 in it and the expansion nodes were the minimum 4137. 
    here is the result:
    *** PASS: test_cases/q7/food_heuristic_grade_tricky.test
    *** 	expanded nodes: 4137
    *** 	thresholds: [15000, 12000, 9000, 7000]

    I still can't really explain some of the behaviour of pacman once the following 
    code is implemented: 
    $ python pacman.py -l trickySearch -p AStarFoodSearchAgent
    
    pacman in this code goes for the current position to node which is farthes. 
    However,on the way to it, it captures some food, which I can't explain why.  
    
    """
 
  
  
    
    
class ClosestDotSearchAgent(SearchAgent):
    def findPathToClosestDot(self, gameState):
        """ FUNCTIONALITY DESCRIPTION
        all the problem is designed in the above. Knowing the probelm I can take 
        the path using any algorithm and return the path. This process is 
        repeated until goal is met which is all dots are eaten. but for every iteration 
        there is a goal which is the closest one and this goal is satisfied once the 
        closest food is eaten. but this process also repaest until all foods also 
        eaten too. Any algorithm can be used,here but a BFS or a UCS would be the best. 
        The following implementation is BFS 
        """
   



class AnyFoodSearchProblem(PositionSearchProblem):
    def isGoalState(self, state):    
        """ FUNCTIONALITY DESCRIPTION
        the goal state happens when the state of the Pacman is at the food location. 
        Thus, if state is one of the food locations which still exists and were not 
        eaten, the the return is True. Otherwise Pacman is just on the board and not
        on any food. Thus the return is False.

        """
       
        
        
 *************************************************************************************************
 *************************************************************************************************
 *************************************************************************************************
 SAMPLE EXECUTION:
 
  
 Question q1
===========

*** PASS: test_cases/q1/graph_backtrack.test
*** 	solution:		['1:A->C', '0:C->G']
*** 	expanded_states:	['A', 'D', 'C']
*** PASS: test_cases/q1/graph_bfs_vs_dfs.test
*** 	solution:		['2:A->D', '0:D->G']
*** 	expanded_states:	['A', 'D']
*** PASS: test_cases/q1/graph_infinite.test
*** 	solution:		['0:A->B', '1:B->C', '1:C->G']
*** 	expanded_states:	['A', 'B', 'C']
*** PASS: test_cases/q1/graph_manypaths.test
*** 	solution:		['2:A->B2', '0:B2->C', '0:C->D', '2:D->E2', '0:E2->F', '0:F->G']
*** 	expanded_states:	['A', 'B2', 'C', 'D', 'E2', 'F']
*** PASS: test_cases/q1/pacman_1.test
*** 	pacman layout:		mediumMaze
*** 	solution length: 130
*** 	nodes expanded:		146

### Question q1: 3/3 ###


Question q2
===========

*** PASS: test_cases/q2/graph_backtrack.test
*** 	solution:		['1:A->C', '0:C->G']
*** 	expanded_states:	['A', 'B', 'C', 'D']
*** PASS: test_cases/q2/graph_bfs_vs_dfs.test
*** 	solution:		['1:A->G']
*** 	expanded_states:	['A', 'B']
*** PASS: test_cases/q2/graph_infinite.test
*** 	solution:		['0:A->B', '1:B->C', '1:C->G']
*** 	expanded_states:	['A', 'B', 'C']
*** PASS: test_cases/q2/graph_manypaths.test
*** 	solution:		['1:A->C', '0:C->D', '1:D->F', '0:F->G']
*** 	expanded_states:	['A', 'B1', 'C', 'B2', 'D', 'E1', 'F', 'E2']
*** PASS: test_cases/q2/pacman_1.test
*** 	pacman layout:		mediumMaze
*** 	solution length: 68
*** 	nodes expanded:		269

### Question q2: 3/3 ###


Question q3
===========

*** PASS: test_cases/q3/graph_backtrack.test
*** 	solution:		['1:A->C', '0:C->G']
*** 	expanded_states:	['A', 'B', 'C', 'D']
*** PASS: test_cases/q3/graph_bfs_vs_dfs.test
*** 	solution:		['1:A->G']
*** 	expanded_states:	['A', 'B']
*** PASS: test_cases/q3/graph_infinite.test
*** 	solution:		['0:A->B', '1:B->C', '1:C->G']
*** 	expanded_states:	['A', 'B', 'C']
*** PASS: test_cases/q3/graph_manypaths.test
*** 	solution:		['1:A->C', '0:C->D', '1:D->F', '0:F->G']
*** 	expanded_states:	['A', 'B1', 'C', 'B2', 'D', 'E1', 'F', 'E2']
*** PASS: test_cases/q3/ucs_0_graph.test
*** 	solution:		['Right', 'Down', 'Down']
*** 	expanded_states:	['A', 'B', 'D', 'C', 'G']
*** PASS: test_cases/q3/ucs_1_problemC.test
*** 	pacman layout:		mediumMaze
*** 	solution length: 68
*** 	nodes expanded:		269
*** PASS: test_cases/q3/ucs_2_problemE.test
*** 	pacman layout:		mediumMaze
*** 	solution length: 74
*** 	nodes expanded:		260
*** PASS: test_cases/q3/ucs_3_problemW.test
*** 	pacman layout:		mediumMaze
*** 	solution length: 152
*** 	nodes expanded:		173
*** PASS: test_cases/q3/ucs_4_testSearch.test
*** 	pacman layout:		testSearch
*** 	solution length: 7
*** 	nodes expanded:		14
*** PASS: test_cases/q3/ucs_5_goalAtDequeue.test
*** 	solution:		['1:A->B', '0:B->C', '0:C->G']
*** 	expanded_states:	['A', 'B', 'C']

### Question q3: 3/3 ###


Question q4
===========

*** PASS: test_cases/q4/astar_0.test
*** 	solution:		['Right', 'Down', 'Down']
*** 	expanded_states:	['A', 'B', 'D', 'C', 'G']
*** PASS: test_cases/q4/astar_1_graph_heuristic.test
*** 	solution:		['0', '0', '2']
*** 	expanded_states:	['S', 'A', 'D', 'C']
*** PASS: test_cases/q4/astar_2_manhattan.test
*** 	pacman layout:		mediumMaze
*** 	solution length: 68
*** 	nodes expanded:		221
*** PASS: test_cases/q4/astar_3_goalAtDequeue.test
*** 	solution:		['1:A->B', '0:B->C', '0:C->G']
*** 	expanded_states:	['A', 'B', 'C']
*** PASS: test_cases/q4/graph_backtrack.test
*** 	solution:		['1:A->C', '0:C->G']
*** 	expanded_states:	['A', 'B', 'C', 'D']
*** PASS: test_cases/q4/graph_manypaths.test
*** 	solution:		['1:A->C', '0:C->D', '1:D->F', '0:F->G']
*** 	expanded_states:	['A', 'B1', 'C', 'B2', 'D', 'E1', 'F', 'E2']

### Question q4: 3/3 ###


Question q5
===========

Starting state is at:  
%%%%%%%%
%.    .%
%   <  %
% %%%% %
% %    %
% % %%%%
%.%   .%
%%%%%%%%
Score: 0

Corners are at:  ((1, 1), (1, 6), (6, 1), (6, 6))
*** PASS: test_cases/q5/corner_tiny_corner.test
*** 	pacman layout:		tinyCorner
*** 	solution length:		28

### Question q5: 3/3 ###


Question q6
===========

Starting state is at:  
%%%%%%
%.  .%
%<   %
%.  .%
%%%%%%
Score: 0

Corners are at:  ((1, 1), (1, 3), (4, 1), (4, 3))
*** PASS: heuristic value less than true cost at start state
Starting state is at:  
%%%%%%
%.  .%
% %% %
%.<%.%
%%%%%%
Score: 0

Corners are at:  ((1, 1), (1, 3), (4, 1), (4, 3))
*** PASS: heuristic value less than true cost at start state
Starting state is at:  
%%%%%%%%
%.%   .%
% % %  %
% % %< %
%   %  %
%%%%%  %
%.    .%
%%%%%%%%
Score: 0

Corners are at:  ((1, 1), (1, 6), (6, 1), (6, 6))
*** PASS: heuristic value less than true cost at start state
Starting state is at:  
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%.      % % %              %.%
%       % % %%%%%% %%%%%%% % %
%       %        %     % %   %
%%%%% %%%%% %%% %% %%%%% % %%%
%   % % % %   %    %     %   %
% %%% % % % %%%%%%%% %%% %%% %
%       %     %%     % % %   %
%%% % %%%%%%% %%%% %%% % % % %
% %           %%     %     % %
% % %%%%% % %%%% % %%% %%% % %
%   %     %      % %   % %%% %
%.  %<%%%%%      % %%% %    .%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
Score: 0

Corners are at:  ((1, 1), (1, 12), (28, 1), (28, 12))
path: ['North', 'East', 'East', 'East', 'East', 'North', 'North', 'West', 'West', 'West', 'West', 'North', 'North', 'North', 'North', 'North', 'North', 'North', 'North', 'West', 'West', 'West', 'West', 'South', 'South', 'East', 'East', 'East', 'East', 'South', 'South', 'South', 'South', 'South', 'South', 'West', 'West', 'South', 'South', 'South', 'West', 'West', 'East', 'East', 'North', 'North', 'North', 'East', 'East', 'East', 'East', 'East', 'East', 'East', 'East', 'South', 'South', 'East', 'East', 'East', 'East', 'East', 'North', 'North', 'East', 'East', 'North', 'North', 'East', 'East', 'North', 'North', 'East', 'East', 'East', 'East', 'South', 'South', 'South', 'South', 'East', 'East', 'North', 'North', 'East', 'East', 'South', 'South', 'South', 'South', 'South', 'North', 'North', 'North', 'North', 'North', 'North', 'North', 'West', 'West', 'North', 'North', 'East', 'East', 'North', 'North']
path length: 106
*** PASS: Heuristic resulted in expansion of 692 nodes

### Question q6: 3/3 ###


Question q7
===========

*** PASS: test_cases/q7/food_heuristic_1.test
*** PASS: test_cases/q7/food_heuristic_10.test
*** PASS: test_cases/q7/food_heuristic_11.test
*** PASS: test_cases/q7/food_heuristic_12.test
*** PASS: test_cases/q7/food_heuristic_13.test
*** PASS: test_cases/q7/food_heuristic_14.test
*** PASS: test_cases/q7/food_heuristic_15.test
*** PASS: test_cases/q7/food_heuristic_16.test
*** PASS: test_cases/q7/food_heuristic_17.test
*** PASS: test_cases/q7/food_heuristic_2.test
*** PASS: test_cases/q7/food_heuristic_3.test
*** PASS: test_cases/q7/food_heuristic_4.test
*** PASS: test_cases/q7/food_heuristic_5.test
*** PASS: test_cases/q7/food_heuristic_6.test
*** PASS: test_cases/q7/food_heuristic_7.test
*** PASS: test_cases/q7/food_heuristic_8.test
*** PASS: test_cases/q7/food_heuristic_9.test
*** PASS: test_cases/q7/food_heuristic_grade_tricky.test
*** 	expanded nodes: 4137
*** 	thresholds: [15000, 12000, 9000, 7000]

### Question q7: 5/4 ###


Question q8
===========

[SearchAgent] using function depthFirstSearch
[SearchAgent] using problem type PositionSearchProblem
*** PASS: test_cases/q8/closest_dot_1.test
*** 	pacman layout:		Test 1
*** 	solution length:		1
[SearchAgent] using function depthFirstSearch
[SearchAgent] using problem type PositionSearchProblem
*** PASS: test_cases/q8/closest_dot_10.test
*** 	pacman layout:		Test 10
*** 	solution length:		1
[SearchAgent] using function depthFirstSearch
[SearchAgent] using problem type PositionSearchProblem
*** PASS: test_cases/q8/closest_dot_11.test
*** 	pacman layout:		Test 11
*** 	solution length:		2
[SearchAgent] using function depthFirstSearch
[SearchAgent] using problem type PositionSearchProblem
*** PASS: test_cases/q8/closest_dot_12.test
*** 	pacman layout:		Test 12
*** 	solution length:		3
[SearchAgent] using function depthFirstSearch
[SearchAgent] using problem type PositionSearchProblem
*** PASS: test_cases/q8/closest_dot_13.test
*** 	pacman layout:		Test 13
*** 	solution length:		1
[SearchAgent] using function depthFirstSearch
[SearchAgent] using problem type PositionSearchProblem
*** PASS: test_cases/q8/closest_dot_2.test
*** 	pacman layout:		Test 2
*** 	solution length:		1
[SearchAgent] using function depthFirstSearch
[SearchAgent] using problem type PositionSearchProblem
*** PASS: test_cases/q8/closest_dot_3.test
*** 	pacman layout:		Test 3
*** 	solution length:		1
[SearchAgent] using function depthFirstSearch
[SearchAgent] using problem type PositionSearchProblem
*** PASS: test_cases/q8/closest_dot_4.test
*** 	pacman layout:		Test 4
*** 	solution length:		3
[SearchAgent] using function depthFirstSearch
[SearchAgent] using problem type PositionSearchProblem
*** PASS: test_cases/q8/closest_dot_5.test
*** 	pacman layout:		Test 5
*** 	solution length:		1
[SearchAgent] using function depthFirstSearch
[SearchAgent] using problem type PositionSearchProblem
*** PASS: test_cases/q8/closest_dot_6.test
*** 	pacman layout:		Test 6
*** 	solution length:		2
[SearchAgent] using function depthFirstSearch
[SearchAgent] using problem type PositionSearchProblem
*** PASS: test_cases/q8/closest_dot_7.test
*** 	pacman layout:		Test 7
*** 	solution length:		1
[SearchAgent] using function depthFirstSearch
[SearchAgent] using problem type PositionSearchProblem
*** PASS: test_cases/q8/closest_dot_8.test
*** 	pacman layout:		Test 8
*** 	solution length:		1
[SearchAgent] using function depthFirstSearch
[SearchAgent] using problem type PositionSearchProblem
*** PASS: test_cases/q8/closest_dot_9.test
*** 	pacman layout:		Test 9
*** 	solution length:		1

### Question q8: 3/3 ###


Finished at 10:55:05

Provisional grades
==================
Question q1: 3/3
Question q2: 3/3
Question q3: 3/3
Question q4: 3/3
Question q5: 3/3
Question q6: 3/3
Question q7: 5/4
Question q8: 3/3
------------------
Total: 26/25

Your grades are NOT yet registered.  To register your grades, make sure
to follow your instructor's guidelines to receive credit on your project.


 
