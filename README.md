
1. usecases
2. identify entities
3. write entities interfaces (strategies/services/engine/enums)
4. expand add fields with data type and complete method signatures
5. Add all files under the same package 


- User should be able to create task with scheduled, recurring , or run now giving the payload
- user should be able to cancel the scheduled/running task.
- User should be able to update the task with scheduled time or type of task.
- User should be able to view past runs and their status
- Tasks should run concurrently

Entities / Nouns

Task
TaskRun
TaskType

Classes 

class Task{
String id;
TaskType taskType;
LocalDateTime createdTime;
Long intervalTimeInMillis;
LocalDateTime startTime;
LocalDateTime endTime;
String payload;
}

TaskRun{
String id;
RunStatus runStatus;
LocalDateTime startTime;
LocalDateTime endTime;
Long lastRuntimeInMillis;
Long nextRunTimeInMillis;
String taskId;
}

enum RunStatus{
Queued, Running, Success, Failed, Cancelled
}

enum Status{
Active, Finished, Cancelled
}

enum TaskType{
Scheduled, Recurring
}

Map<String, Task> tasks;
interface TaskService{
 void createTask(Task task);
 void updateTask(Task task);
 boolean cancelTask(String taskId);
 List<Task> getFutureTasks(); // return task ids to be executed and update the next run time , pick where next run time less than current + 15 sec
 void updateTaskExecutionTime(Long executionTimeInMillis);
}

TaskService taskService;
TaskOrchestrator taskOrchestrator;
interface Watcher{
 void run(); // creates next 5 task runs
}

// will run watcher and orchestrator in seperate thread from main class
Map<String, List<TaskRun>> taskRunsByTaskId;
Queue<TaskRun> taskQueue;
Executor executor;
TaskService taskService;
interface TaskOrchestrator{

void run(); // createsTaskRun, cals executor, update run status, last updated time for the task (pick ones with runtime less than current)

}

TaskService taskService;
interface Executor{
Status execute();
}

// how do we handle multiple recurring time 
ans keep intervalTimeInmillis as list and queue based on that and update nextRunTime

Still need to handle concurrent task execution, cancelling live running task









