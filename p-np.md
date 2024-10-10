# P = NP?

Question posed Stephen Cook in 1971: Are all NP problems also P problems?

* In other words, <mark style="color:red;">are all problems with efficiently verifiable solutions also efficiently solvable</mark>?
* Often stated as “Does P = NP?”



“I have heard it said, with a straight face, that a proof of P = NP would be important because it would let airlines schedule their flights better, or shipping companies pack more boxes in their trucks! \
If \[P = NP], then we could quickly find the smallest Boolean circuits that output (say) a table of historical stock market data, or the human genome.... It seems entirely conceivable that, by analyzing these circuits, we could make an easy fortune on Wall Street, or retrace evolution... For broadly speaking, that which we can compress we can understand, and that which we can understand we can predict. \
So if we could solve the general case—if knowing something was tantamount to knowing the shortest efficient description of it—then we would be almost like gods. \[Assuming P ≠ NP] is the belief that such power will be forever beyond our reach.”\
\- Scott Aaronson [http://www.scottaaronson.com/papers/npcomplete.pdf](http://www.scottaaronson.com/papers/npcomplete.pdf)



Two important classes of yes/no problems:

* P: Efficiently solvable problems.
* NP: Problems with solutions that are efficiently verifiable.\*

Examples of problems in P:

* Is this array sorted?
* Does this array have duplicates?

Examples of problems in NP:

* Is there a solution to this 3SAT problem?
* In graph G, does there exist a path from s to t of weight > k?

Examples of problems not in NP:

* Is this the best chess move I can make next?\
  Hard to verify.
* What is the longest path?\
  Not a yes/no question.



