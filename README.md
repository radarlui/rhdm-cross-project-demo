# rhdm-cross-project-demo
When implementing a complicated rule flow based project using Red Hat Decision Manager or Drools, there may be some performance issues if the number of rules in a project is large. The following is a list of some common issues found.
- Long time to open an asset in the Business Central
- Long time to build the project
- High resource (CPU, memory) consumptions

In order to solve the above performance issues, it is recommened to reduce the number of rules in each project. Therefore breaking down a complicated project into multiple projects can improve the performance. However, there is no clear documents on how to call a sub-process in another project. This demo servers the purpose showing how to break down a long rule flow process into shorter ones in different and integrate them to work together.

In this demo, there are two projects. One is parent-project and the other is child-project. In order for the rule flow process in parent-project to call the sub-process in child-project, the following settings have to be configured in the project settings of parent-project.
