# Pewlett Hackard Retirement Analysis

## Overview of Project
Using SQL relational databases to analyze all employee data at Pewlett-Hackard to determine the number of employees about to retire based on their age and filtered by title.  Based on the findings, propose solutions to the so-called coming "Silver Tsunami".

## Results
- We know from our complete employee data list, there are 240124 employees currently employed at Pewlett-Hackard.  Based on the data, we find that among all employees, there are 90398 that are at or above retirement age.  This is just over 37% of the workforce.
- When those of retiring age are filtered by the job title, we find that 57688 (nearly 64%) of the roles are senior roles, meaning many knoweldgeable staff members are likely to leave the company in the near future.

![Retiring Titles](https://github.com/rscalise88/Pewlett-Hackard-Analysis/blob/main/Images/retiring_titles.PNG)

- Though a small number of managers are set to retire, PH retains currently only retains one manager per department for a total of 9. A loss of 2 in this case is snignificant. It would also appear based on the naming convention that the Technique Leaders may be some kind of lower-level management role overseeing a team rather than a department.  in this case, the loss of their leadership to the tune of 4502 staff is likely to be felt as well.
- Based on our determination of eligibility for the proposed mentorship program, we can see that there are only a small handful of these current employees who would be eligible to pass their knowledge on to new and/or junior staff members.

![Mentors](https://github.com/rscalise88/Pewlett-Hackard-Analysis/blob/main/Images/mentors.PNG)

## Summary 
PH is facing a demographic crisis and if it intends to keep relatively stable employment figures and retain the same level of knowledge, more needs to be done to prevent the loss of talent.  The current proposed eligibility criteria used to determine who could take part in this mentoring program returned a very small list of candidates.  Of these 1549 candidates, it's highly unlikely that all, or even most, would agree to reduce their hours and continue working.  Emplyees will also obviously have the option to continue working, to retire as planned.  Further, not every employee is suited to teaching or mentoring, so having such a limited list is not beneficial. Using the proposed mentorship program as a springboard, we can slightly alter the code to not only account for employees born in 1965, but all those of reitrement age (ie 1962-1965) to expand the pool of potential candidates:

    SELECT DISTINCT ON(e.emp_no) e.emp_no,
    	e.first_name,
    	e.last_name,
  	  e.birth_date,
  	  de.from_date,
    	de.to_date,
    	t.title
    INTO mentorship_eligibility_1952
    FROM employees AS e
      	INNER JOIN dept_emp AS de
  	    	ON (e.emp_no = de.emp_no)
  	    INNER JOIN titles AS t
  		    ON (e.emp_no = t.emp_no)
    WHERE (e.birth_date BETWEEN '1962-01-01' AND '1965-12-31')
    AND (de.to_date = '9999-01-01')
    ORDER BY emp_no;

![1962 Mentors](https://github.com/rscalise88/Pewlett-Hackard-Analysis/blob/main/Images/all_mentors_1952.PNG)


However, this ropes in roughly 2/3 of the current retiring workforce, which could become costly and/or unwieldly.

I propose further refactoring the code to only draw from those who hold senior positions, ideally those among the staff who have the most to teach to new and low-level employees.  Based on the table created above, a simple filter is all that is needed to extract only those with senior positions:

    SELECT DISTINCT ON(me.emp_no) me.emp_no,
	    me.first_name,
	    me.last_name,
	    me.birth_date,
	    me.from_date,
	    me.to_date,
	    me.title
    INTO senior_mentorship_eligibility_1952
    FROM mentorship_eligibility_1952 AS me
    WHERE (me.title = 'Senior Engineer' OR me.title= 'Senior Staff')
    ORDER BY emp_no;

![Senior Staff - 1962 Mentors](https://github.com/rscalise88/Pewlett-Hackard-Analysis/blob/main/Images/senior_mentors_1952.PNG)

The larger pool of candidates helps to ensure that valuable knowledge held by the senior staff that do plan to leave soon and are willing to take on a part-time mentoring role to new and low-level employees is not lost when they retire.  

While this may potentially help address the lost of knowledge and talent at PH, it doesn't address the pending issue of leadership.  When the Technique Leaders and Managers currently set to retire make that choice, it would be beneficial to have an idea of who within the company would be a good candidate to fill those roles.  For the Technique Leaders especially, a similar mentorship program may be beneficial.

It would also be beneficial to look at data outside of the company to determine not just now to retain knoweldge within the company, but attract new talent.  Recent graduate data, upcoming job fairs, etc.
