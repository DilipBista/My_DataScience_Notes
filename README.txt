## TODAY I LEARNED 

09-20-21: I we learned few things in SQL, they are Where command, Order By and Limit 



09-21-21: 

Try to think of your results as batches, sets, or pages. The first five results are your first page. The five after that would be your second page, etc. Update the query to find the tenth page of results.

LIMIT and OFFSET can be used to create multiple pages of data. What is the relationship between OFFSET (number of results to skip), LIMIT (number of results per page), and the page number?

SELECT  first_name, last_name, emp_no
FROM employees
WHERE hire_date LIKE '199%' 
AND birth_date LIKE '%-12-25'
ORDER BY hire_date
LIMIT 5 OFFSET 45;

###    GIT Clone 

1. Make a new repository on github called "now_i_know" or "my_data_science_notes"
- copy the SSH clone address
2. `cd ~/codeup-data-science` 
3. Clone your new repo with git git 
`git clone ` then paste that git clone address
4. `cd now_i_know` and do:
	- `git status` to double check that this is a repo
	- `git remote -v` to show the remotes
5. Move your `today_i_learned.txt` document that you made earlier this morning (or right now..)
6. `git add` the new file
7. `git commit` this new work
8. `git push` to push this work
- Oh btw: if you ever get a message about "there's no remote setup" and then the message gives you the code to push and set up the remote, copy + paste that code snippet