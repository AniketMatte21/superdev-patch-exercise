Project Setup:
Setup Frontend in VS code, and ran on localhost:5173
Setup backend in Intellij Idea, and ran on localhost:8080

Bugs/Problems found:

1) The major bug in my opinion found was the search filter
   --> The search supposed to be filtered by archieved=FALSE, by "term" in title/description and by STATUS(drop down options)
   --> But the problem was in the Spring Boot native query in TaskRepository.java file from line number 14 to 17.

   ##Root Cause:
     --> In SQL AND precadance is higher than OR, the second condition i.e title/description was spilt that we didn't wanted.

   ##Fixing:
     --> Added paranthesis and merged title and description in single condition, so the search query will correctly works with title/description and as well as status.

   ## AI tool used:
     --> Chatgpt: for understanding the incorrect query and right approach to these implementation.

3) The second bug I found was the delay in the search while querying.

    ##Root cause:
     --> The thread implmentation was added in TaskController.java file that made each query by some delays, That looks unwanted.
     --> The shorter query takes long delay while the longer will take shorter delay.
     --> Also these affects the unwanted server threads occupied
     --> Throughput problem

   ## fixing:
     --> Removed the thread code to fix the user search exp, to solve the throughput problem

   ## AI tool used:
     --> Chatgpt: used to find whether any other implmentation works. But suggested to remove.

4) Found the problem in React Application

     --> In filename: useTasks.js, line number 7, the useState hook loading was initialized with false.
     --> It was set true in useEffect() function, but then the user remains stuck on loading state only.

     ## Root Cause:
     --> setLoading(false) was never set even after error.

     ## fixing:
     --> added finally block and set setLoading(true)

5) Small improvements like .env could be add for passing the URL, also can add Lombok dependency in Task.java file.



   

   



