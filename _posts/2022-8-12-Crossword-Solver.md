---
layout: post
title:  Solving Crossword using Backtracking
categories: [Backtracking]
---

We are going to cover the Backtracking Algorithm for Crossword and compared it with the Brute Force approach. The Time and Space Complexity will be discussed at the end of the article.

**Table of contents:**

1. Introduction
2. Naive Approach
3. Backtracking
4. Time and Space Complexity
5. Conclusion

## Introduction
A crossword is a word puzzle that usually consists of a square or rectangular grid of white and black-shaded squares. By solving clues that lead to the solutions, the goal is to fill the white squares with letters, producing words or phrases. The solution words and phrases are inserted in the grid from left to right ("across") and from top to bottom in languages that are written left to right ("down"). The words or phrases are separated by the shaded squares.

Crossword Puzzle | Solution
---|---
![crossword-puzzle](/images/content/2022/08/crossword-puzzle.png) | ![crossword-solution](/images/content/2022/08/crossword-solution.png)

The goal is to fill all of the empty cells in a 6x6 grid with words such that the grid becomes valid. The constraint here is that the character at the intersection point have to be the same.

## Naive Approach
The process in the naive approach fills in the empty cells with no logic and then examines whether it was a valid placement. This can take a long time and be quite inefficient. The Backtracking Algorithm would be an improvement to this naive approach.

## Backtracking
Backtracking is a form of brute-force approach that comes into play when addressing a problem that involves evaluating several options. Since we do not know which one is accurate and we try to solve the problem using the trial and error method, one decision at a time, until we get the desired answer.

The method described above can be used to solve the crossword puzzle. This backtracking algorithm traverses all the vacant cells, incrementally fills the cells with possible words retrieved from a dictionary file, then backtracks when a word filled does not comply with the constraint. This process is repeated until all of the cells are filled.

###### Steps
- We begin by looking for a continuous row/column of empty cells.
- A valid crossword is obtained when all of the cells are filled.
- We try to fill the continuous row/column of empty cells with values retrieved from the dictionary file.
- We examine the intersection point between the horizontal and vertical words for constraint before placing.
- If we can satisfy the constraints, we will place that word at that location, `start-coordinates(a,b)`, `end-coordinates(x,y)` and then restart the procedure by looking for the next continuous row/column of empty cells.
- If none of the words can be placed, we'll have to backtrack and alter the values for previously visited cells.

Let's take a closer look at the algorithm.

###### Pseudocode
```
backtrack_solver(crossword)
    if(no more cells are there to explore)
        return true
    for(all available possibilities)
        try one possibility p
        if(backtrack_solver(crossword with possibility p made) is true)
            return true
        unmake possibility p
    return false
```

###### Implementation
Implementation of the above backtracking algorithm :

The `copy` library is imported to enable the usage of the `deepcopy()` function. In the case of deep copy, an object is copied in another object. It means that any changes made to a copy of an object are not reflected in the original. The `shapely` library is imported to allow us to find the intersection point(constraint) in the crossword.

The class is used to represents each word in the crossword puzzle. When we apply backtracking later, we use the `value` variable to assign values to each word.

```python
import copy
from shapely.geometry import LineString

class Word:
	#coordinates of the starting and ending point
	start_coord = ()
	end_coord = ()	
	#horizontal word = 0, vertical word = 1
	orientation = 0	
	#word length
	length = 0
	#value assigned to this word
	value = ''
```
The `load_crossword_puzzle` and `load_dictionary` function accept an input (filename) and will proceed to read the content of the file. In order to have useful data to work with, it perform a "clean up" by removing the tabs, newlines and spaces and return the result as a list.

```python
def load_crossword_puzzle(filename):
	crossword = []
	with open(filename, 'r') as cfile:
		puzzle = cfile.readlines()
	for line in puzzle:
		replaced = line.replace("\t", "")
		replaced = replaced.replace("\n", "")
		replaced = replaced.replace(" ", "")
		crossword.append(list(replaced))
	return crossword

def load_dictionary(filename):
	dictionary = []
	with open(filename, 'r') as dfile:
		wordslist = dfile.readlines()
	for word in wordslist:
		replaced = word.replace("\n", "")
		dictionary.append(replaced)
	return dictionary
```
The function `find_horizontal_words(crossword)` verify whether the orientation of the words in the crossword puzzle are horizontal. It achieve this by iterating each row of the puzzle and check if the character is `0`. If this is the first instance of `0` character is found, that is the previous character is `#`, the function will save the starting coordinates (row, column) and track the length of the word. The function will continue to iterate to the next column and check for the `0` character. If the next character is `#`, the function will save the previous column position as the ending coordinates of the word.

The function `find_vertical_words(crossword)` verify whether the orientation of the words in the crossword puzzle are vertical. The function is similar to the `find_horizontal_words(crossword)`, except that it iterate through each column of the puzzle to locate the words.

```python
def find_horizontal_words(crossword):
	horizontal_words = []
	for row in range(len(crossword)):		
		column = 0
		word = Word()
		finished = False
		prev = '#' #prev mean the previous char in the word

		while column <= len(crossword[row])-1:			
			if crossword[row][column] == '0':				
				if prev == '0':
					word.length += 1
					prev = '0'
					if column == len(crossword[row])-1:
						if not finished:
							finished = True
						word.end_coord = (row, column)
						prev = '#'

				elif prev == "#":
					if finished:
						finished = False
					word.start_coord = (row, column)
					word.length += 1
					prev = '0'

			elif crossword[row][column] == '#':
				if prev == '0':
					if not finished:
						finished = True
					if word.length > 1:
						word.end_coord = (row, column-1)
					else:
						word = Word()
					prev = '#'

			if word.length > 1 and finished:
				word.orientation = 0
				horizontal_words.append(word)
				word = Word()
				finished = False	

			column += 1		
	return horizontal_words

def find_vertical_words(crossword):
	vertical_words = []
	word = Word()
	started = False
	
	for column in range(0, len(crossword[0])):
		started = False
		for row in range(0, len(crossword)-1):
			if crossword[row][column] == '0' and crossword[row+1][column] == '0':
				if started == False:
					started = True
					word.start_coord = (row, column)
				
				if row == len(crossword)-2 and started:
					word.end_coord = (row+1, column)
					word.length = word.end_coord[0] - word.start_coord[0] + 1
					word.orientation = 1
					vertical_words.append(word)
					word = Word()
					started = False
			else:
				if started:
					word.end_coord = (row, column)
					word.length = word.end_coord[0] - word.start_coord[0] + 1
					word.orientation = 1
					vertical_words.append(word)
					word = Word()
					started = False
	return vertical_words
```
Next we defined the backtracking algorithm function. The function take in three variable, `assigned_variable_list`, `not_assigned_variable_list` and `dict`. The `not_assigned_variable_list` consist of all the horizontal and vertical words pending to be filled in the crossword. The `dict` variable is the value returned by the load_dictionary() function. The `assigned_variable_list` hold all the values that satisfy the constraint in the crossword.

Next, the `get_possible_values()` is called to return all possible words(values) that fit the word length of the crossword. The `check_constraint()` function is called to ensure the value assigned satisfy the constraint of the crossword.

If all possible values are unable to satisfy the constraint, this meant that the previous "word" assigned was wrong. The algorithm will then backtrack and leave the word cells unassigned to try another possibilities.

```python
def backtracking(assigned_variable_list, not_assigned_variable_list, dict):

	#there are no variables to assign a value so we are done
	if len(not_assigned_variable_list) == 0:
		return assigned_variable_list

	var = not_assigned_variable_list[0]	
	possible_val = get_possible_values(var, assigned_variable_list, dict)

	for val in possible_val:
		# we create the variable check_var to do the checking and avoid assigning values which do not comply with the constraint
		check_var = copy.deepcopy(var)
		check_var.value = val
		if check_constraint(check_var, assigned_variable_list):
			var.value = val
			result = backtracking(assigned_variable_list+[var], not_assigned_variable_list[1:], dict)
			if result != None:
				return result
            # we've reached here because the choice we made by putting some 'word' here was wrong 
            # hence now leave the word cell unassigned to try another possibilities 
			var.value = ''
	return None
```

The `get_possible_values` function is to go through the dictionary and return all possible value that fit the word length of the crossword that we are solving. The function also check values already assigned to the crossword to avoid duplication

```python
#returns all possible values for the desired variable
def get_possible_values(var, assigned_variable_list, dict):
	possibles_values = []
	for val in dict:
		if len(val) == var.length:
			possibles_values.append(val)
	
	for item in assigned_variable_list:
		if item.value in possibles_values:
			possibles_values.remove(item.value)
	return possibles_values
```

The function `check_constraint()` check the proposed value to be assigned (`var`) against the `assigned_variable_list`. If they are the same orientation (both are horizontal or both are vertical), no further check is needed. If otherwise (one is horizontal and the other is vertical), the `check_intersections()` will be called to help check for the intersection point (if any).

The `check_intersections()` make use of the `LineString` function from the `shapely` module which we imported earlier. We treat the words as if they were lines and use this idea to find the intersection point of the horizontal and vertical words. This intersection point is the constraint which the algorithm must apply to find a valid solution.

```python
#checks var against assigned variable list
def check_constraint(var, assigned_variable_list):
	if assigned_variable_list != None:
		for word in assigned_variable_list:
			#if orientation is equal they will never intersect
			if var.orientation != word.orientation:
				intersection = check_intersections(var, word)
				if len(intersection) != 0:
					if var.orientation == 0: #horizontal 
						if var.value[int(intersection[0][1]-var.start_coord[1])] != word.value[int(intersection[0][0]-word.start_coord[0])]:          
							return False
					else: #vertical 
						if var.value[int(intersection[0][0]-var.start_coord[0])] != word.value[int(intersection[0][1]-word.start_coord[1])]:          
							return False
	return True

# determine the constraint which the algorithm must apply to get a valid solution
def check_intersections(w1, w2):
	line1 = LineString([w1.start_coord, w1.end_coord])
	line2 = LineString([w2.start_coord, w2.end_coord])
	intersection_point = line1.intersection(line2)
	if not intersection_point.is_empty:
		return [intersection_point.coords[0]] #result(float)
	else:
		return []
```

The function `insert_word_to_puzzle()` will now insert the word found by our solver into the crossword base on their orientation, starting coordinates and ending coordinates.

```python
def insert_word_to_puzzle(crossword, word, coord, orientation):
	pos_count = 0
	for char in word:
		if orientation == 0: #horizontal if orientation == 0
			crossword[coord[0]][coord[1]+pos_count] = char
		else:
			crossword[coord[0]+pos_count][coord[1]] = char
		pos_count += 1
	return crossword
```

The below codes will attempt to run our crossword solver. `puzzle.txt` is our crossword layout we are trying to solve. An example of the layout is as follow, `#` represent the dark shaded area of the crossword and `0` represent the word cell to be filled.

```
#	0	#	#	#	#
#	0	#	#	#	#
#	0	#	#	#	0
#	0	0	0	0	0
#	0	#	#	#	0
#	0	#	#	#	0
```

The file `words.txt` is a list of word separated by a new line for our solver to use as possible values to fill in the word cells. An example is as follow

```
ability
able
about
above
accept
according
```
```python
cw_puzzle = load_crossword_puzzle("puzzle.txt")
dict = load_dictionary("words.txt")
horizontal_word = find_horizontal_words(cw_puzzle)
vertical_word = find_vertical_words(cw_puzzle)
total_words = horizontal_word + vertical_word
assign_var_list = []
suggested_solution = backtracking(assign_var_list, total_words, dict)

print("---------- Crossword ---------")
for line in cw_puzzle:
	print(line)
print("------------------------------")

print("---------- Solution ----------")

if suggested_solution is None:
	print("No solution found")
else: 
	for word in suggested_solution:
		insert_word_to_puzzle(cw_puzzle, word.value, word.start_coord, word.orientation)

	for line in cw_puzzle:
		print(line)

print("------------------------------")
```

Once we run the code, we should see the following output:

```
---------- Crossword ---------
['#', '0', '#', '#', '#', '#']
['#', '0', '#', '#', '#', '#']
['#', '0', '#', '#', '#', '0']
['#', '0', '0', '0', '0', '0']
['#', '0', '#', '#', '#', '0']
['#', '0', '#', '#', '#', '0']
------------------------------
---------- Solution ----------
['#', 'a', '#', '#', '#', '#']
['#', 'l', '#', '#', '#', '#']
['#', 'w', '#', '#', '#', 'i']
['#', 'a', 'b', 'o', 'u', 't']
['#', 'y', '#', '#', '#', 'e']
['#', 's', '#', '#', '#', 'm']
------------------------------
```

## Time and Space Complexity
###### Time Complexity

O((M * P)^D): Each continuous cells will have only one word so D+1 words will be used. At each intersection, we need to check P words. M is the average length of word and this is needed as we need to check constraints (reading time of a string).

###### Space Complexity
O(L) : where L is the length of the given word. This space is used for recursion stack.

## Conclusion
Other ways to solve crossword include:

- Forward Checking
- Dynamic Variable Ordering
- Conflict-Directed Backjumping
- Arc Consistency

This article was originally published at OpenGenus:

[link to my article at OpenGenus](https://iq.opengenus.org/crossword-using-backtracking/ "my article at OpenGenus")