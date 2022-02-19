# Compiler Extra Project

✍🏽 Mohammad Amin Ghasvari

# 🤔 Overview

There are two directories Music player and Task manager. They are android applications that were developed using java programming language by me.

We use these projects to test our software. First, you should install the path library. This library helps us to find all java files.

Then we using the listener to find all of the class and their details.

# 💥 Summary

We define some classes to store the details about classes and methods.

```python
class JavaClass:

    def __init__(self):
        self.name = ""
        self.num_of_fields = 0
        self.num_of_private_fields = 0
        self.num_of_public_fields = 0
        self.methods = []

class JavaMethod:

    def __init__(self):
        self.name = ""
        self.fan_in = 0
        self.fan_out = 0
```

Here is the main code for our listener. I use stack to handle multiple classes in a file scenario.

```python
class ClassInfoListener(JavaParserLabeledListener):

    def __init__(self):
        self.classes = []
        self.this_class = None
        self.class_stack = []

        self.this_method = None
        self.method_stack = []

        self.fan_in = {}

    def enterClassDeclaration(self, ctx: JavaParserLabeled.ClassDeclarationContext):
        if self.this_class is not None:
            self.class_stack.append(self.this_class)
        self.this_class = JavaClass()
        self.this_class.name = ctx.IDENTIFIER().getText()

    def exitFieldDeclaration(self, ctx: JavaParserLabeled.FieldDeclarationContext):
        self.this_class.num_of_fields += 1
        if ctx.parentCtx.parentCtx.children[0].children[0].start.text == "public":
            self.this_class.num_of_public_fields += 1
        elif ctx.parentCtx.parentCtx.children[0].children[0].start.text == "private":
            self.this_class.num_of_private_fields += 1

    def enterMethodDeclaration(self, ctx: JavaParserLabeled.MethodDeclarationContext):
        if self.this_method is not None:
            self.method_stack.append(self.this_method)
        self.this_method = JavaMethod()
        self.this_method.name = ctx.IDENTIFIER()

    def exitMethodCall0(self, ctx: JavaParserLabeled.MethodCall0Context):
        method_name = ctx.children[0].getText()
        if method_name in self.fan_in.keys():
            self.fan_in[method_name] += 1
        else:
            self.fan_in[method_name] = 1

        if self.this_method is not None:
            self.this_method.fan_out += 1

    def exitMethodDeclaration(self, ctx: JavaParserLabeled.MethodDeclarationContext):
        self.this_class.methods.append(self.this_method)
        if len(self.method_stack) != 0:
            self.this_method = self.method_stack[-1]
            self.method_stack = self.method_stack[:-1]
        else:
            self.this_method = None

    def exitClassDeclaration(self, ctx: JavaParserLabeled.ClassDeclarationContext):
        for m in self.this_class.methods:
            if str(m.name) in self.fan_in.keys():
                m.fan_in = self.fan_in[str(m.name)]

        self.classes.append(self.this_class)
        if len(self.class_stack) != 0:
            self.this_class = self.class_stack[-1]
            self.class_stack = self.class_stack[:-1]
        else:
            self.this_class = None
```

# ✅ Result

Finally, the result will be stored in *.log files.

For example for running this software for task management projects.

```python
...
TaskManagerApp = "TaskManagerApp"
MusicPlayer = "MusicPlayer"

def main():
    project = TaskManagerApp 

    # Python version should be above 3.4 !
    java_files = list(Path("./{}".format(project)).rglob("*.[jJ][aA][vV][aA]"))

    classes = []

    # log string
    log = ""

    for file in java_files:
        file_path = str(file.absolute())
....
```

Here is the log file:

```python
-----------------------------------------
C:\Users\Amin\MAG\_term_6\Compiler\iust-compiler992\TaskManagerApp\app\src\main\java\com\example\caspian\taskmanager\view\DialogFragmentEdit.java
1	Classes:
Class: DialogFragmentEdit
13	Fields 
	3	Public Fields 
	10	Private Fields 
11	Methods 
	newInstance
		Fan In: 1
		Fan Out: 2
	onCreate
		Fan In: 1
		Fan Out: 9
	onClick
		Fan In: 0
		Fan Out: 5
	onClick
		Fan In: 0
		Fan Out: 10
	onClick
		Fan In: 0
		Fan Out: 17
	onClick
		Fan In: 0
		Fan Out: 2
	onCreateDialog
		Fan In: 0
		Fan Out: 26
	onResume
		Fan In: 1
		Fan Out: 7
	onActivityResult
		Fan In: 1
		Fan Out: 10
	getUriForFile
		Fan In: 3
		Fan Out: 2
	UpdatePhoto
		Fan In: 2
		Fan Out: 7

-----------------------------------------
C:\Users\Amin\MAG\_term_6\Compiler\iust-compiler992\TaskManagerApp\app\src\main\java\com\example\caspian\taskmanager\view\DialogFragmentSearch.java
3	Classes:
Class: Taskholder
4	Fields 
	1	Public Fields 
	3	Private Fields 
3	Methods 
	onClick
		Fan In: 0
		Fan Out: 4
	bind
		Fan In: 1
		Fan Out: 14
	bold
		Fan In: 2
		Fan Out: 6
Class: TaskAdapter
3	Fields 
	0	Public Fields 
	3	Private Fields 
5	Methods 
	onCreateViewHolder
		Fan In: 0
		Fan Out: 2
	onBindViewHolder
		Fan In: 0
		Fan Out: 2
	getItemCount
		Fan In: 0
		Fan Out: 1
	setTaskList
		Fan In: 1
		Fan Out: 0
	setText
		Fan In: 4
		Fan Out: 0
Class: DialogFragmentSearch
8	Fields 
	3	Public Fields 
	5	Private Fields 
7	Methods 
	newInstance
		Fan In: 1
		Fan Out: 1
	onCreate
		Fan In: 1
		Fan Out: 3
	beforeTextChanged
		Fan In: 0
		Fan Out: 2
	onTextChanged
		Fan In: 0
		Fan Out: 2
	afterTextChanged
		Fan In: 0
		Fan Out: 2
	onCreateView
		Fan In: 0
		Fan Out: 11
	updateUI
		Fan In: 3
		Fan Out: 6

```

Or if we want to test music player

```python
TaskManagerApp = "TaskManagerApp"
MusicPlayer = "MusicPlayer"

def main():
    project = MusicPlayer

    # Python version should be above 3.4 !
    java_files = list(Path("./{}".format(project)).rglob("*.[jJ][aA][vV][aA]"))

    classes = []

    # log string
    log = ""

    for file in java_files:
        file_path = str(file.absolute())
```

This is the result

```python
...
-----------------------------------------
C:\Users\Amin\MAG\_term_6\Compiler\iust-compiler992\MusicPlayer\app\src\main\java\com\mag\musicplayer\services\MusicPlayerService.java
1	Classes:
Class: MusicPlayerService
7	Fields 
	3	Public Fields 
	4	Private Fields 
3	Methods 
	onStartCommand
		Fan In: 0
		Fan Out: 9
	onBind
		Fan In: 0
		Fan Out: 0
	createActionsPI
		Fan In: 1
		Fan Out: 7

-----------------------------------------
C:\Users\Amin\MAG\_term_6\Compiler\iust-compiler992\MusicPlayer\app\src\main\java\com\mag\musicplayer\util\MusicUtil.java
1	Classes:
Class: MusicUtil
0	Fields 
	0	Public Fields 
	0	Private Fields 
1	Methods 
	getStringTime
		Fan In: 0
...
```