# Assignment five Java CompletableFuture (7%)

**Due date Tuesday 19 April at 11:30 pm**

## Instructions

* Fork the project by clicking the "Fork" button on the top-right corner.
* **Make sure that the visibility of your new repository (the one you just forked) is set to private.**
* You can obtain the URL of your forked project by clicking the "Clone" button (beside "Fork") and then the “copy to clipboard icon" at the right of the dropdown.
* Clone the new repository to your computer and open the project with IntelliJ, by using command line or IntelliJ interface.
  * Using the Git command line: You will need to install a Git client yourself if you are not using the lab machines. In a termial on your computer, clone the assignment one repository to your computer using the command “git clone `<url you copied>`”. Open the project into your IntelliJ workspace. (*File* / *Open* the project directory).
  * IntelliJ: Alternatively, you could get the project from GitLab repository via IntelliJ interface. From the menu bar, *Git* > *Clone* > *Copy* the url to Repository URL > *Clone* (Reference: Get a project from version control: https://www.jetbrains.com/help/idea/import-project-or-module-wizard.html)
* Commit your changes regularly, providing an informative commit message and using Git inside IntelliJ (Commit and Changes Tutorial: https://www.jetbrains.com/help/idea/commit-and-push-changes.html)

You are expected to make at least 10 commits with messages to explain what have changed. 10 out of 50 marks are allocated for this.

## Your tasks

The goal of this assignment is to learn how to construct concurrent dataflow (a directed acyclic graph) programs in Java, using *CompletableFuture* objects. 
Part of the program is given to you, and you complete the rest to make it a superannuatation/lifestyle calculator. The program accepts the full name of a person, and then estimates the super balance when the person retires, when the person will die, and how lavish or stingy the person's retirement lifestyle will be.

Your final program should implement the following dataflow graph.
<img src="https://elearn.waikato.ac.nz/pluginfile.php/3285599/mod_resource/content/1/mortality-dataflow.png" />

Run the three test methods in completablefuture_assignment.Mortality.main() and experiment with GET and JOIN methods. Your task is to implement the "query"" method to set up the dataflow graph shown above, with each dataflow node running as a CompletableFuture async task in a thread pool. Upon receiving its input(s), each dataflow node starts executing, and then outputs its result, which may trigger the following dataflow nodes to start executing...

Three Suppliers are implemented for you (PersonInfoSupplier, SuperannuatationStrategySupplier and RetirementAgeSupplier) that enable the program to asynchronously retrieve data for each dataflow node. The "fullnames" file (bundled in the project directory) contains some artificial names and personal details (i.e., gender and birth year). The calculateDeathAge, performance and displayResult are also implemented for you.

## Constructing the Dataflow Graph using CompletableFuture
Modify **Mortality.java** and use CompletableFuture objects and methods to implement the five dataflow objects, and link them together as shown in the above directed acyclic graph (DAG). The available data inputs and their meanings are:

* 'birth year:' the year you were born.
* 'sex:' is 'male' or 'female'.
* 'retirement age:' the (integer) age at which you plan to retire.
* 'start super age:' the (integer) age at which you plan to contributing some of your salary into a superannuation fund.
* 'contribution%:' the (integer) percentage of your salary that you will put into your super each year. This should include the employer contribution.
* 'super strategy:' the name (a string) of the superannuation investment strategy that you wish to use. This can be 'growth' (the highest risk), 'balanced' (medium risk), 'conservative' (low risk) or 'cash' (bank interest only). Any other strategy values are invalid and should throw an exception.

Here are the specifications of the calculation that each dataflow object should perform.

* DeathAge: outputs the expected age that you will die, based on average lifespans.
* WorkingYears: outputs the 'retirement age' minus the 'start work age'.
* RetirementYears: outputs the DeathAge result minus 'retirement age'.
* SuperBalance: iterates WorkingYears times, calculating balance = balance * performance(strategy) + contribution / 100.0. The initial balance is zero. Note that this calculation gives us a multiple of your salary, so 4.2 actually means 4.2 times your average salary, which might be $420K if your average (inflation-adjusted) salary is $100K.
* LifeStyle: outputs your SuperBalance result divided by the number of years of retirement. This is a proportion of your annual salary, so a number like 0.75 means 3/4 of your normal lifestyle. The output message displays this as a percentage (75.0%), so you should multiply your result by 100.0 in the System.out.format message.


The performance of the super fund depends on the global economy, the local stock market, and many other factors. However, we will assume that on average, your super fund will achieve the following performance.

|Strategy Name|	Investment Returns (above inflation)|
|------|----|
|growth |4.5%|
|balanced|3.5%|
|conservative|2.5%|
|cash|1.0%|

Test your program to make sure it works correctly. Each dataflow node should start executing and output its result when it sees its inputs arrive. That is, for example, DeathAge and WorkingYears are calculated asynchronously, and their outputs could be in a different order each time your program runs. The same happens for the calculation of RetirementYears and Superbalance, and so on. Here is one sample output of the name "Mia Collins", with input data indented.

```
Mia Collins
	start super age=31
	retirement age=61
Working years=30
	super strategy==balanced
	contribution%=7
Super payout=3.6 (median salaries)
	birth year=1969
	sex=female
Die at 84
Retirement years=23
You live on 15.7% of median salary
Miserable poverty... 
```
The marks for this assignment will be based on the correct calculation and good use of CompletableFuture methods. Make sure the data from suppliers are retrieved asynchronously (non-blocking) and some dataflow objects that can run in parallel (e.g. DeathAge and WorkingYear, etc.) calculate the output asynchronously when they see the input data.
Note: The output may vary for each run.

## Submission Checklist
* Make sure you have pushed all changes to the repository. Ensure that you can see your changes on GitLab before submission.

## Grading (49 marks) 

|Marks|Allocated to|
|-----|-------|
|9|At least ten informative Commit comments |
|40 |CompletableFuture implementation of the five dataflow objects (8 marks each) |


