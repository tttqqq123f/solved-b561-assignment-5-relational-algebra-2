Download Link: https://assignmentchef.com/product/solved-b561-assignment-5-relational-algebra-2
<br>



For this assignment, you will need to submit 3 files. The first file is a .sql file that should contain all the SQL code relating to problems requesting the development of such code. The second file is a .txt file that should contain the output of the queries in the problems in Part 2. The third file is a .pdf file that should contain your solutions for problems where RA expressions in their standard (i.e., non SQL) notation are requested. Ideally you should use latex to construct this .pdf file. Latex offers a convenient syntax to formulate RA expessions.

Before you solve the problems in this section, we briefly review how you can express RA expressions in SQL in a way that closely mimics their RA specifications. (For more detail, consult the lectures relating to RA and joins.)

Consider a relation <em>R</em>(<em>A,B</em>) and a relation <em>S</em>(<em>C</em>) and consider the following RA expression <em>F</em>: <em>π</em><em>A</em>(<em>R</em>) − <em>π</em><em>A</em>(<em>σ</em><em>B</em>=1(<em>R ./</em><em>B</em>=<em>C </em><em>S</em>))

Then we can write this query in SQL in a variety of ways that closely mimics its RA formulation. One way to write this RA expression in SQL is as follows:

SELECT DISTINCT A FROM   R EXCEPT SELECT A FROM   (SELECT DISTINCT A, B, C FROM         R JOIN S ON (B = C) WHERE A = 1) q

An alternative way to write this query is to use the WITH statement of SQL.<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>To do this, we separate the RA expression <em>F </em>into sub-expressions as follows. (In this case, notice that each sub-expression corresponds to the application of a single RA operation. More generally, one can of course use sub-expressions that can contain multiple RA operations.)

Expression Name       RA expression

<em>E</em><sub>1   </sub><em>π<sub>A</sub></em>(<em>R</em>) <em>E</em>2 <em>R ./</em><em>B</em>=<em>C </em><em>S</em>

<em>E</em><sub>3   </sub><em>σ<sub>B</sub></em>=1(<em>E</em><sub>2</sub>) <em>E</em>4                   <em>π</em><em>A</em>(<em>E</em>3)

<h2>                                           <em>F                                      E</em><sub>1 </sub>− <em>E</em><sub>4</sub></h2>

Then we write the following SQL query. Notice how the expressions <em>E</em>1, <em>E</em>2, <em>E</em>3, and <em>E</em>4 occur as separate queries in the WITH statement and that the final query gives the result for the expression <em>F</em>.<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a>

<h1>WITH E1 AS (SELECT DISTINCT A FROM R), E2 AS (SELECT DISTINCT A, B, C FROM (R JOIN S ON (B = C)) e2), E3 AS (SELECT A, B, C FROM E2 WHERE B = 1), E4 AS (SELECT DISTINCT A FROM E3) (SELECT A FROM E1) EXCEPT (SELECT A FROM E4);</h1>

In your answer to a problem, you may write the resulting RA expression with or without the WITH statement. (Your SQL query should of course closely resemble the RA expression it is aimed to express.)

<h1>1           Theoretical Problems about RA</h1>

<ol>

 <li>(a) Consult the lecture on set joins and semijoins. Using the techniques described in that lecture, develop a general RA expression for the “all-but-two” set semijoin.

  <ul>

   <li>Apply this RA expression to the query “Find the bookno and title of each book that is bought by all but two students who major in ‘CS’.</li>

   <li>Formulate the RA expression obtained in Problem 1b in SQL with relational operators. (So no SQL set predicates are allowed in your solution.)</li>

  </ul></li>

 <li>Consider two RA expressions <em>E</em><sub>1 </sub>and <em>E</em><sub>2 </sub>over the same schema. Furthermore, consider an RA expression <em>F </em>with a schema that is not necessarily the same as that of <em>E</em><sub>1 </sub>and <em>E</em><sub>2</sub>.</li>

</ol>

Consider the following if-then-else query:

<h2>if <em>F </em>6= ∅ thenreturn <em>E</em><sub>1 </sub>else return <em>E</em><sub>2</sub></h2>

So this query evaluates to the expression <em>E</em><sub>1 </sub>if <em>F </em>6= ∅ and to the expression <em>E</em><sub>2 </sub>if <em>F </em>= ∅.

We can formulate this query in SQL as follows<a href="#_ftn3" name="_ftnref3"><sup>[3]</sup></a>:

<h2>select e1.* from        E1 e1 where exists (select 1 from F) union select e2.* from      E2 e1 where not exists (select 1 from F);</h2>

<ul>

 <li>Write an RA expression, in function of <em>E</em><sub>1</sub>, <em>E</em><sub>2</sub>, and <em>F</em>, that expresses this if-then-else statement.</li>

</ul>

<ol>

 <li>Then express this RA expression in SQL with RA operators. In particular, you can not use SQL set predicates in your solution.</li>

</ol>

<ul>

 <li>Let A(x) be a unary relation that can store a set of integers <em>A</em>. Consider the following boolean SQL query:</li>

</ul>

select exists(select 1 from A) as A isNotEmpty;

This boolean query returns the constant “true” if <em>A </em>6= ∅ and returns the constant “false” otherwise. Using the insights you gained from Problem 2a, solve the following problems:

<ol>

 <li>Write an RA expression that expresses the above boolean SQL query.</li>

</ol>

Hint: recall that, in general, a constant value “<strong>a</strong>” can be represented in RA by an expression of the form (C: <strong>a</strong>). (Here, C is some arbitrary attribute name.) Furthermore, recall that we can express (C: <strong>a</strong>) in SQL as “select <strong>a </strong>as C”. Thus RA expressions for the constants “true” and “false” can be the expressions (C: true) and (C: false), respectively.

<ol>

 <li>Write a SQL query with relational operators, thus without set predicates, that expresses the above boolean SQL query.</li>

</ol>

<ol start="3">

 <li>Let <em>f </em>: <em>A </em>→ <em>B </em>be a function from a set <em>A </em>to a set <em>B </em>and let <em>g </em>: <em>B </em>→ <em>C </em>be a function from a set <em>B </em>to a set <em>C</em>. The <em>composition </em>of the functions <em>f </em>and <em>g</em>, denoted <em>g </em>◦ <em>f</em>, is a function from <em>A </em>to <em>C </em>such that for <em>x </em>∈ <em>A</em>, <em>g </em>◦ <em>f</em>(<em>x</em>) is defined as the value <em>g</em>(<em>f</em>(<em>x</em>)).</li>

</ol>

Represent <em>f </em>in a binary relation f with schema (A,B) and represent <em>g </em>in a binary relation g with schema (B,C).

<ul>

 <li>Write an RA expression that computes the function <em>g </em>◦ <em>f</em>. I.e., your expression should compute the binary relation {(<em>x,g</em>◦<em>f</em>(<em>x</em>)) | <em>x </em>∈ <em>A</em>}.</li>

 <li>Let <em>y </em>be a value in <em>C</em>. Write an RA expression that computes the set {<em>x </em>∈ <em>A </em>| <em>g </em>◦ <em>f</em>(<em>x</em>) = <em>y</em>}. I.e., these are the values in <em>A </em>that are mapped by the function <em>g </em>◦ <em>f </em>to the value <em>y</em>.</li>

</ul>

<ol start="4">

 <li>Let <em>f </em>: <em>A </em>→ <em>B </em>be a function from a set <em>A </em>to a set <em>B</em>. We say that <em>f </em>is a <em>one-to-one </em>function if for each pair <em>x</em><sub>1 </sub>and <em>x</em><sub>2 </sub>of different values in <em>A </em>(i.e., <em>x</em><sub>1 </sub>6= <em>x</em><sub>2</sub>) it is the case that <em>f</em>(<em>x</em><sub>1</sub>) 6= <em>f</em>(<em>x</em><sub>2</sub>). Represent <em>f </em>by a relation f with schema (A,B).</li>

</ol>

Write an RA expression that returns the value “true” if <em>f </em>(as stored in f) is a one-one-one function, and returns the value “false” otherwise.

<ol start="5">

 <li>Let <em>f </em>: <em>A </em>→ <em>B </em>be a function from a set <em>A </em>to a set <em>B</em>. We say that <em>f </em>is an <em>onto </em>function if for each value <em>y </em>in <em>B</em>, there exists a value <em>x </em>in <em>A </em>such that <em>f</em>(<em>x</em>) = <em>y</em>. Represent <em>f </em>by a relation f with schema (A,B).</li>

</ol>

Write an RA expression that returns the value “true” if <em>f </em>(as stored in f) is an onto function, and returns the value “false” otherwise.

<ol start="6">

 <li>A <em>graph G </em>is a structure (<em>V,E</em>) where <em>V </em>is a set of vertices and wherein <em>E </em>is a set of edges between these vertices. Thus <em>E </em>⊆ <em>V </em>× <em>V </em>.</li>

</ol>

A <em>path </em>in <em>G </em>is a sequence of vertices (<em>v</em><sub>0</sub><em>,v</em><sub>1</sub><em>,…,v<sub>n</sub></em>) such that for each <em>i </em>∈ [0<em>,n </em>− 1], (<em>v<sub>i</sub>,v<sub>i</sub></em><sub>+1</sub>) ∈ <em>E</em>. We call <em>n </em>the <em>length </em>of this path.

Represent <em>E </em>by a binary relation E(source,target). A pair (<em>s,t</em>) is in E if <em>s </em>and <em>t </em>are vertices in <em>V </em>and (<em>s,t</em>) is an edge in <em>E</em>. Think of <em>s </em>as the source of this edge and <em>t </em>as the target of this edge.

We say that two vertices <em>v </em>and <em>w </em>in <em>V </em>are connected in <em>G </em>by a path of length <em>n </em>if there exists a path (<em>v</em><sub>0</sub><em>,v</em><sub>1</sub><em>,…,v<sub>n</sub></em>) such that <em>v </em>= <em>v</em><sub>0 </sub>and <em>w </em>= <em>v<sub>n</sub></em>.

Write an RA expression that returns the set of pairs (<em>v,w</em>) that are connected by a path of length at most <em>n</em>. (You may assume that <em>n </em>≥ 1.)

<h1>2           Formulating Queries in RA</h1>

In the following problems, we will use the data that you can find in the data.sql file provided for these problems.

Write the following queries as RA expressions in the standard RA notation. Submit these queries in a .pdf document. In these expressions, you can use the following notations for the relations:

<table width="0">

 <tbody>

  <tr>

   <td width="61">Student</td>

   <td width="93"><em>S</em>, <em>S</em><sub>1</sub>, <em>S</em><sub>2</sub>, etc</td>

  </tr>

  <tr>

   <td width="61">Book</td>

   <td width="93"><em>B</em>, <em>B</em><sub>1</sub>, <em>B</em><sub>2 </sub>etc</td>

  </tr>

  <tr>

   <td width="61">Cites</td>

   <td width="93"><em>C</em>, <em>C</em><sub>1</sub>, <em>C</em><sub>2 </sub>etc</td>

  </tr>

  <tr>

   <td width="61">Major</td>

   <td width="93"><em>M</em>, <em>M</em><sub>1</sub>, <em>M</em><sub>2</sub>, etc</td>

  </tr>

  <tr>

   <td width="61">Buys</td>

   <td width="93"><em>T</em>, <em>T</em><sub>1</sub>, <em>T</em><sub>2</sub>, etc</td>

  </tr>

 </tbody>

</table>

Then, for each such RA expression, write a SQL query (possibly using the WITH statement) that mimics this expression as discussed above. Submit these queries in a .sql file as usual.

Furthermore, and where possible, avoid using × or CROSS JOIN operator. In addition, where possible, using semijoin operations instead of join operations.

Each of the problem relates back to a problem in Assignment 2. You can consult the SQL solutions for these problem as they may help you in formulating the queries as RA expressions.

<ol start="7">

 <li>Find the sid and name of each student who majors in CS and who bought a book that cost more than $10. (Assignment 2, Problem 1.)</li>

 <li>Find the bookno, title, and price of each book that cites at least two books that cost less than $60. (Assignment 2, Problem 3.)</li>

 <li>Find the bookno, title, and price of each book that was not bought by any Math student. (Assignment 2, Problem 2.)</li>

 <li>Find the sid and name of each student along with the title and price of the most expensive book(s) bought by that student. (Assignment 2, Problem</li>

</ol>

4.)

<ol start="11">

 <li>Find the booknos and titles of books with the next to highest price. (Assignment 2, Problem 6.)</li>

 <li>Find the bookno, title, and price of each book that cites a book which is not among the most expensive books. (Assignment 2, Problem 7.)</li>

 <li>Find the sid and name of each student who has a single major and such that none of the book(s) bought by that student cost less than $40. (Assignment 2, Problem 8.)</li>

 <li>Find the bookno and title of each book that is bought by all students who major in both “CS” and in “Math”. (Assignment 2, Problem 9.)</li>

 <li>Find the sid and name of each student who, if he or she bought a book that cost at least than $70, also bought a book that cost less than $30. (Assignment 2, Problem 10.)</li>

 <li>Find each pair (s1, s2) where s1 and s2 are the sids of students who have a common major but who did not buy the same set of books. (Assignment 2, Problem 11.)</li>

</ol>

<a href="#_ftnref1" name="_ftn1">[1]</a> This is especially convenient when the RA expression is long and complicated.

<a href="#_ftnref2" name="_ftn2">[2]</a> For better readability, I have used relational-name overloading. Sometimes, you may need to introduce new attribute names in SELECT clauses using the AS clause. Also, use DISTINCT were needed.

<a href="#_ftnref3" name="_ftn3">[3]</a> In this SQL query E1, E2, and F denote SQL queries corresponding to the RA expressions <em>E</em><sub>1</sub>, <em>E</em><sub>2</sub>, and <em>F</em>, respectively.