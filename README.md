# Test Maven project with 3 modules

When adding or removing modules in the top pom.xml, the JAVA DEPENDENCIES which depends on Language Support for Java(TM) by Red Hat(redhat.java) is not updated even after VSCode is restarted.


##### Environment
- Operating System: Windows 10 Pro 1809 OS build 17763.379
- JDK version: Java HotSpot(TM) 64-Bit Server VM (build 25.172-b11, mixed mode)
- Visual Studio Code version: 1.32.3(user setup)
- Java extension version: Language Support for Java(TM) by Red Hat(redhat.java) 0.41.0, Java Dependency Viewer(vscjava.vscode-java-dependency) 0.3.0

##### Steps To Reproduce
1. Manually create a maven project with several sub modules. Each sub module has its own folder and src/main/java inside its own folder.
2. Edit the parent pom.xml, to add or remove modules. There is no change in the JAVA DEPENDENCIES. Even do `Update project configuration` to any pom.xml has no effect.
3. In src/main/java of the sub modules, create empty class with main method. Open the class in the VSCode, if a module does not appears in the JAVA DEPENDENCIES, no "Run | Debug" line appears on the top of main method.
4. However, by searching the opened workspace in workspace storage(%APPDATA%\Code[ - Variant]\User\workspaceStorage\), like(I have grep.exe on my Windows):  
    `grep -e "test-vscode-maven" %APPDATA%\Code\User\workspaceStorage\* -r`  
   There would be some results like:  
   `.......AppData\Roaming\Code\User\workspaceStorage\e8caf83cfcdbff47843f66c1e0d4be69/workspace.json:  "folder": "...........test-vscode-maven/module3"`  
   And manually remove ".......AppData\Roaming\Code\User\workspaceStorage\e8caf83cfcdbff47843f66c1e0d4be69\redhat.java" folder,   
   Then restart VSCode, the problem solved.
5. From the [attach logs](https://github.com/redhat-developer/vscode-java/wiki/Troubleshooting#enable-logging), do `Java: Clean the Java language server workspace` could also solve the problem, I find this when I'm going to commit this bug. It's still a reproducible problem.

[attach a sample project reproducing the error]  
https://github.com/jchprj/test-vscode-maven


##### Current Result
There is no change in the JAVA DEPENDENCIES. 
##### Expected Result
There should be changes corresponding the modules in parent pom.xml.
##### Additional Informations
