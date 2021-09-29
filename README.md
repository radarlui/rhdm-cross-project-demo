# How to improve performance of complicated project in Red Hat Decision Manager (Drools)
When implementing a complicated rule flow based project using Red Hat Decision Manager or Drools, there may be some performance issues if the number of rules in a project is large. The following is a list of some common issues found.
- Long time to open an asset in the Business Central
- Long time to build the project
- High resource (CPU, memory) consumptions

In order to solve the above performance issues, it is recommened to reduce the number of rules in each project. Therefore breaking down a complicated project into multiple projects can improve the performance. However, there is no clear documents on how to call a sub-process in another project. This demo servers the purpose showing how to break down a long rule flow process into shorter ones in different and integrate them to work together.

In this demo, there are two projects. One is parent-project and the other is child-project. In order for the rule flow process in parent-project to call the sub-process in child-project, the following settings have to be configured in the project settings of parent-project.

## Project Dependecies
Add child-project as the project dependency of the parent-project.
![image](https://user-images.githubusercontent.com/8802830/135217709-46b57a97-fb19-43d7-b16e-daccc6345c95.png)

## KIE bases
Include the KIE base of the child-project in the parent-project.
![image](https://user-images.githubusercontent.com/8802830/135217897-3a6a5f4f-f0f3-4263-9b4d-f1249416af9f.png)

Then you can call the sub-project in the child-project from the parent-project using reusable subprocess component.
![image](https://user-images.githubusercontent.com/8802830/135218424-7deb0c1e-dda7-438c-8e7f-930e96effea4.png)

