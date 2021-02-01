# YACS-Mini-Yarn

Big data workloads consist of multiple jobs from different applications. These workloads are too large to run on a single machine. Therefore, they are run on clusters of
interconnected machines. A scheduling framework is used to manage and allocate the resources of the cluster. And one efficeint way is to build a framework for scheduling on
multiple machines. And what we implement here is YACS.
YACS – Yet Another Centralized Scheduling Framework, has one Master, workers which bind together to perform the scheduling tasks in the most efficient way.

# Design
The design of this framework is quite simple but effective.
 1. It has one Master/Driver processes and 3 worker processes mimicing 3 degenerate machines running on the same machine through different threads.
 2. Threading locks are used to prevent any kind of race conditions or dead locks.
 3. The master and the workers are connected through ports. Each worker operates on a different port.
 4. Each of the three workers have a certain number of slots available in them.
 5. A driver sends the queue of requests/tasks/jobs to be scheduled and the master schedules these queue of tasks using three different scheduling algorithms based on selection and they are:
  1. Round Robin Scheduling.
  2. Least Loaded Scheduling.
  3. Random Selection Scheduling.

  6. Both map and reduce tasks are scheduled and sent to workers.
  7. Master schedules the tasks using any one of the above algorithms and sends them to worker to update the time and the slots available.
  8. The worker acknowledge the connections and update the master through port connections.
  9. Log files are maintained to analyse the flow of tasks to and fro worker and master.



# YACS Coding
<h2>Folder Structure</h2>
 - src

    - Master.py
  
    - Worker.py
  
    - Analysis.py
    
    - config.json and requests.py 
  
-logs

    - log_RR.txt
  
    - log_LL.txt
  
    - log_RA.txt
  

<h2>Steps to run:</h2>

- Run the Master.py with command 
 ```python
    $ python3 Master.py config.json <Scheduling Algo(RR/LL/RA)>
  ```
 - Run three workers with the following commands on three different terminals
 ```python
    $ python3 Worker.py 4000 1
    $ python3 Worker.py 4001 2
    $ python3 Worker.py 4002 3
 ```
 - Then run the requests.py to generate task queue
 ```python
    $ python3 requests.py <no_of_requests>
 ```
- 3 log files are then created and Analysis.py helps in analysing them. To run it
 ```python
    $ python3 Analysis.py
 ```
 
 #The logs folder contains logs generated in our tests. U could as well use them to process the analysis.py.
