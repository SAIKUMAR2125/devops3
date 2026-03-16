# devops3
devops new project
import 'package:flutter/material.dart';

void main() {
  runApp(TaskApp());
}

class TaskApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Task Manager',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: TaskHomePage(),
    );
  }
}

class Task {
  String title;
  String description;
  bool completed;

  Task({required this.title, required this.description, this.completed = false});
}

class TaskHomePage extends StatefulWidget {
  @override
  _TaskHomePageState createState() => _TaskHomePageState();
}

class _TaskHomePageState extends State<TaskHomePage> {
  List<Task> tasks = [];

  void addTask(String title, String description) {
    setState(() {
      tasks.add(Task(title: title, description: description));
    });
  }

  void toggleTask(int index) {
    setState(() {
      tasks[index].completed = !tasks[index].completed;
    });
  }

  void deleteTask(int index) {
    setState(() {
      tasks.removeAt(index);
    });
  }

  void openAddTaskScreen() {
    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => AddTaskScreen(addTask: addTask),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Task Manager"),
      ),
      body: tasks.isEmpty
          ? Center(child: Text("No tasks added"))
          : ListView.builder(
              itemCount: tasks.length,
              itemBuilder: (context, index) {
                return Card(
                  margin: EdgeInsets.all(10),
                  child: ListTile(
                    leading: Checkbox(
                      value: tasks[index].completed,
                      onChanged: (value) {
                        toggleTask(index);
                      },
                    ),
                    title: Text(
                      tasks[index].title,
                      style: TextStyle(
                        decoration: tasks[index].completed
                            ? TextDecoration.lineThrough
                            : TextDecoration.none,
                      ),
                    ),
                    subtitle: Text(tasks[index].description),
                    trailing: IconButton(
                      icon: Icon(Icons.delete, color: Colors.red),
                      onPressed: () {
                        deleteTask(index);
                      },
                    ),
                  ),
                );
              },
            ),
      floatingActionButton: FloatingActionButton(
        onPressed: openAddTaskScreen,
        child: Icon(Icons.add),
      ),
    );
  }
}

class AddTaskScreen extends StatefulWidget {
  final Function(String, String) addTask;

  AddTaskScreen({required this.addTask});

  @override
  _AddTaskScreenState createState() => _AddTaskScreenState();
}

class _AddTaskScreenState extends State<AddTaskScreen> {
  final TextEditingController titleController = TextEditingController();
  final TextEditingController descriptionController = TextEditingController();

  void submitTask() {
    if (titleController.text.isEmpty || descriptionController.text.isEmpty) {
      return;
    }

    widget.addTask(titleController.text, descriptionController.text);

    Navigator.pop(context);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Add Task"),
      ),
      body: Padding(
        padding: EdgeInsets.all(20),
        child: Column(
          children: [
            TextField(
              controller: titleController,
              decoration: InputDecoration(
                labelText: "Task Title",
                border: OutlineInputBorder(),
              ),
            ),
            SizedBox(height: 20),
            TextField(
              controller: descriptionController,
              decoration: InputDecoration(
                labelText: "Task Description",
                border: OutlineInputBorder(),
              ),
            ),
            SizedBox(height: 30),
            ElevatedButton(
              onPressed: submitTask,
              child: Text("Add Task"),
            )
          ],
        ),
      ),
    );
  }
}
