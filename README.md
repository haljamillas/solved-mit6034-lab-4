Download Link: https://assignmentchef.com/product/solved-mit6_034-lab-4
<br>
To work on this problem set, you will need to get the code:

This lab has two parts; the first part is on CSPs and the second part is on learning algorithms, specifically KNN and decision trees.

<h1>Constraint Satisfaction Problems</h1>

In this portion of Lab 4, you are to complete the implementation of a general constraint satisfaction problem solver. You’ll test it on problems we’ve worked out by hand in class.

We have provided you a basic CSP implementation in csp.py. The implementation has the Depthfirst-search already completed. It even has a basic built in constraint checker. So it will produce the search trees of the kind for DFS w/ back tracking with basic constraint checking.

However, it doesn’t do forward checking or forward checking + singleton propagation!

So your job is to complete:

forward_checking(state): and

forward_checking_prop_singleton(state):

in the file lab4.py. Here state is an instance of CSPState an object that keep track of the current variable assignments and domains. These functions are called by the Search algorithm at every node in the search tree. These functions should return False at points at which the Domain Reduction Algorithm would backtrack, and True otherwise (i.e. continue extending).

As a hint, here is the (unrefined) pseudocode for the two algorithms.

<h1>Forward Checking</h1>

<ol>

 <li>Let X be the variable currently being assigned.</li>

 <li>Let x be the value being assigned to X.</li>

 <li>Find all the binary constraints that are associated with X.</li>

 <li>For each constraint:

  <ol>

   <li>Let Y be the variable connected to X by that binary constraint.</li>

   <li>For each variable value y in Y’s domain</li>

   <li>If constraint checking fails for X=x and Y=y

    <ol>

     <li>Remove y from Y’s domain</li>

     <li>If the domain of Y is reduced down to the empty set, then the entire check fails:</li>

    </ol></li>

  </ol></li>

</ol>

return False.

<ol start="5">

 <li>If all constraints passed declare success, return True</li>

</ol>

If you get a state with no current variable assignment (at the Root of the search tree) then you should just return True, since forward checking could only be applied when there is some variable assignment.

<h1>Forward Checking with Propagation through Singletons</h1>

<ol>

 <li>Run forward checking, fail if forward checking fails.</li>

 <li>Find variables with domains of size 1.</li>

 <li>Create a queue of singleton variables.</li>

 <li>While singleton queue is not empty

  <ol>

   <li>Pop off the first singleton variable X (add X to list of visited singletons)</li>

   <li>Find all the binary constraints that singleton X is associated with.</li>

   <li>For each constraint therein:</li>

   <li>Let Y be the variable connected to X by that binary constraint:</li>

   <li>For each value of y in Y’s domain:</li>

   <li>If constraint check fails for X = (X’s singleton value) and Y = y:

    <ol>

     <li>Remove y from Y’s domain</li>

     <li>If the domain of Y is reduced down to the empty set, then the entire check fails, return False.</li>

    </ol></li>

   <li>Check to see if domain reduction produced any new and unvisited singletons; if so, add them to the queue.</li>

  </ol></li>

 <li>return True.</li>

</ol>

<h1>API</h1>

These are some useful functions defined in csp.py that you should use in your code to implement the above algorithms:

CSPState: representation of one of the many possible search states in the CSP problem.

<ul>

 <li>get_current_variable() – gets the Variable instance being currently assigned. Returns None if we are in the root state, when there are no variable assignments yet.</li>

 <li>get_constraints_by_name(variable_name) – retrieves all the BinaryConstraint objects associated with variable_name.</li>

 <li>get_variable_by_name(variable_name) – retrieves the Variable object associated with variable_name.</li>

 <li>get_all_variables() – gets the list of all Variable objects in this CSP problem.</li>

</ul>

Variable: representation of a variable in these problems.

<ul>

 <li>get_name() – returns the name of this variable.</li>

 <li>get_assigned_value() – returns the <strong>assigned</strong> value of this variable. <strong>Returns None if </strong><strong>is_assigned() returns False, that is if the variable hasn’t been assigned yet.</strong></li>

 <li>is_assigned() – returns True if we’ve made an assignment for this variable.</li>

 <li>get_domain() – returns a copy of the list of the current domain of this variable. Use this to iterate over values of Y.</li>

</ul>

<strong>You might want to consider using this method to get the singular value of a variable with domain size reduced to 1.</strong>

<ul>

 <li>reduce_domain(value) – remove value from this variable’s domain.  domain_size() – returns the size of this variable’s domain  BinaryConstraint: a binary constraint on variable i, j: i -&gt; j.</li>

 <li>get_variable_i_name() – name of the i variable</li>

 <li>get_variable_j_name() – name of the j variable</li>

 <li>check(state, value_i=value, value_j=value) – checks the binary constraint for a given CSP state, with variable i set by value i, and variable j set by value j. Returns False if the constraint fails. Raises an exception if value_i or value_j are not set or cannot be inferred from state.</li>

</ul>

NOTE: in our implementation of CSPs, constraints are symmetrical; a constraint object exists for each “direction” of a constraint, so you can check for the presence of a constraint by substituting for i and/or j in the most convenient fashion for you.

Here is how you might use the API to get the value of a variable currently being assigned.

var = state.get_current_variable() value = None

if var is not None:   # we are not in the root state    value = var.get_assigned_value()

# Here value is the value of the variable current being assigned.

Here is how you might use the API to get the singular value from a singleton variable:

if singleton_var.domain_size() == 1    value = singleton_var.get_domain()[0]

<h1>Testing</h1>

For unit testing, we have provided moose_csp.py, an implementation of the seating problem involving a Moose, Palin, McCain, Obama, Biden and You — in terms of the framework as defined in csp.py.

Running:

python moose_csp.py dfs

will return the search tree for DFS with constraint checking. When you have finished your

implementation, running python moose_csp.py fc or python moose_csp.py fcps should return the correct search trees under forward checking and forward checking with singleton propagation.  Similarly  Running:

python map_coloring_csp.py [dfs|fc|fcps]

Should return the expected search trees for the B,Y,R, state coloring problem from the 2nd Quiz in 2006.

There are also other fun solved CSP problems in the directory that you can test and play around with. You can submit your own unique solution to an interesting CSP problem to get extra credit!

<h1>EXTRA CREDIT</h1>

As extra credit, try to follow the code in moose_csp.py or map_coloring_csp.py, and implement a problem() function that returns a CSP instance for a problem of your own choosing.

You may do one of the problems from past quizzes: the 2009 Time Traveler scheduling problem or the 2010 Jigsaw puzzle question. Alternately, you may implement something that you find useful or interesting, ideas include: scheduling classes, seating guests for a wedding or dinner party (to maximize harmony), solving crypt-arithmetic puzzles, the 8-queens problem, or crossword puzzles.

You may also try to extend csp.py. For instance, you can add ability to find an optimal solution rather than just a constraint-satisfying solution (i.e. replace DFS with one of the optimal searches we’ve learned). Or you can add support for multi-variable constraints, and make the code solve the Max-flow problem from the 2006 final.

When you’ve succeeded in implementing such a problem or extension, send your working code to the 6.034 staff. Your reward: either a 1-to-3-day extension (depending on difficulty) on one of the previous or future labs, possibly erasing any late penalties. Or if your lab grade is already perfect, praise and recognition from the 6.034 staff.

<strong>Learning  </strong>

Now for something completely different: learning!

<h1>Classifying Congress</h1>

During Obama’s visit to MIT, you got a chance to impress him with your analytical thinking. Now, he has hired you to do some political modeling for him. He seems to surround himself with smart people that way.

He takes a moment out of his busy day to explain what you need to do. “I need a better way to tell which of my plans are going to be supported by Congress,” he explains. “Do you think we can get a model of Democrats and Republicans in Congress, and which votes separate them the most?”

“Yes, we can!” You answer.

<h1>The Data</h1>

You acquire the data on how everyone in the previous Senate and House of Representatives voted on every issue. (These data are available in machine-readable form via voteview.com. We’ve included it in the lab directory, in the files beginning with H110 and S110.)  data_reader.py contains functions for reading data in this format.  read_congress_data(“FILENAME.ord”) reads a specially-formatted file that gives

information about each Congressperson and the votes they cast. It returns a list of dictionaries, one for each member of Congress, including the following items:

<ul>

 <li>‘name’: The name of the Congressperson.</li>

 <li>‘state’: The state they represent.</li>

 <li>‘party’: The party that they were elected under.</li>

 <li>‘votes’: The votes that they cast, as a list of numbers. 1 represents a “yea” vote, -1 represents “nay”, and 0 represents either that they abstained, were absent, or were not a member of Congress at the time.</li>

</ul>

To make sense of the votes, you will also need information about what they were voting on. This is provided by read_vote_data(“FILENAME.csv”), which returns a list of votes in the same order that they appear in the Congresspeople’s entries. Each vote is represented a dictionary of information, which you can convert into a readable string by running vote_info(vote).

The lab file reads in the provided data, storing them in the variables senate_people, senate_votes, house_people, and house_votes.

<h1>Nearest Neighbors</h1>

You decide to start by making a nearest-neighbors classifier that can tell Democrats apart from Republicans in the Senate.

We’ve provided a nearest_neighbors function that classifies data based on training data and a distance function. In particular, this is a third-order function:

<ul>

 <li>First, call nearest_neighbors(distance, k), with distance being the distance</li>

</ul>

function you wish to use and k being the number of neighbors to check. This returns a <em>classifier factory</em>.

<ul>

 <li>A classifier factory is a function that makes classifiers. You call it with some training data as an argument, and it returns a classifier.</li>

 <li>Finally, you call the classifier with a data point (here, a Congressperson) and it returns the classification as a string.</li>

</ul>

Much of this is handled by the evaluate(factory, group1, group2) function, which you can use to test the effectiveness of a classification strategy. You give it a classifier factory (as defined above) and two sets of data. It will train a classifier on one data set and test the results against the other, and then it will switch them and test again.

Given a list of data such as senate_people, you can divide it arbitrarily into two groups using the crosscheck_groups(data) function.

One way to measure the “distance” between Congresspeople is with the <em>Hamming distance</em>: the number of entries that differ. This function is provided as hamming_distance.

An example of putting this all together is provided in the lab code:

senate_group1, senate_group2 = crosscheck_groups(senate_people) evaluate(nearest_neighbors(edit_distance, 1), senate_group1, senate_group2, verbose=1)

Examine the results of this evaluation. In addition to the problems caused by independents, it’s classifying Senator Johnson from South Dakota as a Republican instead of a Democrat, mainly because he missed a lot of votes while he was being treated for cancer. This is a problem with the distance function — when one Senator votes yes and another is absent, that is less of a “disagreement” than when one votes yes and the other votes no.

You should address this. Euclidean distance is a reasonable measure for the distance between lists of discrete numeric features, and is the alternative to Hamming distance that you decide to try. Recall that the formula for Euclidean distance is:

<em>[(x1 – y1)^2 + (x2 – y2)^2 + … + (xn – yn)^2] ^ (1/2)</em>

<ul>

 <li>Make a distance function called euclidean_distance that treats the votes as highdimensional vectors, and returns the Euclidean distance between them.</li>

</ul>

When you evaluate using euclidean_distance, you should get better results, except that some people are being classified as Independents. Given that there are only 2 Independents in the Senate, you want to avoid classifying someone as an Independent just because they vote similarly to one of them.

<ul>

 <li>Make a simple change to the parameters of nearest_neighbors that accomplishes this, and call the classifier factory it outputs my_classifier.</li>

</ul>

<h1>ID Trees</h1>

So far you’ve classified Democrats and Republicans, but you haven’t created a model of which votes distinguish them. You want to make a classifier that explains the distinctions it makes, so you decide to use an ID-tree classifier.

idtree_maker(votes, disorder_metric) is a third-order function similar to nearest_neighbors. You initialize it by giving it a list of vote information (such as senate_votes or house_votes) and a function for calculating the disorder of two classes. It returns a classifier factory that will produce instances of the CongressIDTree class, defined in classify.py, to distinguish legislators based on their votes.

The possible decision boundaries used by CongressIDTree are, for each vote:

<ul>

 <li>Did this legislator vote YES on this vote, or not?</li>

 <li>Did this legislator vote NO on this vote, or not?</li>

</ul>

(These are different because it is possible for a legislator to abstain or be absent.)

You can also use CongressIDTree directly to make an ID tree over the entire data set.

If you print a CongressIDTree, then you get a text representation of the tree. Each level of the ID tree shows the minimum disorder it found, the criterion that gives this minimum disorder, and (marked with a +) the decision it makes for legislators who match the criterion, and (marked with a -) the decision for legislators who don’t. The decisions are either a party name or another ID tree. An example is shown in the section below.

<h1>An ID tree for the entire Senate</h1>

You start by making an ID tree for the entire Senate. This doesn’t leave you anything to test it on, but it will show you the votes that distinguish Republicans from Democrats the most quickly overall. You run this (which you can uncomment in your lab file):

print CongressIDTree(senate_people, senate_votes, homogeneous_disorder)

The ID tree you get here is:

Disorder: -49

Yes on S.Con.Res. 21: Kyl Amdt. No. 583; To reform the death tax by setting the

exemption at $5 million per estate, indexed for inflation, and the top death

tax rate at no more than 35% beginning in 2010; to avoid subjecting an

estimated 119,200 families, family businesses, and family farms to the death

tax each and every year; to promote continued economic growth and job creation; and to make the enhanced teacher deduction permanent.:

+ Republican

<ul>

 <li>Disorder: -44</li>

</ul>

Yes on H.R. 1585: Feingold Amdt. No. 2924; To safely redeploy United States   troops from Iraq.:   + Democrat

<ul>

 <li>Disorder: -3</li>

</ul>

No on H.R. 1495: Coburn Amdt. No. 1089; To prioritize Federal spending to

ensure the needs of Louisiana residents who lost their homes as a result of

Hurricane Katrina and Rita are met before spending money to design or     construct a nonessential visitors center.:

+ Democrat

<ul>

 <li>Disorder: -2</li>

</ul>

Yes on S.Res. 19: S. Res. 19; A resolution honoring President

Gerald

Rudolph Ford.:

+ Disorder: -4

Yes on H.R. 6: Motion to Waive C.B.A. re: Inhofe Amdt. No.

1666; To

ensure agricultural equity with respect to the renewable fuels standard.:         + Democrat

<ul>

 <li>Independent – Republican</li>

</ul>

Some things that you can observe from these results are:

<ul>

 <li>Senators like to write bills with very long-winded titles that make political points.</li>

 <li>The key issue that most clearly divided Democrats and Republicans was the issue that Democrats call the “estate tax” and Republicans call the “death tax”, with 49 Republicans voting to reform it.</li>

 <li>The next key issue involved 44 Democrats voting to redeploy troops from Iraq.</li>

 <li>The issues below that serve only to peel off homogenous groups of 2 to 4 people.</li>

</ul>

<h1>Implementing a better disorder metric</h1>

You should be able to reduce the depth and complexity of the tree, by changing the disorder metric from the one that looks for the largest homogeneous group to the information-theoretical metric described in lecture.

You can find this formula on page 429 of <a href="http://courses.csail.mit.edu/6.034f/ai3/ch21.pdf">the reading</a><a href="http://courses.csail.mit.edu/6.034f/ai3/ch21.pdf">.</a>

    Write the information_disorder(group1, group2) function to replace

homogeneous_disorder. This function takes in the lists of classifications that fall on each side of the decision boundary, and returns the information-theoretical disorder.

Example:

information_disorder([“Democrat”, “Democrat”, “Democrat”],

[“Republican”, “Republican”])

=&gt; 0.0

information_disorder([“Democrat”, “Republican”], [“Republican”,

“Democrat”])

=&gt; 1.0

Once this is written, you can try making a new CongressIDTree with it. (if you’re having trouble, keep in mind you should return a float or similar)

<h1>Evaluating over the House of Representatives</h1>

Now, you decide to evaluate how well ID trees do in the wild, weird world of the House of Representatives.

You can try running an ID tree on the entire House and all of its votes. It’s disappointing. The 110th

House began with a vote on the rules of order, where everyone present voted along straight party lines.

It’s not a very informative result to observe that Democrats think Democrats should make the rules and Republicans think Republicans should make the rules.

Anyway, since your task was to make a tool for classifying the newly-elected Congress, you’d like it to work after a relatively small number of votes. We’ve provided a function,

limited_house_classifier, which evaluates an ID tree classifier that uses only the most recent <em>N</em> votes in the House of Representatives. You just need to find a good value of <em>N</em>.

<ul>

 <li>Using limited_house_classifier, find a good number <em>N_1</em> of votes to take into account, so that the resulting ID trees classify at least 430 Congresspeople correctly. How many training examples (previous votes) does it take to predict at least 90 senators correctly? What about 95? <strong>To pass the online tests</strong>, you will need to find close to the minimum such values for N_1, N_2, and N_3. Keep guessing to find close to the minimum that will pass the offline tests.</li>

</ul>

Do the values surprise you? Is the house more unpredictable than the senate, or is it just bigger?

<ul>

 <li>Which is better at predicting the senate, 200 training samples, or 2000? Why?</li>

</ul>

The total number of Congresspeople in the evaluation may change, as people who didn’t vote in the last <em>N</em> votes (perhaps because they’re not in office anymore) aren’t included.

<h1>Survey</h1>

Please answer these questions at the bottom of your ps4.py file:

<ul>

 <li>How many hours did this problem set take?</li>

 <li>Which parts of this problem set, if any, did you find interesting?</li>

 <li>Which parts of this problem set, if any, did you find boring or tedious?</li>

</ul>

<h1>FAQs</h1>

<strong>Q:</strong> For the N’s for the limited_house_classifier, I got some (large) values, and it passed the offline tests, but it failed the online tests. If I subtract even 1 from the values, it doesn’t classify enough people correctly. What’s wrong?

<strong>A:</strong> The number of correct classifications is not a monotonic function of N. For example, if N_1 is 60, then 426 representatives are classified correctly, but if N_1 is 40, then 428 representatives are classified correctly. The online tests are not exactly the same as the offline tests. You will need to just try smaller values of N.

<strong>Q:</strong> My code passes the offline eval_test but not the online one.

<strong>A:</strong> Make sure you did the part of the lab where you adjust the nearest-neighbors parameters (for my_classifier) to avoid classifying too many people as Independents.







MIT OpenCourseWare <a href="http://ocw.mit.edu/">http://ocw.mit.edu</a>

6.034 Artificial Intelligence

Fall 2010

For information about citing these materials or our Terms of Use, visit: <a href="http://ocw.mit.edu/terms">http://ocw.mit.edu/terms</a><a href="http://ocw.mit.edu/terms">.</a>