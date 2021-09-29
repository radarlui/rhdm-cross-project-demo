# How to improve performance of complicated project in Red Hat Decision Manager (Drools)
When implementing a complicated rule flow based project using Red Hat Decision Manager or Drools, there may be some performance issues if the number of rules in a project is large. The following is a list of some common issues found.
- Long time to open an asset in the Business Central
- Long time to build the project
- High resource (CPU, memory) consumptions

In order to solve the above performance issues, it is recommened to reduce the number of rules in each project. Therefore breaking down a complicated project into multiple projects can improve the performance. However, there is no clear documents on how to call a sub-process in another project. This demo servers the purpose showing how to break down a long rule flow process into shorter ones in different and integrate them to work together.

In this demo, there are two projects. One is parent-project and the other is child-project. In order for the rule flow process in parent-project to call the sub-process in child-project, the following settings have to be configured in the project settings of parent-project.

### Project Dependecies
Add child-project as the project dependency of the parent-project.
![image](https://user-images.githubusercontent.com/8802830/135217709-46b57a97-fb19-43d7-b16e-daccc6345c95.png)

## KIE bases
Include the KIE base of the child-project in the parent-project.
![image](https://user-images.githubusercontent.com/8802830/135217897-3a6a5f4f-f0f3-4263-9b4d-f1249416af9f.png)

Then you can call the sub-project in the child-project from the parent-project using reusable subprocess component.
![image](https://user-images.githubusercontent.com/8802830/135218424-7deb0c1e-dda7-438c-8e7f-930e96effea4.png)

### Procedures to run the demo
1. Download and install Red Hat Decision Manager (RHDM)
2. Start the RHDM
3. Open and login Decision Central (e.g. http://localhost:8080/decision-central)
4. Add a new Space (e.g. DemoSpace)
5. Import parent-project with Repository URL at https://github.com/radarlui/parent-project.git
6. Import child-project with Repository URL at https://github.com/radarlui/child-project.git
7. Build both projects in Decision Central
8. Deploy both projects using KIE Server Swagger UI at http://localhost:8080/kie-server/docs or using CURL: 
```
curl -X PUT "http://localhost:8080/kie-server/services/rest/server/containers/parent-project_1.0.0-SNAPSHOT" -H "accept: application/json" -H "content-type: application/json" -d "{ \"container-id\" : \"parent-project_1.0.0-SNAPSHOT\", \"release-id\" : { \"group-id\" : \"com.demospace\", \"artifact-id\" : \"parent-project\", \"version\" : \"1.0.0-SNAPSHOT\" }}"
```
```
curl -X PUT "http://localhost:8080/kie-server/services/rest/server/containers/child-project_1.0.0-SNAPSHOT" -H "accept: application/json" -H "content-type: application/json" -d "{ \"container-id\" : \"child-project_1.0.0-SNAPSHOT\", \"release-id\" : { \"group-id\" : \"com.demospace\", \"artifact-id\" : \"child-project\", \"version\" : \"1.0.0-SNAPSHOT\" }}"
```
![image](https://user-images.githubusercontent.com/8802830/135237444-e88a2a98-bd25-431b-a467-0bc33a363b45.png)

9. Execute the rule flow process by calling the API POST /server/containers/instances/{containerId} with containerId parent-project_1.0.0-SNAPSHOT and the following json body
```
{
  "lookup": "parent-project-sl-ksession",
  "commands": [
    {
         "insert": {
            "object": {
               "com.demospace.TestDO": {
                         "testValue1": 1

               }
            },
            "out-identifier":"do"
         }
    },
    {

      "start-process": {
        "processId": "parent-project.parent-process",
        "data":     null,
        "parameter":  [

        ],
        "out-identifier": "procId"
      }
    }
  ]
}
```
![image](https://user-images.githubusercontent.com/8802830/135241222-3bb9a4d7-8080-40f6-82f4-7a74c1a081e6.png)

![image](https://user-images.githubusercontent.com/8802830/135241277-f013a5b2-b1f7-41c9-9293-0c88491ad736.png)



10. You can see the resultValue in the TestDO has a value which is resulted from executed the rules from both parent-project and child-project




